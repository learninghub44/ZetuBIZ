# Architecture

This document describes the planned architecture for ZetuBIZ.

## System Overview

ZetuBIZ is a multi-tenant SaaS application built on a serverless architecture with strong tenant isolation and security-first design.

## Technology Stack

### Frontend
- **Language:** HTML5, CSS3, JavaScript (ES6+)
- **Framework:** Vanilla JavaScript (no framework initially)
- **Styling:** TailwindCSS via CDN or build process
- **Typography:** Inter (Google Fonts)
- **Build Tool:** To be determined (likely Vite or similar)

### Backend
- **Runtime:** Cloudflare Workers (serverless)
- **API:** RESTful endpoints via Workers
- **Authentication:** Supabase Auth
- **File Storage:** Cloudflare R2

### Database
- **Database:** Supabase PostgreSQL
- **ORM:** Direct SQL or lightweight ORM (to be determined)
- **Migrations:** Supabase CLI
- **Security:** Row Level Security (RLS)

### Additional Services
- **Email:** Resend or SendGrid
- **PDF Generation:** Puppeteer or similar server-side rendering
- **Payment Providers:** Paystack, M-Pesa Daraja, PayHero

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                         Client Browser                        │
│  (HTML/CSS/JS + TailwindCSS)                                 │
└──────────────────────┬──────────────────────────────────────┘
                       │ HTTPS
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                    Cloudflare Pages                           │
│  (Static Asset Hosting)                                       │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                  Cloudflare Workers                           │
│  (Serverless API Endpoints)                                   │
│  - Authentication                                             │
│  - Business Logic                                             │
│  - Payment Processing                                         │
│  - PDF Generation                                             │
└──────────┬──────────────────────────┬────────────────────────┘
           │                          │
           ▼                          ▼
┌──────────────────────┐   ┌──────────────────────┐
│   Supabase           │   │   Cloudflare R2       │
│   - PostgreSQL        │   │   - File Storage      │
│   - Auth             │   │   - Documents         │
│   - RLS Policies     │   │   - Logos             │
└──────────────────────┘   └──────────────────────┘
           │
           ▼
┌──────────────────────┐
│  External Services   │
│  - Email Provider     │
│  - Payment Providers  │
└──────────────────────┘
```

## Module Architecture

### 1. Authentication Module
**Location:** `workers/auth/` or `functions/auth/`
**Responsibilities:**
- User registration and login
- Email verification
- Password recovery
- Session management
- Token validation

**Dependencies:** Supabase Auth

### 2. Business Module
**Location:** `workers/business/`
**Responsibilities:**
- Business creation and configuration
- Business settings management
- Branding configuration
- Onboarding workflow

**Dependencies:** Supabase Database

### 3. Client Module
**Location:** `workers/clients/`
**Responsibilities:**
- Client CRUD operations
- Client search and filtering
- Client contact management

**Dependencies:** Supabase Database

### 4. Project Module
**Location:** `workers/projects/`
**Responsibilities:**
- Project CRUD operations
- Milestone management
- Team assignment
- Project financial summaries

**Dependencies:** Supabase Database

### 5. Quotation Module
**Location:** `workers/quotations/`
**Responsibilities:**
- Quotation creation and editing
- Line item management
- Calculations (server-side)
- PDF generation
- Public link generation
- Client acceptance tracking

**Dependencies:** Supabase Database, PDF Service

### 6. Invoice Module
**Location:** `workers/invoices/`
**Responsibilities:**
- Invoice creation from scratch or quotation
- Safe numbering
- Finalization and snapshots
- Credit notes
- PDF generation
- Public link generation

**Dependencies:** Supabase Database, PDF Service

### 7. Payment Module
**Location:** `workers/payments/`
**Responsibilities:**
- Payment initiation via providers
- Webhook processing
- Payment verification
- Payment allocation
- Idempotency handling

**Dependencies:** Payment Provider APIs, Supabase Database

### 8. Expense Module
**Location:** `workers/expenses/`
**Responsibilities:**
- Expense CRUD operations
- Document upload
- Category management
- Expense reports

**Dependencies:** Supabase Database, Cloudflare R2

### 9. Reconciliation Module
**Location:** `workers/reconciliation/`
**Responsibilities:**
- CSV import
- Transaction matching
- Manual resolution
- Reconciliation reports

**Dependencies:** Supabase Database

### 10. Report Module
**Location:** `workers/reports/`
**Responsibilities:**
- Financial summaries
- Invoice reports
- Payment reports
- Export generation

**Dependencies:** Supabase Database

### 11. Notification Module
**Location:** `workers/notifications/`
**Responsibilities:**
- Email composition and sending
- Reminder scheduling
- Notification preferences
- Delivery tracking

**Dependencies:** Email Provider, Supabase Database

### 12. Team Module
**Location:** `workers/team/`
**Responsibilities:**
- Team member invitations
- Role assignment
- Permission enforcement
- Access revocation

**Dependencies:** Supabase Database, Supabase Auth

### 13. Platform-Admin Module
**Location:** `workers/admin/` (separate deployment)
**Responsibilities:**
- Tenant management
- System monitoring
- Audit log access
- Feature flags
- Background job management

**Dependencies:** Supabase Database (with elevated access)

## Data Flow

### Authentication Flow
```
Client → Cloudflare Pages → Cloudflare Worker (Auth) → Supabase Auth
                                                      ↓
                                              Session Token
                                                      ↓
                                                  Client
```

### Quotation Creation Flow
```
Client → Cloudflare Worker (Quotations) → Supabase Database
                                      ↓
                              PDF Generation Service
                                      ↓
                                  Cloudflare R2
                                      ↓
                                  Client
