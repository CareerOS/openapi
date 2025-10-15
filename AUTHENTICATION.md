# CareerOS API Authentication Guide

This guide explains how authentication works with the CareerOS API and provides detailed examples for different authentication flows.

## Table of Contents

1. [Overview](#overview)
2. [Authentication Methods](#authentication-methods)
3. [JWT Bearer Token Authentication](#jwt-bearer-token-authentication)
4. [API Key Authentication](#api-key-authentication)

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

Contact CareerOS support at `oleksii@thecareeros.com` to request API credentials. You'll receive:
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
