# Development Guide

This document provides exact steps for setting up and running ZetuBIZ in a local development environment.

## Prerequisites

- **Node.js:** 18 or higher
- **npm:** 9 or higher
- **Git:** Latest version
- **Supabase CLI:** Latest version
- **Wrangler CLI:** Latest version (for Cloudflare Workers)
- **Code Editor:** VS Code or similar

## Initial Setup

### 1. Clone the Repository

```bash
git clone <repository-url>
cd ZetuBIZ
```

### 2. Install Node Dependencies

```bash
npm install
```

This installs:
- Development dependencies
- Build tools
- Testing frameworks
- Supabase CLI
- Wrangler CLI

### 3. Install Supabase CLI

```bash
npm install -g supabase
```

Verify installation:
```bash
supabase --version
```

### 4. Install Wrangler CLI

```bash
npm install -g wrangler
```

Verify installation:
```bash
wrangler --version
```

## Environment Configuration

### 1. Create Environment File

```bash
cp .env.example .env
```

### 2. Configure Supabase

Create a Supabase project at https://supabase.com

Add to `.env`:
```bash
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
```

### 3. Configure Email Provider

Choose Resend or SendGrid and add to `.env`:

**Resend:**
```bash
EMAIL_PROVIDER=resend
RESEND_API_KEY=your-resend-api-key
RESEND_FROM_EMAIL=noreply@yourdomain.com
```

**SendGrid:**
```bash
EMAIL_PROVIDER=sendgrid
SENDGRID_API_KEY=your-sendgrid-api-key
SENDGRID_FROM_EMAIL=noreply@yourdomain.com
```

### 4. Configure Payment Providers