```

### Payment Flow
```
Client → Cloudflare Worker (Payments) → Payment Provider API
                                      ↓
                              Payment Initiation
                                      ↓
                                  Client
                                      ↓
                              Provider Webhook
                                      ↓
                      Cloudflare Worker (Webhook Handler)
                                      ↓
                              Supabase Database
                                      ↓
                              Email Notification
```

## Tenant Isolation Strategy

### Database Level
- Every table includes `business_id` foreign key
- All queries filter by `business_id`
- Indexes on `business_id` for performance

### Application Level
- Server-side validation of business membership on every operation
- Session tokens include business context
- Authorization checks before data access

### RLS Policies
- Supabase Row Level Security policies enforce tenant isolation
- Policies check user's business membership
- No direct table access without RLS check

Example RLS policy:
```sql
CREATE POLICY "Users can view own business data"
ON clients
FOR SELECT
USING (
  business_id IN (
    SELECT business_id 
    FROM business_memberships 
    WHERE user_id = auth.uid()
  )
);
```

## Security Architecture

### Secret Management
- Secrets stored in Cloudflare Workers secrets
- Never exposed to client-side code
- Payment credentials encrypted in database
- Rotation procedures documented

### Authentication
- Supabase Auth for user authentication
- JWT tokens for session management
- Token expiration and refresh
- Secure cookie flags (HttpOnly, Secure, SameSite)

### Authorization
- Role-based permissions (owner, admin, finance, manager, staff, viewer)
- Server-side permission checks
- No client-side permission enforcement

### Payment Security
- Webhook signature verification
- Idempotency keys for all payment operations
- Never trust client-side payment confirmation
- Verify via server-side API or webhook

### Audit Trail
- All sensitive operations logged
- Audit events include actor, timestamp, action, outcome
- Audit logs append-only for ordinary users
- Platform administrators can view all audit logs

## File Structure (Planned)

```
ZetuBIZ/
├── public/                 # Static assets
│   ├── index.html
│   ├── css/
│   ├── js/
│   └── assets/
├── workers/                # Cloudflare Workers
│   ├── auth/
│   ├── business/
│   ├── clients/
│   ├── projects/
│   ├── quotations/
│   ├── invoices/
│   ├── payments/
│   ├── expenses/
│   ├── reconciliation/
│   ├── reports/
│   ├── notifications/
│   ├── team/
│   └── admin/
├── supabase/               # Database migrations
│   └── migrations/
├── docs/                   # Documentation
│   ├── ZetuBIZ-MASTER-SPEC.md
│   ├── ARCHITECTURE.md
│   ├── DATABASE.md
│   ├── DEVELOPMENT.md
│   ├── ENVIRONMENT.md
│   ├── TESTING.md
│   ├── DEPLOYMENT.md
│   ├── IMPLEMENTATION-STATUS.md
│   └── HANDOVER.md
├── src/                    # Shared code (if needed)
│   ├── types/
│   ├── utils/
│   └── constants/
├── tests/                  # Test files
├── .env.example
├── .gitignore
├── AGENTS.md
├── README.md
├── TASKS.md
├── package.json
└── wrangler.toml
```

## API Design

### RESTful Conventions
- Use HTTP verbs correctly (GET, POST, PUT, DELETE)
- Use plural nouns for resources (/clients, /invoices)
- Use query parameters for filtering and pagination
- Return appropriate HTTP status codes
- Use JSON for request/response bodies

### Authentication
- Require valid session token for protected endpoints
- Include token in Authorization header
- Return 401 for invalid or expired tokens
- Return 403 for insufficient permissions

### Error Responses
Standard error format:
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input",
    "details": {
      "field": "email",
      "issue": "Invalid format"
    }
  }
}
```

### Pagination
Use cursor-based or offset-based pagination:
```
GET /invoices?limit=20&offset=0
```

Response:
```json
{
  "data": [...],
  "pagination": {
    "limit": 20,
    "offset": 0,
    "total": 150,
    "has_more": true
  }
}
```

## Performance Considerations

### Database
- Index foreign keys and frequently queried columns
- Use connection pooling
- Optimize complex queries
- Consider read replicas for scaling

### Caching
- Cache static assets on Cloudflare CDN
- Cache frequently accessed data (Redis, if needed)
- Use HTTP caching headers appropriately

### Workers
- Keep worker execution time under limits
- Use background jobs for long-running tasks
- Implement retry logic for external API calls

## Scalability Strategy

### Horizontal Scaling
- Cloudflare Workers auto-scale
- Database can scale vertically or with read replicas
- File storage scales with Cloudflare R2

### Multi-Region
- Deploy Cloudflare Workers to multiple regions
- Consider database read replicas for global access
- CDN for static assets

### Rate Limiting
- Implement rate limiting on API endpoints
- Per-tenant rate limits
- Global rate limits for abuse prevention

## Monitoring and Observability

### Logging
- Structured JSON logs
- Correlation IDs for request tracing
- Log levels (debug, info, warn, error)
- Never log secrets or sensitive data

### Metrics
- Request latency
- Error rates
- Throughput
- Provider availability
- Database query performance

### Alerts
- High error rate
- Slow responses
- Provider downtime
- Failed webhooks
- Security events

## Disaster Recovery

### Backups
- Automated daily database backups
- Point-in-time recovery capability
- Backup retention policy (30 days)

### Rollback
- Database migration rollback procedures
- Application deployment rollback
- Documented recovery procedures

### High Availability
- Multi-region deployment
- Database failover (if supported)
- CDN for static assets

---

**Status:** Planned architecture - implementation pending  
**Last updated:** October 20, 2026
