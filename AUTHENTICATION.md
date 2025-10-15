# CareerOS API Authentication Guide

This guide explains how authentication works with the CareerOS API and provides detailed examples for different authentication flows.

## Table of Contents

1. [Overview](#overview)
2. [Authentication Methods](#authentication-methods)
3. [JWT Bearer Token Authentication](#jwt-bearer-token-authentication)
4. [API Key Authentication](#api-key-authentication)
5. [Token Management](#token-management)
6. [Permissions and Scopes](#permissions-and-scopes)
7. [Security Best Practices](#security-best-practices)
8. [Troubleshooting](#troubleshooting)

---

## Overview

CareerOS uses industry-standard authentication mechanisms to secure API access:

- **JWT Bearer Tokens** (Auth0) - For student and advisor applications
- **API Keys** - For integration partners and server-to-server communication

All authenticated endpoints require one of these authentication methods.

---

## Authentication Methods

### Method Comparison

| Method | Use Case | Token Format | Expiration |
|--------|----------|--------------|------------|
| JWT Bearer | Student/Advisor apps | `Bearer <token>` | 24 hours |
| API Key | Integration partners | `X-API-Key: <key>` | No expiration |

---

## JWT Bearer Token Authentication

### How It Works

1. **User authenticates** through their university's identity provider (SAML SSO, OAuth, etc.)
2. **CareerOS validates** the user's credentials with the university
3. **Auth0 issues** a JWT access token containing user identity and permissions
4. **Client includes token** in the `Authorization` header for all API requests
5. **CareerOS validates token** on each request and authorizes access

### University Login Flow

CareerOS supports multiple authentication providers per university:

#### Step 1: Get University Providers

```bash
GET /public/v1/universities/{universityID}/providers
```

**Response:**
```json
{
  "providers": [
    {
      "name": "saml",
      "display_name": "University Single Sign-On",
      "enabled": true
    },
    {
      "name": "google",
      "display_name": "Sign in with Google",
      "enabled": true
    }
  ]
}
```

#### Step 2: Initiate Authentication

For SAML/SSO:
```bash
GET /public/v1/universities/{universityID}/providers/saml
```

For Google OAuth:
```bash
GET /public/v1/oauth/google/start?university_id={universityID}
```

#### Step 3: Handle Callback

After successful authentication, the user is redirected to:
```
/public/v1/oauth/login/callback?code=<authorization_code>&state=<state>
```

The application exchanges the authorization code for an access token.

### Token Structure

JWT tokens contain:

```json
{
  "sub": "auth0|123456789",
  "iss": "https://auth.thecareeros.com/",
  "aud": "https://api.thecareeros.com",
  "iat": 1634567890,
  "exp": 1634654290,
  "scope": "read:profile write:resume",
  "university_id": "uuid",
  "cohort_id": "uuid",
  "role": "student"
}
```

### Using JWT Tokens

Include the token in the `Authorization` header:

```bash
curl -X GET "https://api.thecareeros.com/api/v1/users/self" \
  -H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiI..."
```

**JavaScript Example:**
```javascript
const accessToken = localStorage.getItem('access_token');

fetch('https://api.thecareeros.com/api/v1/users/self', {
  headers: {
    'Authorization': `Bearer ${accessToken}`,
    'Content-Type': 'application/json'
  }
})
.then(response => response.json())
.then(data => console.log(data));
```

**Python Example:**
```python
import requests

access_token = "your-jwt-token"
headers = {
    'Authorization': f'Bearer {access_token}',
    'Content-Type': 'application/json'
}

response = requests.get(
    'https://api.thecareeros.com/api/v1/users/self',
    headers=headers
)
profile = response.json()
```

---

## API Key Authentication

### For Integration Partners

API keys are issued to integration partners for server-to-server communication.

### Obtaining an API Key

Contact CareerOS support at `partnerships@thecareeros.com` to request API credentials. You'll receive:
- API Key
- Partner ID
- Allowed IP addresses (optional)

### Using API Keys

Include the API key in the `X-API-Key` header:

```bash
curl -X POST "https://api.thecareeros.com/integrations/jobs/sync" \
  -H "X-API-Key: your-api-key-here" \
  -H "Content-Type: application/json" \
  -d '{
    "jobs": [...]
  }'
```

### API Key Endpoints

The following endpoints support API key authentication:

- `/integrations/jobs/sync` - Sync job postings
- `/integrations/students/search` - Search student profiles
- `/integrations/employers/jobs` - Employer job management
- `/integrations/employers/events/search` - Event management
- `/webhook/*` - All webhook endpoints

### API Key Security

**Best Practices:**
- ✅ Store API keys in environment variables, never in code
- ✅ Use different keys for development and production
- ✅ Rotate keys periodically
- ✅ Restrict API key usage by IP address
- ✅ Monitor API key usage for suspicious activity

**Never:**
- ❌ Commit API keys to version control
- ❌ Share API keys in public channels
- ❌ Use production keys in development

---

## Token Management

### Token Expiration

JWT access tokens expire after **24 hours** for security.

### Detecting Expired Tokens

When a token expires, you'll receive:

```
HTTP/1.1 401 Unauthorized
```

```json
{
  "error": "Unauthorized",
  "message": "Token has expired"
}
```

### Refreshing Tokens

**Option 1: Silent Re-authentication**
- Use iframe-based silent authentication
- Auth0 session is maintained in cookies
- Obtain new token without user interaction

**Option 2: Prompt Re-login**
- Redirect user to login flow
- Better for security-sensitive operations

**Example (JavaScript):**
```javascript
async function makeAuthenticatedRequest(url, options = {}) {
  let token = localStorage.getItem('access_token');

  options.headers = {
    ...options.headers,
    'Authorization': `Bearer ${token}`,
    'Content-Type': 'application/json'
  };

  let response = await fetch(url, options);

  // If token expired, refresh and retry
  if (response.status === 401) {
    token = await refreshToken();
    localStorage.setItem('access_token', token);

    options.headers['Authorization'] = `Bearer ${token}`;
    response = await fetch(url, options);
  }

  return response;
}
```

### Logout

To logout, simply discard the access token:

```javascript
localStorage.removeItem('access_token');
// Optionally call logout endpoint
fetch('https://api.thecareeros.com/api/v1/logout', {
  method: 'POST',
  headers: {
    'Authorization': `Bearer ${token}`
  }
});
```

---

## Permissions and Scopes

### User Roles

CareerOS supports multiple user roles with different permission levels:

| Role | Description | Access Level |
|------|-------------|--------------|
| **student** | University student | Own profile, jobs, resumes, applications |
| **advisor** | Career advisor | Student data (assigned cohorts), analytics, appointments |
| **admin** | University admin | All university data, settings, user management |
| **partner** | Integration partner | API key access to integration endpoints |

### Role-Based Access

Endpoints enforce role-based access control:

#### Student Endpoints
- `/api/v1/users/self/*` - Own profile data
- `/api/v1/jobs/*` - Job search and saving
- `/api/v1/resume/*` - Resume management
- `/api/v1/applications/*` - Application tracking
- `/api/v1/contacts/*` - Contact management
- `/api/v1/events/*` - Event discovery

#### Advisor Endpoints
- `/api/v1/advisor/*` - Advisor-specific search
- `/api/v2/university/students` - Students in assigned cohorts
- `/api/v2/university/analytics/*` - Analytics dashboards
- `/api/v1/advisor/appointments/*` - Appointment management

### Permission Errors

When attempting to access unauthorized resources:

```
HTTP/1.1 403 Forbidden
```

```json
{
  "error": "Forbidden",
  "message": "You do not have permission to access this resource"
}
```

---

## Security Best Practices

### For Client Applications

1. **Store Tokens Securely**
   - Use httpOnly cookies for web apps (preferred)
   - Use secure storage for mobile apps (Keychain, Keystore)
   - Never store in localStorage if possible (XSS risk)

2. **Use HTTPS Only**
   - All API requests must use HTTPS
   - Never send tokens over HTTP

3. **Validate Token Before Use**
   - Check token expiration before making requests
   - Handle 401 responses gracefully

4. **Implement Token Refresh**
   - Refresh tokens before expiration
   - Don't wait for 401 responses

5. **Clear Tokens on Logout**
   - Remove all stored authentication data
   - Invalidate session server-side

### For Server Applications

1. **Secure API Key Storage**
   - Use environment variables
   - Use secret management services (AWS Secrets Manager, etc.)
   - Never hardcode keys

2. **Implement Rate Limiting**
   - Prevent abuse and DoS attacks
   - Track API usage per key

3. **IP Whitelisting**
   - Restrict API key usage to known IP addresses
   - Contact support to configure IP restrictions

4. **Monitor API Usage**
   - Set up alerts for unusual activity
   - Track failed authentication attempts

5. **Rotate Keys Regularly**
   - Rotate API keys every 90 days
   - Update keys immediately if compromised

---

## Troubleshooting

### Common Authentication Errors

#### 401 Unauthorized

**Possible Causes:**
- Token is expired
- Token is malformed
- Token signature is invalid
- Token is missing

**Solution:**
```bash
# Check token expiration
jwt_decode(token) # Should show exp claim

# Refresh token or re-authenticate
# Ensure Bearer prefix is included
Authorization: Bearer <token>
```

#### 403 Forbidden

**Possible Causes:**
- User doesn't have required role
- Accessing resource outside permission scope
- University/cohort restrictions

**Solution:**
- Verify user role matches required permissions
- Check that advisor has access to requested cohort
- Contact support if permissions are incorrect

#### 429 Too Many Requests

**Possible Causes:**
- Rate limit exceeded
- Too many failed authentication attempts

**Solution:**
```python
import time

response = requests.get(url, headers=headers)
if response.status_code == 429:
    retry_after = int(response.headers.get('Retry-After', 60))
    time.sleep(retry_after)
    response = requests.get(url, headers=headers)
```

### Testing Authentication

#### Test Token Validity

```bash
curl -X GET "https://api.thecareeros.com/api/v1/users/self" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -v
```

Look for:
- `200 OK` - Token is valid
- `401 Unauthorized` - Token is invalid/expired
- `403 Forbidden` - Token is valid but lacks permissions

#### Decode JWT (Debug Only)

**Never decode tokens in production - validate server-side**

```javascript
// For debugging only
const token = "eyJhbGciOiJSUzI1NiI...";
const payload = JSON.parse(atob(token.split('.')[1]));
console.log(payload);
```

---

## Support

For authentication issues:

- **Email**: auth-support@thecareeros.com
- **Documentation**: https://docs.thecareeros.com/authentication
- **Status Page**: https://status.thecareeros.com

For API key requests or issues:
- **Email**: partnerships@thecareeros.com