Add to `.env` (only for providers you're using):

```bash
# Paystack
PAYSTACK_SECRET_KEY=your-paystack-secret-key
PAYSTACK_PUBLIC_KEY=your-paystack-public-key

# M-Pesa Daraja
MPESA_CONSUMER_KEY=your-consumer-key
MPESA_CONSUMER_SECRET=your-consumer-secret
MPESA_PASSKEY=your-passkey
MPESA_SHORTCODE=your-shortcode
MPESA_ENVIRONMENT=sandbox

# PayHero
PAYHERO_API_KEY=your-payhero-api-key
PAYHERO_MERCHANT_ID=your-merchant-id
```

### 5. Configure Cloudflare

```bash
wrangler login
```

Add to `.env`:
```bash
CLOUDFLARE_ACCOUNT_ID=your-account-id
CLOUDFLARE_API_TOKEN=your-api-token
```

## Database Setup

### 1. Link to Supabase Project

```bash
supabase link --project-ref YOUR_PROJECT_REF
```

### 2. Run Initial Migrations

```bash
supabase db push
```

This creates all tables, indexes, and RLS policies defined in `docs/DATABASE.md`.

### 3. Seed Development Data (Optional)

Create a seed file in `supabase/seed.sql`:
```bash
supabase db seed
```

### 4. Generate TypeScript Types

```bash
supabase gen types typescript --local > src/types/supabase.ts
```

## Running the Application

### Start Development Server

```bash
npm run dev
```

This starts:
- Frontend development server (typically on http://localhost:3000)
- Cloudflare Workers local environment
- Hot module reloading

### Start Cloudflare Workers Locally

```bash
wrangler dev
```

### Start Supabase Local (Optional)

```bash
supabase start
```

This runs a local Supabase instance for offline development.

## Development Workflow

### 1. Create a Feature Branch

```bash
git checkout -b feature/your-feature-name
```

### 2. Make Changes

- Edit code
- Add tests
- Update documentation

### 3. Run Tests

```bash
npm test
```

### 4. Run Linting

```bash
npm run lint
```

### 5. Run Type Checking (if using TypeScript)

```bash
npm run type-check
```

### 6. Commit Changes

```bash
git add .
git commit -m "Descriptive commit message"
```

### 7. Push Branch

```bash
git push origin feature/your-feature-name
```

## Database Development

### Create a New Migration

```bash
supabase migration new describe_your_change
```

Edit the generated migration file in `supabase/migrations/`.

### Apply Migration

```bash
supabase db push
```

### View Migration Status

```bash
supabase migration list
```

### Reset Database (Development Only)

```bash
supabase db reset
```

**Warning:** This deletes all data in the local database.

### Access Database Directly

```bash
supabase db reset
supabase db shell
```

## Testing

### Run All Tests

```bash
npm test
```

### Run Specific Test File

```bash
npm test -- path/to/test.spec.js
```

### Run Tests in Watch Mode

```bash
npm test -- --watch
```

### Run Tests with Coverage

```bash
npm test -- --coverage
```

See `docs/TESTING.md` for detailed testing strategy.

## Debugging

### Debug Cloudflare Workers

```bash
wrangler dev --local
```

Add console.log statements in your worker code.

### Debug Frontend

Use browser DevTools:
- Open DevTools (F12)
- Check Console for errors
- Use Network tab to inspect API calls
- Use Sources tab for debugging JavaScript

### Debug Database

Use Supabase dashboard:
- Go to https://supabase.com/dashboard
- Select your project
- Use SQL Editor to run queries
- Use Table Editor to inspect data

## Common Development Tasks

### Add a New API Endpoint

1. Create worker file in `workers/your-module/`
2. Implement handler function
3. Add route in main worker or router
4. Add tests
5. Update documentation

### Add a New Database Table

1. Create migration: `supabase migration new add_table_name`
2. Define table schema
3. Add RLS policies
4. Apply migration: `supabase db push`
5. Generate types: `supabase gen types typescript --local`
6. Update documentation

### Add a New Frontend Page

1. Create HTML file in `public/` or `src/`
2. Add styling with TailwindCSS
3. Add JavaScript for interactivity
4. Test responsive layout
5. Update documentation

## Code Style

### JavaScript

- Use ES6+ syntax
- Use const/let, not var
- Use arrow functions for callbacks
- Use template literals for strings
- Add JSDoc comments for functions

### SQL

- Use uppercase for SQL keywords
- Use lowercase for table/column names
- Add comments for complex queries
- Use prepared statements

### Git Commits

Use conventional commits:
```
feat: add quotation creation
fix: resolve invoice calculation error
docs: update API documentation
test: add payment integration tests
refactor: simplify payment module
```

## Troubleshooting

### Supabase Connection Issues

**Problem:** Cannot connect to Supabase

**Solution:**
- Check SUPABASE_URL in .env
- Verify network connectivity
- Check Supabase service status
- Try: `supabase status`

### Migration Conflicts

**Problem:** Migration fails with conflict

**Solution:**
- Check migration history: `supabase migration list`
- Resolve conflict manually
- Or reset: `supabase db reset` (development only)

### Cloudflare Workers Errors

**Problem:** Worker fails to start

**Solution:**
- Check wrangler.toml configuration
- Verify CLOUDFLARE_ACCOUNT_ID
- Check worker syntax
- View logs: `wrangler tail`

### Environment Variables Not Loading

**Problem:** Process.env values undefined

**Solution:**
- Verify .env file exists
- Check .env file syntax (no spaces around =)
- Restart development server
- Check .gitignore (ensure .env is ignored)

## Performance Tips

### Database Queries
- Use indexes on frequently queried columns
- Avoid SELECT * - select only needed columns
- Use pagination for large result sets
- Use connection pooling

### Frontend
- Lazy load images
- Minimize JavaScript bundle size
- Use code splitting for large apps
- Enable browser caching

### Workers
- Keep execution time under limits
- Use caching for expensive operations
- Implement retry logic for external APIs
- Use background jobs for long tasks

## Security in Development

### Never Commit Secrets
- Ensure .env is in .gitignore
- Never commit real API keys
- Use .env.example as template only

### Use Test Environments
- Use provider sandbox/test modes
- Use test payment cards
- Use test email addresses

### Validate Inputs
- Never trust client-side validation
- Validate all inputs server-side
- Sanitize user inputs
- Use parameterized queries

## IDE Configuration

### VS Code Extensions (Recommended)

- ESLint
- Prettier
- PostgreSQL
- Tailwind CSS IntelliSense
- GitLens

### VS Code Settings

Create `.vscode/settings.json`:
```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  }
}
```

## Next Steps

After completing setup:
1. Read `docs/ZetuBIZ-MASTER-SPEC.md`
2. Read `docs/ARCHITECTURE.md`
3. Check `docs/IMPLEMENTATION-STATUS.md`
4. Pick a task from `TASKS.md`
5. Start implementing

---

**Status:** Development guide documented - implementation pending  
**Last updated:** October 20, 2026
