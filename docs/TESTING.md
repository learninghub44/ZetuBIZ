# Testing Strategy

This document describes the testing approach, commands, coverage requirements, and test data for ZetuBIZ.

## Testing Philosophy

- Test critical paths end-to-end
- Test financial calculations with unit tests
- Test tenant isolation with integration tests
- Test payment integrations with sandbox environments
- Never use live payments in automated tests
- All features must have passing tests before completion

## Test Types

### Unit Tests

**Purpose:** Test individual functions and modules in isolation

**What to Test:**
- Financial calculations (invoice totals, tax calculations, discounts)
- Status transition logic
- Data validation functions
- Utility functions
- Payment provider adapters (with test mocks)

**Tools:** Jest or similar (to be determined)

**Run:**
```bash
npm run test:unit
```

### Integration Tests

**Purpose:** Test module interactions and database operations

**What to Test:**
- Database CRUD operations
- Authentication flows
- Authorization checks
- API endpoint responses
- RLS policy enforcement
- Tenant isolation

**Tools:** Jest with Supabase test database

**Run:**
```bash
npm run test:integration
```

### End-to-End Tests

**Purpose:** Test complete user workflows

**What to Test:**
- User registration and onboarding
- Quotation creation to PDF download
- Quotation acceptance and invoice conversion
- Invoice creation and payment initiation
- Payment webhook processing
- Secure document sharing
- Email sending

**Tools:** Playwright or Cypress (to be determined)

**Run:**
```bash
npm run test:e2e
```

## Test Commands

### Run All Tests

```bash
npm test
```

### Run Unit Tests Only

```bash
npm run test:unit
```

### Run Integration Tests Only

```bash
npm run test:integration
```

### Run E2E Tests Only

```bash
npm run test:e2e
```

### Run Tests in Watch Mode

```bash
npm test -- --watch
```

### Run Tests with Coverage

```bash
npm test -- --coverage
```

### Run Specific Test File

```bash
npm test path/to/test.spec.js
```

## Test Coverage Requirements

### Minimum Coverage Targets

- **Overall:** 80%
- **Critical paths (financial calculations, auth):** 95%
- **API endpoints:** 85%
- **Database operations:** 90%

### Coverage Report

```bash
npm test -- --coverage
```

Coverage report generated in `coverage/` directory.

## Test Data

### Test Fixtures

Test fixtures are clearly marked and isolated from production data.

**Location:** `tests/fixtures/`

**Example fixtures:**
- Test users
- Test businesses
- Test clients
- Test quotations
- Test invoices
- Test payments

### Test Database

Use a separate Supabase project for testing:

```bash
# In .env.test
SUPABASE_URL=https://your-test-project.supabase.co
SUPABASE_ANON_KEY=your-test-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-test-service-role-key
```

Reset test database before each test run:

```bash
supabase db reset --db-url $TEST_DATABASE_URL
```

### Safe Test Accounts

**Payment Providers:**

**Paystack Test:**
- Use test mode keys
- Use test cards from Paystack documentation
- Example test card: `4084 0640 0000 0000` (success)
- Example test card: `4084 0640 0000 0001` (fail)

**M-Pesa Sandbox:**
- Use sandbox credentials
- Use sandbox shortcodes
- Test with small amounts (KES 1-10)

**Email Providers:**
- Use test mode if available
- Send to test email addresses only
- Example: `test@yourdomain.com`

## Critical Test Scenarios

### 1. Financial Calculations

**Test Cases:**
- Line item subtotal calculation
- Discount application (percentage and fixed)
- Tax calculation
- Rounding behavior
- Multi-line item totals
- Currency conversion (if implemented)

**Example:**
```javascript
test('calculates invoice total correctly', () => {
  const lineItems = [
    { quantity: 2, unitPrice: 100 },
    { quantity: 1, unitPrice: 50 }
  ];
  const result = calculateTotal(lineItems, 0.1, 0.16); // 10% discount, 16% tax
  expect(result.total).toBe(270); // (250 - 25) + 36
});
```

### 2. Tenant Isolation

**Test Cases:**
- User cannot access another business's clients
- User cannot access another business's invoices
- RLS policies enforced correctly
- Cross-tenant API calls blocked

**Example:**
```javascript
test('user cannot access another business data', async () => {
  const user1 = await createTestUser();
  const business1 = await createTestBusiness(user1.id);
  const user2 = await createTestUser();
  const business2 = await createTestBusiness(user2.id);
  
  const clients = await getClients(user1, business2.id);
  expect(clients).toEqual([]);
});
```

### 3. Authentication and Authorization

