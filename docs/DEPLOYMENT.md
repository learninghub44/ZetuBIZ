# Deployment Guide

This document describes the deployment process for ZetuBIZ to Cloudflare Pages and Workers.

## Deployment Overview

ZetuBIZ uses:
- **Cloudflare Pages** for static frontend hosting
- **Cloudflare Workers** for serverless API endpoints
- **Supabase** for database and authentication
- **Cloudflare R2** for file storage

## Prerequisites

- Cloudflare account with Workers and Pages enabled
- Supabase production project
- Configured environment variables
- DNS configured (if using custom domain)

## Environment Configuration

### Production Environment Variables

Set these in Cloudflare Workers secrets:

```bash
wrangler secret put SUPABASE_SERVICE_ROLE_KEY
wrangler secret put ENCRYPTION_KEY
wrangler secret put RESEND_API_KEY
wrangler secret put PAYSTACK_SECRET_KEY
wrangler secret put R2_ACCESS_KEY_ID
wrangler secret put R2_SECRET_ACCESS_KEY
```

Set these in Cloudflare Pages environment variables:
- SUPABASE_URL
- SUPABASE_ANON_KEY
- APP_URL
- EMAIL_PROVIDER
- CLOUDFLARE_ACCOUNT_ID

### Staging Environment

Create a separate Cloudflare Workers/Pages project for staging:
- Use test/sandbox payment providerKeys
- Use test email provider
- Use staging Supabase project

## Deployment Process

### 1. Build for Production

```bash
npm run build
```

This creates optimized production assets in `dist/` or `public/`.

### 2. Deploy to Cloudflare Pages

```bash
npm run deploy:pages
```

Or using Wrangler:

```bash
wrangler pages deploy ./public --project-name=zetubiz
```

### 3. Deploy Cloudflare Workers

```bash
wrangler deploy
```

This deploys all workers in the project.

### 4. Verify Deployment

- Access the deployed application
- Test critical user flows
- Check worker logs for errors
- Verify database connectivity

## Deployment Environments

### Development

- **URL:** http://localhost:3000
- **Database:** Local Supabase or development project
- **Payment:** Sandbox/test mode
- **Purpose:** Local development

### Staging

- **URL:** https://staging.zetubiz.com (or similar)
- **Database:** Staging Supabase project
- **Payment:** Sandbox/test mode
- **Purpose:** Pre-production testing

### Production

- **URL:** https://zetubiz.com
- **Database:** Production Supabase project
- **Payment:** Live mode
- **Purpose:** Live production application

## Database Migrations

### Apply Migrations to Production

```bash
supabase db push --db-url $PRODUCTION_DATABASE_URL
```

**Important:**
- Test migrations in staging first
- Backup database before major migrations
- Have rollback plan ready
- Run during low-traffic period

### Migration Rollback

If a migration fails:

1. Identify the failing migration
2. Roll back to previous version:
```bash
supabase migration down
```
3. Fix the migration
4. Re-apply:
```bash
supabase db push
```

## Rollback Procedures

### Application Rollback

If deployment causes issues:

**Cloudflare Pages:**
```bash
wrangler pages deploy ./public --project-name=zetubiz --branch=previous-branch
```

**Cloudflare Workers:**
```bash
wrangler deploy --version previous-version
```

### Database Rollback

If database migration causes issues:

1. Restore from backup via Supabase dashboard
2. Or use point-in-time recovery (available within 7 days)
3. Document the issue
4. Fix the migration
5. Re-apply when ready

## Monitoring

### Cloudflare Analytics

Monitor via Cloudflare dashboard:
- Request volume
- Error rates
- Response times
- Geographic distribution

### Worker Logs

View logs:
```bash
wrangler tail
```

Or via Cloudflare dashboard:
- Workers & Pages > Your Worker > Logs

### Database Monitoring

Monitor via Supabase dashboard:
- Query performance
- Connection pool
- Storage usage
- Backup status

### Error Tracking

Configure Sentry (if using):
```bash
wrangler secret put SENTRY_DSN
```

View errors at https://sentry.io

## Health Checks

### Application Health

Create a health check endpoint:
```
GET /health
```

Returns:
```json
{
  "status": "healthy",
  "timestamp": "2026-10-20T12:00:00Z",
  "database": "connected",
  "services": {
    "email": "connected",
    "payments": "connected"
  }
}
```

### Database Health

Check database connectivity:
- Run test query
- Check connection pool
- Verify RLS policies

### External Service Health

Monitor:
- Email provider status
- Payment provider status
- Cloudflare status

