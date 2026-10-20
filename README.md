# ZetuBIZ

A customizable business management platform for freelancers, developers, consultants, agencies, and small businesses in Kenya and beyond.

## Product Overview

ZetuBIZ helps businesses manage:
- Clients and contacts
- Projects and milestones
- Quotations and proposals
- Invoices and receipts
- Online payment collection
- Expenses and financial reports
- Team collaboration
- Business branding and document templates

**Primary market:** Kenya, with architecture supporting international expansion  
**Target users:** Developers, freelancers, consultants, agencies, SMEs, and service-based businesses

## Key Features

- **Adaptive Onboarding:** Workspace configuration based on business type and needs
- **Document Studio:** Professional quotation and invoice templates with live preview
- **Secure Sharing:** High-entropy tokens for client document access
- **Payment Integration:** Support for Paystack, M-Pesa Daraja, and PayHero
- **Multi-Tenancy:** Complete data isolation between businesses
- **Financial Accuracy:** Decimal-safe calculations and immutable invoice snapshots
- **Email Integration:** Transactional email for documents and reminders

## Technology Stack

- **Frontend:** HTML5, CSS3, JavaScript (ES6+)
- **Styling:** TailwindCSS
- **Backend:** Cloudflare Workers (serverless)
- **Hosting:** Cloudflare Pages
- **Storage:** Cloudflare R2
- **Database:** Supabase PostgreSQL
- **Authentication:** Supabase Auth
- **Email:** Resend or SendGrid

## Prerequisites

- Node.js 18+ (for development tools)
- Git
- Supabase account (for database and auth)
- Cloudflare account (for deployment)
- Email provider account (Resend or SendGrid)

## Installation

### 1. Clone the Repository

```bash
git clone <repository-url>
cd ZetuBIZ
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Configure Environment Variables

Copy the example environment file:

```bash
cp .env.example .env
```

Edit `.env` with your values (see `docs/ENVIRONMENT.md` for details):

```bash
# Supabase
SUPABASE_URL=your_supabase_project_url
SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_supabase_service_role_key

# Email Provider
RESEND_API_KEY=your_resend_api_key
# or
SENDGRID_API_KEY=your_sendgrid_api_key

# Payment Providers
PAYSTACK_SECRET_KEY=your_paystack_secret_key
MPESA_CONSUMER_KEY=your_mpesa_consumer_key
MPESA_CONSUMER_SECRET=your_mpesa_consumer_secret
MPESA_PASSKEY=your_mpesa_passkey
PAYHERO_API_KEY=your_payhero_api_key

# Cloudflare
CLOUDFLARE_ACCOUNT_ID=your_cloudflare_account_id
CLOUDFLARE_API_TOKEN=your_cloudflare_api_token
```

**Important:** Never commit `.env` to Git. Use `.env.example` as a template only.

### 4. Set Up Supabase

1. Create a new Supabase project
2. Run database migrations:

```bash
npx supabase db push
```

3. Enable Row Level Security (RLS) policies:

```bash
npx supabase db reset
```

See `docs/DATABASE.md` for detailed schema and migration instructions.

### 5. Set Up Cloudflare Workers

1. Install Wrangler CLI:

```bash
npm install -g wrangler
```

2. Authenticate:

```bash
wrangler login
```

3. Configure your project:

```bash
wrangler init
```

## Development

### Start Local Development Server

```bash
npm run dev
```

This starts:
- Frontend development server
- Cloudflare Workers local environment
- Supabase local development (if configured)

### Run Database Migrations

```bash
npx supabase db push
```

### Generate Types from Database

```bash
npx supabase gen types typescript --local > src/types/supabase.ts
```

### Run Tests

```bash
npm test
```

See `docs/TESTING.md` for detailed testing instructions.

### Lint Code

```bash
npm run lint
```

## Configuration

### Business Settings

Business settings are configured through the application UI after onboarding:
- Business profile and branding
- Document templates
- Payment connections
- Team permissions
- Notification preferences

### Document Templates

Select from 10 professional templates in the Document Studio:
- Modern Minimal
- Professional Corporate
- Clean Freelancer
- Creative Studio
- Software Development Proposal
- Agency Services
- Consulting and Professional Services
- Product and Retail Sales
- Milestone-Based Project
- Simple Tax and Payment Summary

## Testing

See `docs/TESTING.md` for:
- Test strategy
- Test commands
- Test coverage requirements
- Test data fixtures

## Deployment

### Deploy to Cloudflare Pages

```bash
npm run deploy
```

This builds the production application and deploys to Cloudflare Pages.

### Deploy Cloudflare Workers

```bash
wrangler deploy
```

See `docs/DEPLOYMENT.md` for:
- Complete deployment process
- Environment configuration
- Rollback procedures
- Monitoring setup

## Documentation

- **[AGENTS.md](AGENTS.md)** - Instructions for coding agents
- **[docs/ZetuBIZ-MASTER-SPEC.md](docs/ZetuBIZ-MASTER-SPEC.md)** - Complete product specification
- **[docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)** - Architecture and system design
- **[docs/DATABASE.md](docs/DATABASE.md)** - Database schema and migrations
- **[docs/DEVELOPMENT.md](docs/DEVELOPMENT.md)** - Development setup guide
- **[docs/ENVIRONMENT.md](docs/ENVIRONMENT.md)** - Environment variables reference
- **[docs/TESTING.md](docs/TESTING.md)** - Testing strategy and commands
- **[docs/DEPLOYMENT.md](docs/DEPLOYMENT.md)** - Deployment procedures
- **[docs/IMPLEMENTATION-STATUS.md](docs/IMPLEMENTATION-STATUS.md)** - Implementation checklist
- **[docs/HANDOVER.md](docs/HANDOVER.md)** - Handover notes for agents
- **[TASKS.md](TASKS.md)** - Current development tasks

## Security

- All secrets managed via environment variables
- Tenant isolation enforced at database and application level
- Payment credentials encrypted at rest
- Webhook signature verification for all payment callbacks
- Row Level Security (RLS) on all Supabase tables
- Server-side authorization for all sensitive operations

See `AGENTS.md` for detailed security requirements.

## Contributing

1. Read `AGENTS.md` before starting work
2. Read the master specification
3. Check `IMPLEMENTATION-STATUS.md` for current state
4. Pick a task from `TASKS.md`
5. Follow the coding conventions
6. Test your changes
7. Update documentation
8. Submit with clear commit messages

## License

[To be determined]

## Support

For questions about:
- **Product:** See master specification
- **Architecture:** See architecture document
- **Setup:** See development guide
- **Deployment:** See deployment guide
- **Current status:** See implementation status and handover notes

---

**Status:** Greenfield project - specification complete, implementation pending  
**Last updated:** October 20, 2026
