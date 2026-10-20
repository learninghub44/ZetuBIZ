# Environment Variables

This document lists all environment variables used by ZetuBIZ, their purpose, where they belong, and whether they are required.

## Required Variables

### Supabase

**SUPABASE_URL**
- **Purpose:** Supabase project URL
- **Where:** Cloudflare Workers secrets, .env
- **Required:** Yes
- **Example:** `https://your-project.supabase.co`

**SUPABASE_ANON_KEY**
- **Purpose:** Supabase anonymous/public key for client-side access
- **Where:** Cloudflare Workers secrets, .env
- **Required:** Yes
- **Example:** `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...`

**SUPABASE_SERVICE_ROLE_KEY**
- **Purpose:** Supabase service role key for server-side admin access
- **Where:** Cloudflare Workers secrets only (never client-side)
- **Required:** Yes
- **Example:** `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...`

**Security Note:** Never expose service role key to client-side code.

## Optional Variables

### Email Provider

Choose one email provider:

**EMAIL_PROVIDER**
- **Purpose:** Email provider selection (resend or sendgrid)
- **Where:** .env
- **Required:** Yes (if using email features)
- **Values:** `resend` or `sendgrid`

**RESEND_API_KEY**
- **Purpose:** Resend API key for sending emails
- **Where:** Cloudflare Workers secrets, .env
- **Required:** Yes (if using Resend)
- **Example:** `re_1234567890abcdef`

**RESEND_FROM_EMAIL**
- **Purpose:** Default sender email address
- **Where:** .env
- **Required:** Yes (if using Resend)
- **Example:** `noreply@yourdomain.com`

**SENDGRID_API_KEY**
- **Purpose:** SendGrid API key for sending emails
- **Where:** Cloudflare Workers secrets, .env
- **Required:** Yes (if using SendGrid)
- **Example:** `SG.1234567890abcdef`

**SENDGRID_FROM_EMAIL**
- **Purpose:** Default sender email address
- **Where:** .env
- **Required:** Yes (if using SendGrid)
- **Example:** `noreply@yourdomain.com`

### Payment Providers

Configure only the providers you're using:

**PAYSTACK_SECRET_KEY**
- **Purpose:** PayStack secret key for API authentication
- **Where:** Cloudflare Workers secrets, .env
- **Required:** No (only if using Paystack)
- **Example:** `sk_test_1234567890abcdef`

**PAYSTACK_PUBLIC_KEY**
- **Purpose:** PayStack public key for client-side payment initiation
- **Where:** .env (client-side)
- **Required:** No (only if using Paystack)
- **Example:** `pk_test_1234567890abcdef`

**MPESA_CONSUMER_KEY**
- **Purpose:** M-Pesa Daraja consumer key
- **Where:** Cloudflare Workers secrets, .env
- **Required:** No (only if using M-Pesa)
- **Example:** `your-consumer-key`

**MPESA_CONSUMER_SECRET**
- **Purpose:** M-Pesa Daraja consumer secret
- **Where:** Cloudflare Workers secrets, .env
- **Required:** No (only if using M-Pesa)
- **Example:** `your-consumer-secret`

**MPESA_PASSKEY**
- **Purpose:** M-Pesa Daraja passkey for STK push
- **Where:** Cloudflare Workers secrets, .env
- **Required:** No (only if using M-Pesa)
- **Example:** `your-passkey`

**MPESA_SHORTCODE**
- **Purpose:** M-Pesa business short code
- **Where:** .env
- **Required:** No (only if using M-Pesa)
- **Example:** `174379`

**MPESA_ENVIRONMENT**
- **Purpose:** M-Pesa environment (sandbox or live)
- **Where:** .env
- **Required:** No (only if using M-Pesa)
- **Values:** `sandbox` or `live`
- **Default:** `sandbox`

**PAYHERO_API_KEY**
- **Purpose:** PayHero API key
- **Where:** Cloudflare Workers secrets, .env
- **Required:** No (only if using PayHero)
- **Example:** `your-payhero-api-key`

**PAYHERO_MERCHANT_ID**
- **Purpose:** PayHero merchant ID
- **Where:** .env
- **Required:** No (only if using PayHero)
- **Example:** `your-merchant-id`

### Cloudflare

**CLOUDFLARE_ACCOUNT_ID**
- **Purpose:** Cloudflare account ID for Workers deployment
- **Where:** .env, wrangler.toml
- **Required:** Yes (for deployment)
- **Example:** `1234567890abcdef`

**CLOUDFLARE_API_TOKEN**
- **Purpose:** Cloudflare API token for authentication
- **Where:** Cloudflare Workers secrets, .env
- **Required:** Yes (for deployment)
- **Example:** `your-api-token`

### Application

**NODE_ENV**
- **Purpose:** Application environment
- **Where:** .env
- **Required:** No
- **Values:** `development`, `staging`, `production`
- **Default:** `development`

**APP_URL**
- **Purpose:** Base URL for the application
- **Where:** .env
- **Required:** Yes (for email links, webhooks)
- **Example:** `https://your-domain.com`

**API_BASE_URL**
- **Purpose:** Base URL for API endpoints
- **Where:** .env
- **Required:** No (defaults to relative paths)
- **Example:** `https://api.your-domain.com`

### File Storage

**R2_BUCKET_NAME**
- **Purpose:** Cloudflare R2 bucket name for file storage
- **Where:** .env, wrangler.toml
- **Required:** Yes (for file uploads)
- **Example:** `zetubiz-documents`