## Backup and Recovery

### Automated Backups

Supabase provides:
- Daily automated backups
- 7-day point-in-time recovery
- 30-day backup retention

### Manual Backup

Before major changes:
```bash
# Export database
supabase db dump -f backup-$(date +%Y%m%d).sql
```

### Recovery Procedures

**Database Recovery:**
1. Access Supabase dashboard
2. Go to Settings > Database
3. Select restore point
4. Confirm recovery

**Application Recovery:**
1. Rollback to previous deployment
2. Verify application works
3. Investigate root cause
4. Fix and redeploy

## Security in Production

### Secrets Management

- Never commit secrets to Git
- Use Cloudflare Workers secrets for sensitive data
- Rotate secrets regularly
- Monitor for secret leaks

### HTTPS Only

- Force HTTPS via Cloudflare settings
- Use HSTS headers
- No HTTP access to production

### Security Headers

Set security headers:
```
Content-Security-Policy: default-src 'self'
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: geolocation=(), microphone=()
```

### Rate Limiting

Implement rate limiting on:
- API endpoints
- Authentication attempts
- Payment initiation
- Email sending

## Performance Optimization

### Caching

- Cache static assets on Cloudflare CDN
- Cache API responses where appropriate
- Use cache headers correctly
- Implement cache invalidation

### Database Optimization

- Use indexes on frequently queried columns
- Optimize slow queries
- Use connection pooling
- Consider read replicas for scaling

### Worker Optimization

- Keep worker execution time under limits
- Use caching for expensive operations
- Implement retry logic for external APIs
- Minimize cold starts

## Scaling

### Horizontal Scaling

Cloudflare Workers auto-scale automatically.

### Database Scaling

- Vertical scaling via Supabase dashboard
- Consider read replicas for global access
- Optimize queries before scaling

### CDN Scaling

Cloudflare CDN automatically scales globally.

## Deployment Checklist

Before deploying to production:

- [ ] All tests passing
- [ ] Code reviewed
- [ ] Security review completed
- [ ] Database migrations tested in staging
- [ ] Environment variables configured
- [ ] Secrets set in Cloudflare
- [ ] Backup taken before deployment
- [ ] Rollback plan documented
- [ ] Monitoring configured
- [ ] Documentation updated
- [ ] Stakeholders notified

## Troubleshooting

### Deployment Fails

**Problem:** Deployment command fails

**Solutions:**
- Check Cloudflare credentials
- Verify wrangler.toml configuration
- Check build output exists
- View error logs
- Try deploying individual workers

### Application Not Working After Deployment

**Problem:** Application deployed but not functional

**Solutions:**
- Check environment variables
- Verify database connectivity
- Check worker logs for errors
- Test health check endpoint
- Rollback if necessary

### Database Migration Fails

**Problem:** Migration fails in production

**Solutions:**
- Check migration syntax
- Verify database state
- Rollback to previous migration
- Fix migration
- Re-apply

### High Error Rate After Deployment

**Problem:** Error rate increases after deployment

**Solutions:**
- Check worker logs
- Identify failing endpoints
- Rollback if critical
- Investigate root cause
- Fix and redeploy

## Continuous Deployment

### GitHub Actions (Example)

```yaml
name: Deploy
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
      - run: npm install
      - run: npm test
      - run: npm run build
      - run: wrangler deploy
```

### Deployment Pipeline

1. Code pushed to main branch
2. Tests run automatically
3. Build production assets
4. Deploy to staging
5. Run smoke tests
6. Deploy to production
7. Run health checks
8. Notify team

## Post-Deployment Verification

After deploying to production:

1. **Health Check:** Verify health endpoint returns healthy
2. **Critical Flows:** Test quotation creation, invoice creation, payment flow
3. **Database:** Verify database connectivity and performance
4. **Logs:** Check for errors in worker logs
5. **Monitoring:** Verify metrics are being collected
6. **User Testing:** Have team members test key features
7. **Rollback Plan:** Confirm rollback procedure is ready

## Incident Response

If production issue detected:

1. **Assess:** Determine severity and impact
2. **Communicate:** Notify stakeholders
3. **Mitigate:** Rollback if critical
4. **Investigate:** Review logs and metrics
5. **Fix:** Implement fix
6. **Test:** Test in staging
7. **Deploy:** Deploy fix to production
8. **Verify:** Confirm issue resolved
9. **Document:** Document incident and learnings

---

**Status:** Deployment guide documented  
**Last updated:** October 20, 2026
