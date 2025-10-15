# CareerOS API - A Modern Career Development Platform

## Overview

CareerOS provides a comprehensive API-first platform that empowers career centers, students, and partners to deliver exceptional career development experiences. Built on modern technology standards and designed with both students and advisors in mind, CareerOS offers a complete solution for career services management.

**Total Endpoints**: 100+

---

## 🎯 What's New

### Recently Added Features

#### CalendarOS API - Appointment Scheduling Module
Complete appointment booking workflow with confirmation and attendance tracking:
- Recurring availability schedules for advisors
- Student-initiated booking with pending approval
- Advisor confirmation workflow
- Attendance marking with outcomes tracking
- Multiple appointment types (counseling, resume review, mock interview, etc.)
- Meeting platform integration (Zoom, Teams, Google Meet, etc.)

#### Agreements API - Three-Way Agreement Management
Comprehensive internship agreement workflow:
- Draft agreement creation with internship details
- Electronic signature workflow for students, employers, and universities
- Signature status tracking and audit trail
- Notification system for all parties
- Modification with signature reset
- Complete document management

#### Employers API - Employer Onboarding & Management
Complete employer lifecycle management:
- Public registration endpoint (no auth required)
- Multi-user management with roles (admin, recruiter, hiring_manager, viewer)
- Granular permission controls
- ATS and calendar integrations
- Profile and settings management

#### Custom Fields API - Institution-Specific Extensions
Extend CareerOS with flexible custom field definitions:
- 10 field types (text, number, date, dropdowns, etc.)
- Dropdown value management
- 8 supported entities (students, jobs, companies, applications, events, contacts, agreements, employers)
- Visibility controls for students and employers
- Searchable and filterable fields
- Usage statistics and analytics

#### Reports API (PROPOSAL) - Comprehensive Analytics
Multi-format reporting with data-driven insights:
- Student activity and progress reports
- Cohort-level analytics
- Multi-format export (JSON, CSV, PDF)
- Timeline visualization
- Top companies analysis
- Engagement metrics tracking

---

## Core Strengths

### 1. Modern Technology Stack

CareerOS is built on industry-leading standards and modern architectural patterns:

- **OpenAPI 3.1.0 Specification** - Industry-standard API documentation
- **RESTful API Design** - Clean, predictable, and easy-to-use endpoints
- **JWT Authentication** - Secure, token-based authentication powered by Auth0
- **Real-time Capabilities** - WebSocket support for live notifications and updates
- **Clear API Versioning** - Multiple API versions supporting different use cases

### 2. Comprehensive Feature Set

#### Student-Centric Features
- **Resume OS** - Complete resume building and management system
  - Multiple resume templates
  - AI-powered resume scoring
  - Action verb suggestions
  - Peer and advisor review workflow

- **Job Discovery & Tracking**
  - Advanced job search with intelligent filtering
  - Save jobs and track application status
  - AI-powered job extraction from external sources
  - Application pipeline management

- **Professional Networking**
  - Contact management and relationship tracking
  - Direct messaging with recruiters and contacts
  - Alumni networking capabilities
  - Company research and engagement tracking

- **Career Progress Tracking**
  - Personal activity reports
  - Career development milestones
  - Skills assessment and recommendations
  - Self-service appointment scheduling

#### Advisor Tools
- **Student Management**
  - Comprehensive student profiles
  - AI-generated student summaries
  - Contact and networking visibility
  - Progress tracking across cohorts

- **Analytics & Reporting**
  - Cohort-level analytics
  - Engagement metrics (logins, session duration, active students)
  - Application statistics and trends
  - Networking and resume metrics
  - Top companies analysis

- **Appointment Management**
  - Flexible scheduling system
  - Availability management with recurring schedules
  - Multiple appointment types (counseling, resume review, mock interview, etc.)
  - Status tracking and notes

- **Data-Driven Insights**
  - Identify at-risk students
  - Track program effectiveness
  - Export data for further analysis