**Test Cases:**
- User can register and login
- Invalid credentials rejected
- Session tokens validated
- Permissions enforced on API endpoints
- Role-based access control

### 4. Payment Integration

**Test Cases:**
- Payment initiation with test provider
- Webhook signature verification
- Idempotency key prevents duplicate charges
- Payment status updates correctly
- Failed payments handled gracefully

**Important:** Use provider sandbox/test mode only.

### 5. Quotation to Invoice Flow

**Test Cases:**
- Quotation creation saves correctly
- Quotation can be finalized
- Quotation acceptance recorded
- Accepted quotation converts to invoice
- Line items preserved during conversion
- Invoice numbering unique

### 6. PDF Generation

**Test Cases:**
- PDF generates successfully
- PDF matches preview
- PDF includes all required elements
- Multi-page PDFs render correctly
- PDF is downloadable

### 7. Secure Document Sharing

**Test Cases:**
- Public token is high-entropy
- Public link works for valid token
- Invalid token rejected
- Token scoped to correct business
- Token can be revoked

### 8. Email Sending

**Test Cases:**
- Email composed correctly
- Template variables replaced
- Email sent successfully
- Delivery tracked
- Failed sends retried

## Known Test Limitations

### Payment Provider Tests

**Limitation:** Cannot test actual money movement in automated tests

**Mitigation:**
- Use provider sandbox environments
- Use test adapters that simulate provider responses
- Manual testing with small amounts in sandbox

### Email Delivery Tests

**Limitation:** Cannot verify actual email delivery in automated tests

**Mitigation:**
- Use email provider test mode
- Verify API call succeeds
- Manual verification of test emails

### PDF Rendering Tests

**Limitation:** Visual PDF quality requires manual review

**Mitigation:**
- Automated tests verify PDF generation succeeds
- Automated tests verify PDF contains required text
- Manual review of PDF layout and quality

### Cross-Browser Tests

**Limitation:** Automated tests may not cover all browsers

**Mitigation:**
- Test in Chrome, Firefox, Safari, Edge
- Use browser testing service (BrowserStack, etc.)
- Manual testing for critical flows

## Test Organization

### Directory Structure

```
tests/
├── unit/
│   ├── calculations.spec.js
│   ├── validation.spec.js
│   └── utils.spec.js
├── integration/
│   ├── auth.spec.js
│   ├── database.spec.js
│   └── api.spec.js
├── e2e/
│   ├── onboarding.spec.js
│   ├── quotations.spec.js
│   ├── invoices.spec.js
│   └── payments.spec.js
├── fixtures/
│   ├── users.json
│   ├── businesses.json
│   └── clients.json
└── helpers/
    ├── auth.js
    ├── database.js
    └── api.js
```

## Continuous Integration

### GitHub Actions (Example)

```yaml
name: Tests
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
      - run: npm install
      - run: npm test
      - run: npm run test:coverage
```

## Test Reporting

### Coverage Reports

Generate coverage report:
```bash
npm test -- --coverage
```

Upload to coverage service (Codecov, Coveralls, etc.) if configured.

### Test Results

Test results output to console and JUnit XML format for CI integration.

## Manual Testing Checklist

Before releasing a feature:

- [ ] Feature works in development environment
- [ ] Feature works in staging environment
- [ ] Mobile layout tested on actual devices
- [ ] PDF quality reviewed manually
- [ ] Email delivery verified manually
- [ ] Payment flow tested in sandbox
- [ ] Accessibility tested with screen reader
- [ ] Performance acceptable
- [ ] Error states tested
- [ ] Security review completed

## Troubleshooting Tests

### Tests Failing Due to Database State

**Problem:** Tests fail because of dirty database state

**Solution:**
```bash
supabase db reset --db-url $TEST_DATABASE_URL
```

### Tests Failing Due to Environment Variables

**Problem:** Tests fail because environment variables not set

**Solution:**
```bash
cp .env.example .env.test
# Edit .env.test with test values
export NODE_ENV=test
npm test
```

### Flaky Tests

**Problem:** Tests pass sometimes, fail sometimes

**Solutions:**
- Add retries for network-dependent tests
- Use deterministic test data
- Fix race conditions
- Increase timeouts for slow operations

## Test Maintenance

### When to Update Tests

- When adding new features
- When fixing bugs
- When changing API contracts
- When modifying database schema
- When updating dependencies

### Test Debt

Track test debt in `docs/IMPLEMENTATION-STATUS.md`:
- Features without tests
- Tests with known issues
- Tests needing refactoring

---

**Status:** Testing strategy documented  
**Last updated:** October 20, 2026
