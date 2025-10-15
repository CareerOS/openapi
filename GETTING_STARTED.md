# Getting Started with CareerOS API

Welcome to the CareerOS API! This comprehensive guide will help you quickly integrate with our platform and start building exceptional career development experiences.

## Table of Contents

1. [Overview](#overview)
2. [API Navigation Guide](#api-navigation-guide)
3. [Authentication](#authentication)
4. [Making Your First Request](#making-your-first-request)
5. [Feature Modules](#feature-modules)
6. [Common Use Cases](#common-use-cases)
7. [Code Examples](#code-examples)
8. [Rate Limits](#rate-limits)
9. [BETA Features & Feedback](#beta-features--feedback)
10. [Support](#support)

---

## Overview

The CareerOS API is a modern, RESTful API platform that empowers career centers, students, and partners to deliver exceptional career development experiences. Our API-first architecture provides comprehensive access to:

- **Student Profile Management** - Complete career journey tracking
- **Job & Company Discovery** - Advanced search with intelligent filtering
- **Resume OS** - AI-powered resume building and scoring
- **Application Tracking** - End-to-end application lifecycle management
- **Alumni Networking** - Connect students with successful alumni (AlumniOS)
- **Event Management** - Career fairs, workshops, and networking events (EventOS)
- **Appointment Scheduling** - Complete booking workflow (CalendarOS - BETA)
- **Internship Agreements** - Three-way agreement signing system (BETA)
- **Employer Registration** - Comprehensive employer onboarding (BETA)
- **Custom Fields** - Institution-specific data extensions (BETA)
- **Analytics & Reporting** - Data-driven insights and exports (PROPOSAL)

### API Versions

CareerOS provides multiple API versions optimized for different use cases:

- **Public API V1** (`/public/v1/*`) - Public endpoints for discovery and authentication (no auth required)
- **API V1** (`/api/v1/*`) - Main authenticated API for students and career advisors (100+ endpoints)
- **API V2** (`/api/v2/*`) - Enhanced career advisor tools with advanced analytics
- **Webhook API** (`/webhook/*`) - Endpoints for receiving webhooks from external services

---

## API Navigation Guide

### For Student Application Developers

**Start Here:**
1. `/api/v1/users/self` - Get current user profile
2. `/api/v1/jobs/search` - Search for jobs
3. `/api/v1/resume/base` - Resume management
4. `/api/v1/applications` - Application tracking
5. `/api/v1/events/search` - Event discovery (EventOS)
6. `/api/v1/alumn/*` - Alumni networking (AlumniOS)
7. `/api/v1/appointments` - Schedule advisor meetings (CalendarOS)

**Key Features:**
- Self-service career management
- Job and company research
- Resume building with AI scoring
- Application pipeline tracking
- Event registration
- Appointment booking

### For Career Advisor Applications

**Start Here:**
1. `/api/v2/university/students` - View your students
2. `/api/v2/university/students/{id}/summary` - AI-generated student insights
3. `/api/v2/university/analytics/*` - Cohort analytics
4. `/api/v1/appointments` - Manage student appointments (CalendarOS)
5. `/api/v1/reports/cohort` - Comprehensive cohort reports
6. `/api/v1/agreements/internship` - Manage internship agreements (BETA)

**Key Features:**
- Student progress monitoring
- Cohort-level analytics
- Appointment management with attendance tracking
- Comprehensive reporting with multi-format export
- Internship agreement workflow

### For University Administrators

**Start Here:**
1. `/api/v1/custom-fields` - Configure institution-specific fields (BETA)
2. `/api/v1/filterfields` - Manage dropdown values
3. `/api/v2/advisor/reports/cohort` - University-wide analytics
4. `/api/v1/employers/register` - Employer onboarding (BETA)
5. `/api/v1/agreements/internship` - Agreement management (BETA)

**Key Features:**
- Custom field definitions for all entities
- Dropdown value management
- Employer registration workflow
- Multi-party agreement system
- Advanced reporting capabilities

### For Integration Partners

**Start Here:**
1. Authentication - Obtain API keys
2. `/webhook/documents/scan-results` - Receive scan results
3. Employer APIs - Job posting integration
4. Public APIs - Discovery without authentication

---

### Base URLs

- **Production**: `https://api.thecareeros.com`
- **Development**: `https://api-dev.thecareeros.com`

---

## Authentication

CareerOS uses **JWT Bearer token authentication** powered by Auth0.

### Getting Your Access Token

Most CareerOS API endpoints require authentication. You'll need to obtain a JWT access token through your university's authentication provider.

#### Authentication Flow

1. **User logs in** through their university's identity provider (SAML, OAuth, etc.)
2. **CareerOS validates** the user's identity
3. **JWT token is issued** with appropriate permissions
4. **Include token** in all API requests

#### Using Your Token

Include your access token in the `Authorization` header of every request:

```bash
Authorization: Bearer <your-access-token>
```

**Example:**
```bash
curl -X GET "https://api.thecareeros.com/api/v1/users/self" \
  -H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9..."
```

### Token Expiration

- Access tokens expire after **24 hours**
- When a token expires, you'll receive a `401 Unauthorized` response
- Refresh your token by re-authenticating through the login flow

### API Key Authentication (Partners Only)

Integration partners can use API key authentication for certain endpoints:

```bash
X-API-Key: <your-api-key>
```

Contact CareerOS support to obtain API credentials for integration use cases.

---

## Making Your First Request

Let's make your first API call to retrieve your user profile.

### Step 1: Get Your Profile

```bash
curl -X GET "https://api.thecareeros.com/api/v1/users/self" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json"
```

**Response:**
```json
{
  "id": "123e4567-e89b-12d3-a456-426614174000",
  "email": "student@university.edu",
  "first_name": "Jane",
  "last_name": "Doe",
  "university": {
    "id": "uuid",
    "name": "University Name"
  },
  "cohort": {
    "id": "uuid",
    "name": "Class of 2025"
  },
  "created_at": "2024-09-01T00:00:00Z"
}
```

### Step 2: Search for Jobs

```bash
curl -X GET "https://api.thecareeros.com/api/v1/jobs/search?query=software+engineer&limit=10" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json"
```

**Response:**
```json
{
  "jobs": [
    {
      "id": "job-uuid",
      "title": "Software Engineer Intern",
      "company": {
        "id": "company-uuid",
        "name": "Tech Company Inc.",
        "logo_url": "https://..."
      },
      "location": "San Francisco, CA",
      "type": "internship",
      "posted_at": "2024-10-01T00:00:00Z"
    }
  ],
  "total": 150,
  "limit": 10,
  "offset": 0
}
```

### Step 3: Save a Job

```bash
curl -X POST "https://api.thecareeros.com/api/v1/jobs" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "job_id": "job-uuid"
  }'
```

---

## Feature Modules

CareerOS is organized into specialized modules, each providing a complete solution for specific aspects of career development:

### Resume OS (`/api/v1/resume/*`)
**Complete resume lifecycle management**

Build, score, and improve resumes with our AI-powered system:
- Multiple professional templates
- Structured content management
- AI-powered scoring with action verb suggestions
- Peer and advisor review workflow
- Version control and copying
- Export in multiple formats

```bash
# Create new resume
POST /api/v1/resume/base/empty

# Update content
PUT /api/v1/resume/base/{id}/content

# Get AI score
GET /api/v1/resume/score/{id}

# Request review
POST /api/v1/resume/review
```

### EventOS (`/api/v1/events/*`)
**Career event discovery and management**

Complete event lifecycle from discovery to attendance:
- Search career fairs, workshops, networking events
- Event details with speaker information
- Registration and application management
- Calendar integration (ICS download)
- Cancellation handling

```bash
# Search events
GET /api/v1/events/search?type=career_fair

# Get event details
GET /api/v1/events/{eventId}

# Register for event
POST /api/v1/events/{eventId}/apply

# Download calendar invite
GET /api/v1/events/{eventId}/download-ics
```

### AlumniOS (`/api/v1/alumn/*`)
**Alumni networking and mentorship**

Connect current students with successful alumni:
- Search alumni by cohort or university
- Alumni profiles with career paths
- Contact information and networking
- Mentorship opportunities

```bash
# Get alumni by cohort
GET /api/v1/alumn/cohort/{cohortID}

# Get alumni by university
GET /api/v1/alumn/university/{universityID}

# Get alumni profile
GET /api/v1/alumn/{id}
```

### CalendarOS (BETA) (`/api/v1/appointments/*`)
**Appointment scheduling and calendar management**

Complete appointment booking workflow with confirmation and attendance tracking:

**For Students:**
- Search advisor availability
- Book appointments (creates pending status)
- Receive confirmation notifications
- View upcoming and past appointments

**For Advisors:**
- Set recurring availability schedules
- Confirm pending bookings
- Mark attendance after appointments
- Track outcomes and action items
- Configure appointment types (counseling, resume review, mock interview, etc.)

```bash
# Check availability
GET /api/v1/appointments/availability?advisor_id={id}&date={date}

# Book appointment (student)
POST /api/v1/appointments
{
  "advisor_id": "uuid",
  "appointment_type": "resume_review",
  "date": "2025-10-20",
  "time": "14:00"
}

# Confirm booking (advisor)
POST /api/v1/appointments/{id}/confirm

# Mark attendance (advisor)
POST /api/v1/appointments/{id}/mark-attendance
{
  "attended": true,
  "outcomes": "Reviewed resume, discussed interview prep",
  "action_items": ["Update work experience", "Practice behavioral questions"]
}
```

### Agreements API (BETA) (`/api/v1/agreements/internship/*`)
**Three-way internship agreement management**

Complete workflow for creating, signing, and managing internship agreements between students, employers, and universities:

**Key Features:**
- Draft agreement creation with internship details
- Electronic signature workflow for all three parties
- Signature status tracking and audit trail
- Notification system for all parties
- Modification with signature reset
- Document attachment support

**Agreement Statuses:**
- `draft` - Being prepared
- `pending_signatures` - Sent to parties
- `partially_signed` - One or more parties signed
- `fully_signed` - All parties signed
- `active` - Internship in progress
- `completed` - Internship completed

```bash
# Create agreement
POST /api/v1/agreements/internship
{
  "student_id": "uuid",
  "employer_id": "uuid",
  "position_title": "Software Engineering Intern",
  "start_date": "2025-06-01",
  "end_date": "2025-08-31",
  "compensation": {
    "type": "paid",
    "amount": 25,
    "currency": "USD",
    "period": "hourly"
  }
}

# Sign agreement
POST /api/v1/agreements/internship/{id}/sign
{
  "party": "student",
  "signature_acceptance": true,
  "signature_name": "Jane Doe",
  "signature_method": "electronic"
}

# Send notification
POST /api/v1/agreements/internship/{id}/notify
{
  "notification_type": "signature_request",
  "recipients": ["employer"]
}

# Get signature status
GET /api/v1/agreements/internship/{id}/signatures
```

### Employers API (BETA) (`/api/v1/employers/*`)
**Employer registration and organization management**

Complete employer onboarding and management system:

**Registration Flow:**
- Public registration endpoint (no auth required)
- Company profile creation
- Primary contact setup
- University partnership selection
- Email verification and activation

**Profile Management:**
- Company information and branding
- Multiple office locations
- Culture values and benefits
- Diversity statements
- Activity statistics

**User Management:**
- Multi-user access with roles (admin, recruiter, hiring_manager, viewer)
- Granular permissions per user
- Invitation workflow
- Cannot remove last admin (safeguard)

**Settings & Integrations:**
- Notification preferences
- Application settings
- ATS integration (Greenhouse, Lever, Workday, Taleo)
- Calendar integration (Google, Outlook)
- Privacy controls
- Partnership preferences

```bash
# Register employer (public endpoint)
POST /api/v1/employers/register
{
  "company_name": "Tech Corp",
  "primary_contact": {
    "first_name": "John",
    "last_name": "Smith",
    "email": "john@techcorp.com",
    "title": "Campus Recruiter"
  },
  "headquarters": {
    "city": "San Francisco",
    "state": "CA"
  }
}

# Update profile (authenticated)
PUT /api/v1/employers/profile
{
  "description": "Leading tech company...",
  "website": "https://techcorp.com",
  "logo_url": "https://..."
}

# Add team member
POST /api/v1/employers/users
{
  "email": "recruiter@techcorp.com",
  "role": "recruiter",
  "permissions": {
    "can_post_jobs": true,
    "can_view_applications": true
  }
}

# Configure settings
PUT /api/v1/employers/settings
{
  "notifications": {
    "new_applications": true
  },
  "integrations": {
    "ats_integration": {
      "enabled": true,
      "provider": "greenhouse"
    }
  }
}
```

### Custom Fields (BETA) - Dynamic & Flexible
**Zero-configuration custom data for all entities**

CareerOS supports **arbitrary custom fields** on all major entities without requiring pre-definition!

**How It Works:**
Simply include any key-value pair in your API requests. Fields not part of the standard schema are automatically stored and returned in the `custom_fields` object.

**Supported Entities (11 modules):**
- Students
- Jobs
- Companies
- Applications
- Events (EventOS)
- Contacts
- Internship Agreements
- Employers
- Appointments (CalendarOS)
- Resumes (ResumeOS)
- Alumni (AlumniOS)

**Example - It Just Works:**
```bash
# Send custom fields in any request
PUT /api/v1/users/self/profile
{
  "first_name": "Maria",
  "last_name": "Garcia",
  "email": "maria@university.edu",
  "program_track": "mba_fulltime",
  "preferred_industry": "consulting",
  "work_authorization": "us_citizen",
  "career_interests": ["strategy", "innovation"]
}

# Response includes custom fields
{
  "id": "uuid",
  "first_name": "Maria",
  "last_name": "Garcia",
  "email": "maria@university.edu",
  "custom_fields": {
    "program_track": "mba_fulltime",
    "preferred_industry": "consulting",
    "work_authorization": "us_citizen",
    "career_interests": ["strategy", "innovation"]
  }
}

# Works for ALL entities
POST /api/v1/applications
{
  "job_id": "uuid",
  "status": "applied",
  "application_source": "career_fair",
  "notes_from_recruiter": "Strong candidate",
  "internal_rating": 4.5
}
```

**Optional Metadata API (`/api/v1/custom-fields/*`):**

When you need UI forms, validation, or dropdown management, use the optional metadata API:

```bash
# Define metadata for a custom field (optional)
POST /api/v1/custom-fields
{
  "entity_type": "student",
  "field_name": "preferred_industry",
  "display_label": "Preferred Industry",
  "field_type": "single_select",
  "help_text": "Select your preferred industry",
  "is_searchable": true,
  "is_visible_to_students": true
}

# Get metadata definitions
GET /api/v1/custom-fields?entity_type=student&status=active

# Update metadata
PUT /api/v1/custom-fields/{id}
{
  "display_label": "Industry Preference",
  "help_text": "Choose your target industry"
}
```

**Use Metadata API When You Need:**
- ✅ UI form labels and help text
- ✅ Validation rules (required, min/max, patterns)
- ✅ Field type definitions
- ✅ Visibility controls
- ✅ Search/filter configuration

**Skip Metadata API If:**
- ✅ You just want to store arbitrary key-value data (it works automatically!)
- ✅ You're handling validation in your application layer
- ✅ You don't need UI form definitions

### Filter Fields API (`/api/v1/filterfields`)
**Standardized dropdown values across all entities**

Retrieve all available filter options (dropdown values) for consistent data entry and powerful filtering:

**Categories:**
- **Companies** - locations, sizes, tags/industries
- **Jobs** - locations, types, experience levels
- **Events** - types, formats (virtual/in-person)
- **Students** - class years, student levels, majors, minors, GPA ranges, work authorization
- **Applications** - statuses, sources
- **Internships** - agreement statuses, compensation types, work locations
- **Custom Fields** - Institution-defined dropdown values

```bash
# Get all filter fields
GET /api/v1/filterfields?category=all&include_counts=true

# Get student-specific fields
GET /api/v1/filterfields?category=students

# Response includes counts
{
  "students": {
    "student_levels": [
      {"id": "...", "name": "Junior", "value": "junior", "count": 450},
      {"id": "...", "name": "Senior", "value": "senior", "count": 380}
    ],
    "work_authorization": [
      {"id": "...", "name": "US Citizen", "value": "us_citizen", "count": 820},
      {"id": "...", "name": "F1 Visa", "value": "f1_visa", "count": 245}
    ]
  }
}
```

### Reports API (PROPOSAL) (`/api/v1/reports/*`)
**Comprehensive analytics and reporting**

> **Note:** The Reports API is currently a PROPOSAL. We're actively seeking feedback from partners to refine these endpoints before finalization. Please share your requirements and suggestions!

**Student Reports:**
- Activity reports with timeline visualization
- Progress reports with career readiness tracking
- Multi-format export (JSON, CSV, PDF)

**Advisor Reports:**
- Cohort-level analytics
- Engagement metrics
- Application statistics
- Networking and resume metrics
- Top companies analysis

```bash
# Student activity report
GET /api/v1/reports/student/activity?student_id={id}&format=pdf

# Student progress report
GET /api/v1/reports/student/progress?student_id={id}

# Cohort analytics
GET /api/v1/reports/cohort?cohort_ids={id1,id2}&format=csv&from_date=2025-01-01
```

---

## Common Use Cases

### For Students

#### 1. Search and Save Jobs
```bash
# Search for jobs
GET /api/v1/jobs/search?query=marketing&location=New+York

# Save a job
POST /api/v1/jobs
{"job_id": "uuid"}

# Get saved jobs
GET /api/v1/users/self/jobs
```

#### 2. Build a Resume
```bash
# Create empty resume
POST /api/v1/resume/base/empty
{"title": "My Resume", "template_id": "uuid"}

# Update resume content
PUT /api/v1/resume/base/{id}/content
{
  "personal_info": {...},
  "work_experience": [...],
  "education": [...]
}

# Get resume score
GET /api/v1/resume/score/{id}
```

#### 3. Register for Events
```bash
# Search events
GET /api/v1/events/search?type=career_fair

# Apply to event
POST /api/v1/events/{eventId}/apply

# Download calendar invite
GET /api/v1/events/{eventId}/download-ics
```

#### 4. Track Applications
```bash
# Get all applications
GET /api/v2/applications?status=applied

# View application details
GET /api/v1/applications/{id}
```

### For Career Advisors

#### 1. View Student Progress
```bash
# Get students in your cohorts
GET /api/v2/university/students?cohort_ids=uuid1,uuid2

# Get student summary
GET /api/v2/university/students/{id}/summary

# View student's contacts
GET /api/v2/university/students/{id}/contacts
```

#### 2. Access Analytics
```bash
# Networking analytics
GET /api/v2/university/analytics/networking

# Top companies students target
GET /api/v2/university/analytics/companies/top
```

#### 3. Manage Appointments
```bash
# View your availability
GET /api/v1/advisor/appointments/availability

# Get upcoming appointments
GET /api/v1/advisor/appointments?status=scheduled

# Add notes after appointment
POST /api/v2/university/student/{id}/notes
```

---

## Code Examples

### JavaScript/TypeScript (Node.js)

```javascript
const fetch = require('node-fetch');

const API_BASE = 'https://api.thecareeros.com';
const ACCESS_TOKEN = 'your-access-token';

async function getUserProfile() {
  const response = await fetch(`${API_BASE}/api/v1/users/self`, {
    headers: {
      'Authorization': `Bearer ${ACCESS_TOKEN}`,
      'Content-Type': 'application/json'
    }
  });

  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }

  const profile = await response.json();
  return profile;
}

async function searchJobs(query, limit = 20) {
  const params = new URLSearchParams({ query, limit });
  const response = await fetch(`${API_BASE}/api/v1/jobs/search?${params}`, {
    headers: {
      'Authorization': `Bearer ${ACCESS_TOKEN}`,
      'Content-Type': 'application/json'
    }
  });

  const data = await response.json();
  return data;
}

// Usage
getUserProfile()
  .then(profile => console.log('Profile:', profile))
  .catch(error => console.error('Error:', error));
```

### Python

```python
import requests

API_BASE = 'https://api.thecareeros.com'
ACCESS_TOKEN = 'your-access-token'

headers = {
    'Authorization': f'Bearer {ACCESS_TOKEN}',
    'Content-Type': 'application/json'
}

def get_user_profile():
    response = requests.get(f'{API_BASE}/api/v1/users/self', headers=headers)
    response.raise_for_status()
    return response.json()

def search_jobs(query, limit=20):
    params = {'query': query, 'limit': limit}
    response = requests.get(
        f'{API_BASE}/api/v1/jobs/search',
        headers=headers,
        params=params
    )
    response.raise_for_status()
    return response.json()

# Usage
try:
    profile = get_user_profile()
    print(f"Profile: {profile}")

    jobs = search_jobs('software engineer')
    print(f"Found {jobs['total']} jobs")
except requests.exceptions.HTTPError as e:
    print(f"Error: {e}")
```

### cURL

```bash
#!/bin/bash

API_BASE="https://api.thecareeros.com"
ACCESS_TOKEN="your-access-token"

# Get user profile
curl -X GET "$API_BASE/api/v1/users/self" \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json"

# Search jobs
curl -X GET "$API_BASE/api/v1/jobs/search?query=engineer&limit=10" \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json"

# Save a job
curl -X POST "$API_BASE/api/v1/jobs" \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"job_id": "uuid-here"}'
```

---

## Rate Limits

To ensure fair usage and system stability, the CareerOS API enforces rate limits:

| Endpoint Type | Rate Limit | Window |
|--------------|------------|--------|
| Public API | 100 requests | 1 minute |
| Authenticated API | 300 requests | 1 minute |
| Search endpoints | 60 requests | 1 minute |
| Advisor V2 API | 500 requests | 1 minute |
| Webhook endpoints | N/A | N/A |

### Rate Limit Headers

Every API response includes rate limit information:

```
X-RateLimit-Limit: 300
X-RateLimit-Remaining: 295
X-RateLimit-Reset: 1634567890
```

### Handling Rate Limits

When you exceed the rate limit, you'll receive a `429 Too Many Requests` response:

```json
{
  "error": "Rate limit exceeded",
  "retry_after": 60
}
```

**Best Practices:**
- Implement exponential backoff when receiving 429 responses
- Cache responses when possible
- Use webhooks instead of polling for real-time updates
- Batch requests when appropriate

---

## Error Handling

The CareerOS API uses standard HTTP status codes:

| Status Code | Meaning |
|------------|---------|
| 200 | Success |
| 201 | Created successfully |
| 400 | Bad request - check your request parameters |
| 401 | Unauthorized - invalid or expired token |
| 403 | Forbidden - insufficient permissions |
| 404 | Not found - resource doesn't exist |
| 422 | Validation error - request data is invalid |
| 429 | Rate limit exceeded |
| 500 | Internal server error |

### Error Response Format

```json
{
  "error": "Validation failed",
  "details": [
    {
      "field": "email",
      "message": "Invalid email format"
    }
  ]
}
```

---

## BETA Features & Feedback

CareerOS continuously evolves based on partner and user feedback. Several advanced features are currently in **BETA** status, and we actively welcome your input to refine them.

### What BETA Means

**BETA** features are:
- ✅ **Fully functional** and ready for integration
- ✅ **Production-ready** and thoroughly tested
- ✅ **Documented** with complete API specifications
- ✅ **Supported** by our team

However, **BETA features may evolve** based on partner feedback:
- API endpoints may be enhanced with additional fields
- Request/response structures may be refined
- New capabilities may be added
- Naming conventions may be adjusted for clarity

### Current BETA Features

#### CalendarOS - Appointment Scheduling (`/api/v1/appointments/*`)
Complete appointment booking workflow with confirmation and attendance tracking.

**Feedback Welcome:**
- Additional appointment types you need
- Integration requirements (Google Calendar, Outlook, etc.)
- Notification preferences
- Reporting needs

#### Agreements API (`/api/v1/agreements/internship/*`)
Three-way internship agreement management and electronic signing.

**Feedback Welcome:**
- Additional agreement types (employment, NDA, etc.)
- Signature methods and verification
- Document requirements
- Workflow customization

#### Employers API (`/api/v1/employers/*`)
Employer registration, profile management, and team collaboration.

**Feedback Welcome:**
- Additional ATS integrations needed
- Custom fields for employer profiles
- Permission models
- Reporting requirements

#### Custom Fields API (`/api/v1/custom-fields/*`)
Institution-specific field definitions and dropdown management.

**Feedback Welcome:**
- Additional field types
- Validation rule requirements
- Bulk operations
- Import/export capabilities

### Reports API (PROPOSAL)

The Reports API (`/api/v1/reports/*`) is currently a **PROPOSAL**, meaning we're actively designing it based on partner requirements.

**We Need Your Input:**
- What reports are most critical for your institution?
- What export formats do you require?
- What data points should be included?
- How should data be aggregated?
- What time ranges and filters are needed?

### How to Provide Feedback

We value your input and want to ensure our APIs meet your needs:

**For Technical Feedback:**
- Email: api-feedback@thecareeros.com
- Include: Feature name, specific feedback, use case description

**For Feature Requests:**
- Email: product@thecareeros.com
- Include: Detailed requirements, expected behavior, business value

**For Integration Support:**
- Email: api-support@thecareeros.com
- Include: Technical details, error messages, desired outcome

**Response Time:**
- Technical questions: Within 24 hours
- Feature feedback: Acknowledged within 48 hours
- Integration support: Within 12 hours (business days)

### Version Compatibility

**API Versioning Policy:**
- Core API V1 (`/api/v1/*`) - Stable, breaking changes announced 90 days in advance
- Advisor API V2 (`/api/v2/*`) - Stable, backward compatible additions only
- BETA features - May evolve based on feedback, changes announced 30 days in advance
- PROPOSAL features - Under active design, significant changes possible

**We Commit To:**
- Clear communication about changes
- Migration guides for breaking changes
- Deprecation notices with ample transition time
- Backward compatibility whenever possible

---

## Support

### Documentation
- **API Reference**: [OpenAPI Specification](./openapi.yaml)
- **Authentication Guide**: [AUTHENTICATION.md](./AUTHENTICATION.md)

### Contact
- **Email**: api-support@thecareeros.com
- **Website**: https://thecareeros.com
- **Status Page**: https://status.thecareeros.com

### Reporting Issues
For bugs or feature requests, contact your CareerOS account manager or email support.

---

## Next Steps

1. ✅ **Authenticate** and get your access token
2. ✅ **Make your first API call** to `/api/v1/users/self`
3. ✅ **Explore the API reference** for available endpoints
4. ✅ **Review code examples** for your programming language
5. ✅ **Join our developer community** for updates and support

Happy coding! 🚀