**R2_ACCESS_KEY_ID**
- **Purpose:** R2 access key ID
- **Where:** Cloudflare Workers secrets, .env
- **Required:** Yes (for file uploads)
- **Example:** `your-access-key-id`

**R2_SECRET_ACCESS_KEY**
- **Purpose:** R2 secret access key
- **Where:** Cloudflare Workers secrets, .env
- **Required:** Yes (for file uploads)
- **Example:** `your-secret-access-key`

### Security

**JWT_SECRET**
- **Purpose:** Secret for JWT token signing (if custom JWT implementation)
- **Where:** Cloudflare Workers secrets, .env
- **Required:** No (using Supabase Auth)
- **Example:** `your-jwt-secret`

**SESSION_SECRET**
- **Purpose:** Secret for session encryption
- **Where:** Cloudflare Workers secrets, .env
- **Required:** No (using Supabase Auth)
- **Example:** `your-session-secret`

**ENCRYPTION_KEY**
- **Purpose:** Key for encrypting sensitive data (payment credentials)
- **Where:** Cloudflare Workers secrets only
- **Required:** Yes (for credential encryption)
- **Example:** `your-32-character-encryption-key`

### Monitoring

**SENTRY_DSN**
- **Purpose:** Sentry DSN for error tracking
- **Where:** Cloudflare Workers secrets, .env
- **Required:** No
- **Example:** `https://your-sentry-dsn`

**LOG_LEVEL**
- **Purpose:** Logging verbosity
- **Where:** .env
- **Required:** No
- **Values:** `debug`, `info`, `warn`, `error`
- **Default:** `info`

## Environment-Specific Configuration

### Development (.env.development)

```bash
NODE_ENV=development
APP_URL=http://localhost:3000
LOG_LEVEL=debug
MPESA_ENVIRONMENT=sandbox
```

### Staging (.env.staging)

```bash
NODE_ENV=staging
APP_URL=https://staging.zetubiz.com
LOG_LEVEL=info
MPESA_ENVIRONMENT=sandbox
```

### Production (.env.production)

```bash
NODE_ENV=production
APP_URL=https://zetubiz.com
LOG_LEVEL=warn
MPESA_ENVIRONMENT=live
```

## Cloudflare Workers Secrets

Set these using Wrangler CLI:

```bash
wrangler secret put SUPABASE_SERVICE_ROLE_KEY
wrangler secret put ENCRYPTION_KEY
wrangler secret put RESEND_API_KEY
wrangler secret put PAYSTACK_SECRET_KEY
```

Or set in Cloudflare dashboard:
1. Go to Workers & Pages
2. Select your worker
3. Settings > Variables and Secrets
4. Add secrets

## Security Best Practices

### Never Commit Secrets
- Add `.env` to `.gitignore`
- Never commit `.env` file
- Never commit secrets in code
- Use `.env.example` as template only

### Use Different Keys per Environment
- Development keys should be test/sandbox keys
- Staging keys should be test/sandbox keys
- Production keys should be live keys
- Never use production keys in development

### Rotate Keys Regularly
- Rotate API keys every 90 days
- Rotate encryption keys annually
- Document rotation procedures
- Test key rotation in staging first

### Limit Key Permissions
- Use minimum required permissions
- Use scoped keys where available
- Revoke unused keys
- Monitor key usage

## Variable Validation

The application should validate:
- Required variables are present
- Variable formats are correct (URLs, emails)
- No empty strings where values are expected
- Environment-specific configurations are appropriate

## Troubleshooting

### Variables Not Loading

**Problem:** Environment variables undefined

**Solutions:**
- Check .env file exists
- Check .env file syntax (no spaces around =)
- Restart development server
- Check Cloudflare Workers secrets are set
- Verify variable names match exactly

### Invalid API Keys

**Problem:** API authentication failing

**Solutions:**
- Verify API key is correct
- Check key has required permissions
- Verify key is for correct environment (test/live)
- Check key hasn't expired or been revoked

### Cloudflare Workers Secrets Not Working

**Problem:** Workers can't access secrets

**Solutions:**
- Verify secrets are set in Cloudflare dashboard
- Check secret names match exactly
- Use `wrangler secret list` to verify
- Redeploy worker after adding secrets

## .env.example Template

```bash
# Supabase
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key

# Email Provider
EMAIL_PROVIDER=resend
RESEND_API_KEY=your-resend-api-key
RESEND_FROM_EMAIL=noreply@yourdomain.com

# Payment Providers
PAYSTACK_SECRET_KEY=your-paystack-secret-key
PAYSTACK_PUBLIC_KEY=your-paystack-public-key
MPESA_CONSUMER_KEY=your-consumer-key
MPESA_CONSUMER_SECRET=your-consumer-secret
MPESA_PASSKEY=your-passkey
MPESA_SHORTCODE=174379
MPESA_ENVIRONMENT=sandbox
PAYHERO_API_KEY=your-payhero-api-key
PAYHERO_MERCHANT_ID=your-merchant-id

# Cloudflare
CLOUDFLARE_ACCOUNT_ID=your-account-id
CLOUDFLARE_API_TOKEN=your-api-token

# Application
NODE_ENV=development
APP_URL=http://localhost:3000

# File Storage
R2_BUCKET_NAME=zetubiz-documents
R2_ACCESS_KEY_ID=your-access-key-id
R2_SECRET_ACCESS_KEY=your-secret-access-key

# Security
ENCRYPTION_KEY=your-32-character-encryption-key

# Monitoring (Optional)
SENTRY_DSN=https://your-sentry-dsn
LOG_LEVEL=debug
```

---

**Status:** Environment variables documented  
**Last updated:** October 20, 2026