### 3. Professional Documentation

CareerOS provides extensive documentation to help developers integrate quickly and successfully:

- **Authentication Guide** - Detailed security and token management documentation
- **OpenAPI Specification** - Complete, machine-readable API documentation
- **Common Use Cases** - Real-world examples for both students and advisors

### 4. Integration-Friendly Design

Built for seamless integration with existing systems:

- **Webhook Support** - Real-time notifications for key events
- **Public APIs** - Discoverability without authentication
- **Partner API Keys** - Dedicated authentication for integration partners
- **Rate Limiting** - Fair usage policies clearly documented
- **Standard HTTP Status Codes** - Predictable error handling

---

## Key Features by Category

### Public APIs (`/public/v1/*`)
- University and cohort discovery
- Authentication provider lookup
- Company, job, and event browsing without login
- Filter fields and metadata

**10 public endpoints** for seamless integration

### User Management APIs (`/api/v1/users/*`)
- User profile management
- Self-service appointments
- Activity and progress reports
- University and cohort information

### Resume OS (`/api/v1/resume/*`)
Complete resume lifecycle management:
- Create from templates or start from scratch
- Copy and version existing resumes
- Update content with structured data
- Get AI-powered scoring and feedback
- Request peer and advisor reviews
- Track review status and counts

**11 dedicated resume endpoints**

### Job Management (`/api/v1/jobs/*`)
- Advanced search with multiple filters
- Save jobs to personal collection
- Track saved jobs by status
- Manage saved search filters
- AI-powered job extraction

### Networking (`/api/v1/contacts/*`)
- Contact management by company and status
- Direct messaging capabilities
- Real-time chat integration
- Relationship tracking

### Events (`/api/v1/events/*`)
- Event search and discovery
- Registration and application
- Speaker information
- Calendar integration (ICS download)
- Cancellation management

### Alumni Network (`/api/v1/alumn/*`)
Unique differentiator connecting students with alumni:
- Search by cohort
- Search by university
- Alumni profiles and contact information

### AI Features (`/api/v1/ai/*`)
Forward-thinking capabilities:
- Job extraction from external sources
- Smart content analysis
- Enhanced matching algorithms

### Advisor Tools - V2 API (`/api/v2/*`)
Enhanced analytics and management:
- Student listing with advanced filtering
- AI-generated student summaries
- Networking analytics
- Top companies analysis
- Cohort reporting
- Student notes and case management

**Over 100 documented endpoints** covering the complete career development lifecycle

---

## Why CareerOS Stands Out

### 1. Built for Both Students and Advisors
Unlike many platforms that focus primarily on administrative needs, CareerOS is designed from the ground up to serve both students and career advisors equally well.

### 2. API-First Architecture
Every feature is accessible via API, enabling institutions to build custom experiences, integrate with existing systems, and create mobile applications.

### 3. Modern Development Experience
- Industry-standard OpenAPI specification
- Comprehensive code examples
- Clear error messages and validation
- Consistent response formats
- Detailed documentation

### 4. Real-Time Capabilities
WebSocket support for live updates ensures students and advisors always have the most current information.

### 5. AI-Powered Intelligence
Built-in AI features for job extraction, resume scoring, and student summaries help users make better decisions faster.

### 6. Alumni Networking
Unique focus on connecting current students with successful alumni creates valuable mentorship and networking opportunities.

### 7. Resume OS
A complete resume management system that goes beyond simple document storage to include scoring, review workflows, and actionable feedback.

### 8. Flexible Authentication
Support for multiple authentication methods (JWT, API keys) and integration with university identity providers.

### 9. Comprehensive Analytics
Deep insights into student engagement, career readiness, and program effectiveness help advisors make data-driven decisions.

### 10. Scalable Design
Built to support institutions of any size, from small colleges to large university systems with multiple campuses.

---

## Technical Excellence

### Security
- Auth0-powered JWT authentication
- Role-based access control
- Secure token management
- API key authentication for partners
- Regular security audits

### Performance
- Efficient endpoint design
- Clear rate limiting
- Pagination support
- Optimized query parameters
- Caching-friendly architecture

### Reliability
- Standard HTTP status codes
- Comprehensive error messages
- Validation at every endpoint
- Graceful degradation
- Production and development environments

### Developer Experience
- OpenAPI 3.1.0 specification for code generation
- Consistent naming conventions
- Predictable response structures
- Detailed parameter descriptions
- Real-world code examples

---

## API Statistics

| Category                 | Count |
|--------------------------|-------|
| **Total Endpoints**      | 100+ |
| **Public APIs**          | 10 |
| **User APIs**            | 12 |
| **Company APIs**         | 11 |
| **Job APIs**             | 7 |
| **Contact APIs**         | 8 |
| **Resume APIs**          | 11 |
| **Event APIs**           | 6 |
| **Alumni APIs**          | 3 |
| **Advisor V1 APIs**      | 3 |
| **Advisor V2 APIs**      | 8 |
| **AI Features**          | 1 |
| **CalendarOS**           | 5 |
| **Agreements**           | 5 |
| **Employers**            | 5 |
| **Custom Fields (BETA)** | 2 |
| **Reports (PROPOSAL)**   | 3 |
| **Filter Fields**        | 1 |

### Feature Module Breakdown

**Stable Modules (5):**
- ResumeOS
- EventOS
- AlumniOS
- Jobs & Companies
- AI Features
- CalendarOS
- Agreements API
- Employers API
- Custom Fields API

**Proposal (1):**
- Reports API

---

## BETA Features & Partner Feedback

### What BETA Means for Integration Partners

CareerOS marks certain features as **BETA** to indicate:

- Naming conventions may be adjusted for clarity

📢 **We Want Your Input**
- Share your integration requirements
- Suggest improvements and enhancements
- Report any issues or challenges
- Help shape the future of these APIs

### Reports API - Currently a PROPOSAL

The Reports API is in **PROPOSAL** status, meaning we're actively designing it based on partner requirements.

**We Need Your Input On:**
- Critical reports for your institution
- Required export formats
- Data points and metrics to include
- Time ranges and filtering options
- Scheduled report delivery
- Report sharing and distribution
- Data visualization requirements

### API Versioning & Compatibility

**Our Commitments:**
- ✅ Core API V1 (`/api/v1/*`) - Stable, breaking changes announced 90 days in advance
- ✅ Advisor API V2 (`/api/v2/*`) - Stable, backward compatible additions only
- ✅ BETA features - May evolve, changes announced 30 days in advance
- ✅ PROPOSAL features - Under active design, significant changes possible
- ✅ Clear migration guides for any breaking changes
- ✅ Deprecation notices with ample transition time
- ✅ Backward compatibility whenever possible

## Getting Started

### Quick Start
1. Obtain authentication credentials
2. Make your first API call to `/api/v1/users/self`
3. Explore endpoints relevant to your use case
4. Integrate into your application

## Conclusion

CareerOS represents a modern, comprehensive approach to career development platforms. With its robust API, extensive feature set, and focus on both student and advisor needs, CareerOS provides the foundation for exceptional career services programs.

The platform combines the best of modern technology—OpenAPI standards, JWT authentication, real-time capabilities, AI-powered features—with practical, real-world features that students and advisors use every day.

Whether you're building a custom student portal, integrating with existing systems, or creating mobile applications, CareerOS provides the APIs and documentation you need to succeed.

---

## Questions or Feedback

For questions about the CareerOS API:
- **Technical Documentation**: Review the [API Reference](./openapi.yaml)
- **Authentication Help**: See [AUTHENTICATION.md](./AUTHENTICATION.md)
- **Support**: Contact oleksii@thecareeros.com
- **Website**: https://thecareeros.com

Let's build something great together!
