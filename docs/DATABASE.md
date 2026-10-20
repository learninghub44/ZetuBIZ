# Database Schema

This document describes the database schema, relationships, migrations, and Row Level Security (RLS) policies for ZetuBIZ.

## Database System

- **Database:** Supabase PostgreSQL
- **Schema Version:** 1.0
- **Migration Tool:** Supabase CLI

## Schema Overview

The database is designed with multi-tenancy as a core principle. Every business table includes a `business_id` foreign key to enforce tenant isolation.

## Core Tables

### Users & Authentication

#### users
```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email TEXT UNIQUE NOT NULL,
  password_hash TEXT NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE default NOW()
);
```

#### sessions
```sql
CREATE TABLE sessions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  token TEXT UNIQUE NOT NULL,
  expires_at TIMESTAMP WITH TIME ZONE NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### Business & Tenancy

#### businesses
```sql
CREATE TABLE businesses (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name TEXT NOT NULL,
  legal_name TEXT,
  category TEXT NOT NULL,
  business_type TEXT NOT NULL,
  country TEXT NOT NULL DEFAULT 'KE',
  region TEXT,
  business_email TEXT,
  business_phone TEXT,
  business_address TEXT,
  website TEXT,
  tax_id TEXT,
  default_currency TEXT NOT NULL DEFAULT 'KES',
  timezone TEXT NOT NULL DEFAULT 'Africa/Nairobi',
  date_format TEXT NOT NULL DEFAULT 'DD/MM/YYYY',
  owner_id UUID NOT NULL REFERENCES users(id),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(name, owner_id)
);
```

#### business_memberships
```sql
CREATE TABLE business_memberships (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  role TEXT NOT NULL CHECK (role IN ('owner', 'admin', 'finance', 'manager', 'staff', 'viewer')),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(business_id, user_id)
);
```

#### business_settings
```sql
CREATE TABLE business_settings (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  settings_json JSONB NOT NULL DEFAULT '{}',
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(business_id)
);
```

#### branding
```sql
CREATE TABLE branding (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  logo_url TEXT,
  primary_color TEXT,
  secondary_color TEXT,
  font_family TEXT,
  document_style TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(business_id)
);
```

#### onboarding_progress
```sql
CREATE TABLE onboarding_progress (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  business_id UUID REFERENCES businesses(id) ON DELETE SET NULL,
  current_step INTEGER NOT NULL DEFAULT 1,
  completed_steps JSONB NOT NULL DEFAULT '[]',
  business_category TEXT,
  team_size TEXT,
  workflow_selections JSONB NOT NULL DEFAULT '[]',
  referral_source TEXT,
  referral_details TEXT,
  status TEXT NOT NULL DEFAULT 'in_progress',
  last_updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  completed_at TIMESTAMP WITH TIME ZONE,
  UNIQUE(user_id)
);
```

### Clients

#### clients
```sql
CREATE TABLE clients (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  name TEXT NOT NULL,
  client_type TEXT NOT NULL CHECK (client_type IN ('individual', 'business')),
  email TEXT,
  phone TEXT,
  billing_address TEXT,
  tax_id TEXT,
  notes TEXT,
  archived_at TIMESTAMP WITH TIME ZONE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### client_contacts
```sql
CREATE TABLE client_contacts (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  client_id UUID NOT NULL REFERENCES clients(id) ON DELETE CASCADE,
  name TEXT NOT NULL,
  email TEXT,
  phone TEXT,
  is_primary BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### Projects

#### projects
```sql
CREATE TABLE projects (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  client_id UUID REFERENCES clients(id) ON DELETE SET NULL,
  name TEXT NOT NULL,
  description TEXT,
  status TEXT NOT NULL DEFAULT 'active' CHECK (status IN ('draft', 'active', 'on_hold', 'completed', 'cancelled')),
  start_date DATE,
  end_date DATE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### milestones
```sql
CREATE TABLE milestones (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  project_id UUID NOT NULL REFERENCES projects(id) ON DELETE CASCADE,
  name TEXT NOT NULL,
  description TEXT,
  status TEXT NOT NULL DEFAULT 'pending' CHECK (status IN ('pending', 'in_progress', 'completed', 'cancelled')),
  due_date DATE,
  completed_at TIMESTAMP WITH TIME ZONE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### project_assignments
```sql
CREATE TABLE project_assignments (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  project_id UUID NOT NULL REFERENCES projects(id) ON DELETE CASCADE,
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  role TEXT NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(project_id, user_id)
);
```

### Quotations

#### quotations
```sql
CREATE TABLE quotations (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  client_id UUID NOT NULL REFERENCES clients(id) ON DELETE CASCADE,
  project_id UUID REFERENCES projects(id) ON DELETE SET NULL,
  number TEXT NOT NULL,
  status TEXT NOT NULL DEFAULT 'draft' CHECK (status IN ('draft', 'sent', 'viewed', 'accepted', 'rejected', 'expired', 'converted', 'cancelled')),
  title TEXT,
  reference TEXT,
  issue_date DATE NOT NULL,
  validity_date DATE NOT NULL,
  subtotal DECIMAL(19,4) NOT NULL DEFAULT 0,
  discount_amount DECIMAL(19,4) NOT NULL DEFAULT 0,
  tax_amount DECIMAL(19,4) NOT NULL DEFAULT 0,
  total DECIMAL(19,4) NOT NULL DEFAULT 0,
  currency TEXT NOT NULL,
  notes TEXT,
  terms TEXT,
  public_token TEXT UNIQUE,
  template_id TEXT,
  finalized_at TIMESTAMP WITH TIME ZONE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(business_id, number)
);
```

#### quotation_line_items
```sql
CREATE TABLE quotation_line_items (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  quotation_id UUID NOT NULL REFERENCES quotations(id) ON DELETE CASCADE,
  description TEXT NOT NULL,
  quantity DECIMAL(19,4) NOT NULL,
  unit TEXT,
  unit_price DECIMAL(19,4) NOT NULL,
  discount_percent DECIMAL(5,2) DEFAULT 0,
  tax_percent DECIMAL(5,2) DEFAULT 0,
  amount DECIMAL(19,4) NOT NULL,
  sort_order INTEGER DEFAULT 0
);
```

#### quotation_revisions
```sql
CREATE TABLE quotation_revisions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  quotation_id UUID NOT NULL REFERENCES quotations(id) ON DELETE CASCADE,
  revision_number INTEGER NOT NULL,
  snapshot_json JSONB NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(quotation_id, revision_number)
);
```

#### quotation_acceptances
```sql
CREATE TABLE quotation_acceptances (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  quotation_id UUID NOT NULL REFERENCES quotations(id) ON DELETE CASCADE,
  accepted_by TEXT,
  accepted_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  evidence TEXT,
  ip_address TEXT,
  user_agent TEXT
);
```

### Invoices

#### invoices
```sql
CREATE TABLE invoices (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  client_id UUID NOT NULL REFERENCES clients(id) ON DELETE CASCADE,
  project_id UUID REFERENCES projects(id) ON DELETE SET NULL,
  quotation_id UUID REFERENCES quotations(id) ON DELETE SET NULL,
  number TEXT NOT NULL,
  status TEXT NOT NULL DEFAULT 'draft' CHECK (status IN ('draft', 'issued', 'sent', 'partially_paid', 'paid', 'overdue', 'void', 'cancelled')),
  issue_date DATE NOT NULL,
  due_date DATE NOT NULL,
  purchase_order_ref TEXT,
  subtotal DECIMAL(19,4) NOT NULL DEFAULT 0,
  discount_amount DECIMAL(19,4) NOT NULL DEFAULT 0,
  tax_amount DECIMAL(19,4) NOT NULL DEFAULT 0,
  total DECIMAL(19,4) NOT NULL DEFAULT 0,
  balance DECIMAL(19,4) NOT NULL DEFAULT 0,
  currency TEXT NOT NULL,
  notes TEXT,
  terms TEXT,
  payment_instructions TEXT,
  public_token TEXT UNIQUE,
  template_id TEXT,
  finalized_at TIMESTAMP WITH TIME ZONE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(business_id, number)
);
```

#### invoice_line_items
```sql
CREATE TABLE invoice_line_items (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  invoice_id UUID NOT NULL REFERENCES invoices(id) ON DELETE CASCADE,
  description TEXT NOT NULL,
  quantity DECIMAL(19,4) NOT NULL,
  unit TEXT,
  unit_price DECIMAL(19,4) NOT NULL,
  discount_percent DECIMAL(5,2) DEFAULT 0,
  tax_percent DECIMAL(5,2) DEFAULT 0,
  amount DECIMAL(19,4) NOT NULL,
  sort_order INTEGER DEFAULT 0
);
```

#### invoice_snapshots
```sql
CREATE TABLE invoice_snapshots (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  invoice_id UUID NOT NULL REFERENCES invoices(id) ON DELETE CASCADE,
  snapshot_json JSONB NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### credit_notes
```sql
CREATE TABLE credit_notes (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  invoice_id UUID NOT NULL REFERENCES invoices(id) ON DELETE CASCADE,
  number TEXT NOT NULL,
  amount DECIMAL(19,4) NOT NULL,
  reason TEXT NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(business_id, number)
);
```

### Payments

#### payments
```sql
CREATE TABLE payments (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  invoice_id UUID NOT NULL REFERENCES invoices(id) ON DELETE CASCADE,
  provider TEXT NOT NULL,
  provider_transaction_id TEXT,
  amount DECIMAL(19,4) NOT NULL,
  currency TEXT NOT NULL,
  status TEXT NOT NULL DEFAULT 'pending' CHECK (status IN ('pending', 'processing', 'successful', 'failed', 'cancelled', 'expired', 'uncertain')),
  payment_method TEXT,
  initiated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  completed_at TIMESTAMP WITH TIME ZONE,
  failure_reason TEXT,
  idempotency_key TEXT UNIQUE
);
```

#### payment_allocations
```sql
CREATE TABLE payment_allocations (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  payment_id UUID NOT NULL REFERENCES payments(id) ON DELETE CASCADE,
  invoice_id UUID NOT NULL REFERENCES invoices(id) ON DELETE CASCADE,
  amount DECIMAL(19,4) NOT NULL,
  allocated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### provider_configs
```sql
CREATE TABLE provider_configs (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  provider TEXT NOT NULL,
  credentials_encrypted TEXT NOT NULL,
  test_mode BOOLEAN DEFAULT TRUE,
  status TEXT NOT NULL DEFAULT 'not_configured' CHECK (status IN ('not_configured', 'testing', 'connected', 'degraded', 'authentication_failed', 'disconnected')),
  last_verified_at TIMESTAMP WITH TIME ZONE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(business_id, provider)
);
```

### Expenses

#### expenses
```sql
CREATE TABLE expenses (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  client_id UUID REFERENCES clients(id) ON DELETE SET NULL,
  description TEXT NOT NULL,
  amount DECIMAL(19,4) NOT NULL,
  currency TEXT NOT NULL,
  date DATE NOT NULL,
  category_id UUID REFERENCES expense_categories(id) ON DELETE SET NULL,
  payment_method TEXT,
  document_url TEXT,
  document_name TEXT,
  notes TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### expense_categories
```sql
CREATE TABLE expense_categories (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  name TEXT NOT NULL,
  description TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### Reconciliation

#### reconciliation_imports
```sql
CREATE TABLE reconciliation_imports (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  file_name TEXT NOT NULL,
  file_url TEXT,
  status TEXT NOT NULL DEFAULT 'processing' CHECK (status IN ('processing', 'completed', 'failed')),
  imported_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  imported_by UUID REFERENCES users(id)
);
```

#### imported_transactions
```sql
CREATE TABLE imported_transactions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  import_id UUID NOT NULL REFERENCES reconciliation_imports(id) ON DELETE CASCADE,
  transaction_date DATE NOT NULL,
  description TEXT,
  amount DECIMAL(19,4) NOT NULL,
  reference TEXT,
  matched_payment_id UUID REFERENCES payments(id),
  match_status TEXT NOT NULL DEFAULT 'unmatched' CHECK (match_status IN ('unmatched', 'matched', 'duplicate', 'ambiguous')),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### match_decisions
```sql
CREATE TABLE match_decisions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  transaction_id UUID NOT NULL REFERENCES imported_transactions(id) ON DELETE CASCADE,
  payment_id UUID REFERENCES payments(id),
  decided_by UUID NOT NULL REFERENCES users(id),
  decided_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  reason TEXT
);
```

### Reports & Exports

#### report_exports
```sql
CREATE TABLE report_exports (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  type TEXT NOT NULL,
  parameters JSONB NOT NULL,
  file_url TEXT,
  file_name TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  created_by UUID NOT NULL REFERENCES users(id)
);
```

### Notifications

#### notifications
```sql
CREATE TABLE notifications (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  type TEXT NOT NULL,
  title TEXT NOT NULL,
  message TEXT NOT NULL,
  read_at TIMESTAMP WITH TIME ZONE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### notification_preferences
```sql
CREATE TABLE notification_preferences (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  preferences_json JSONB NOT NULL DEFAULT '{}',
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(business_id, user_id)
);
```

### Audit

#### audit_events
```sql
CREATE TABLE audit_events (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  actor UUID REFERENCES users(id),
  tenant_id UUID REFERENCES businesses(id),
  action TEXT NOT NULL,
  target_type TEXT,
  target_id UUID,
  timestamp TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  outcome TEXT NOT NULL CHECK (outcome IN ('success', 'failure')),
  metadata_json JSONB NOT NULL DEFAULT '{}',
  ip_address TEXT,
  user_agent TEXT
);
```

### Platform Admin

#### admin_users
```sql
CREATE TABLE admin_users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email TEXT UNIQUE NOT NULL,
  password_hash TEXT NOT NULL,
  role TEXT NOT NULL CHECK (role IN ('super_admin', 'admin', 'support')),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### tenant_status
```sql
CREATE TABLE tenant_status (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  status TEXT NOT NULL DEFAULT 'active' CHECK (status IN ('active', 'suspended', 'restricted')),
  restricted_at TIMESTAMP WITH TIME ZONE,
  restricted_by UUID REFERENCES admin_users(id),
  reason TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(business_id)
);
```

#### feature_flags
```sql
CREATE TABLE feature_flags (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name TEXT UNIQUE NOT NULL,
  enabled BOOLEAN NOT NULL DEFAULT FALSE,
  description TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### background_jobs
```sql
CREATE TABLE background_jobs (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  type TEXT NOT NULL,
  payload JSONB NOT NULL,
  status TEXT NOT NULL DEFAULT 'pending' CHECK (status IN ('pending', 'processing', 'completed', 'failed')),
  attempts INTEGER DEFAULT 0,
  error_message TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  completed_at TIMESTAMP WITH TIME ZONE
);
```

#### webhook_deliveries
```sql
CREATE TABLE webhook_deliveries (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  provider TEXT NOT NULL,
  payload JSONB NOT NULL,
  status TEXT NOT NULL DEFAULT 'pending' CHECK (status IN ('pending', 'delivered', 'failed')),
  attempts INTEGER DEFAULT 0,
  delivered_at TIMESTAMP WITH TIME ZONE,
  error_message TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

## Indexes

Create indexes for performance:

```sql
-- Business isolation indexes
CREATE INDEX idx_businesses_owner ON businesses(owner_id);
CREATE INDEX idx_clients_business ON clients(business_id);
CREATE INDEX idx_projects_business ON projects(business_id);
CREATE INDEX idx_quotations_business ON quotations(business_id);
CREATE INDEX idx_invoices_business ON invoices(business_id);
CREATE INDEX idx_payments_business ON payments(business_id);
CREATE INDEX idx_expenses_business ON expenses(business_id);

-- Foreign key indexes
CREATE INDEX idx_quotations_client ON quotations(client_id);
CREATE INDEX idx_invoices_client ON invoices(client_id);
CREATE INDEX idx_payments_invoice ON payments(invoice_id);

-- Status indexes
CREATE INDEX idx_quotations_status ON quotations(business_id, status);
CREATE INDEX idx_invoices_status ON invoices(business_id, status);
CREATE INDEX idx_payments_status ON payments(business_id, status);

-- Date indexes
CREATE INDEX idx_invoices_due_date ON invoices(due_date);
CREATE INDEX idx_quotations_validity ON quotations(validity_date);

-- Search indexes
CREATE INDEX idx_clients_name ON clients(business_id, name);
CREATE INDEX idx_quotations_number ON quotations(business_id, number);
CREATE INDEX idx_invoices_number ON invoices(business_id, number);
```

## Row Level Security (RLS) Policies

### Enable RLS

```sql
ALTER TABLE businesses ENABLE ROW LEVEL SECURITY;
ALTER TABLE clients ENABLE ROW LEVEL SECURITY;
ALTER TABLE quotations ENABLE ROW LEVEL SECURITY;
ALTER TABLE invoices ENABLE ROW LEVEL SECURITY;
-- Enable on all business tables
```

### Example RLS Policies

#### Business Access
```sql
-- Users can view businesses they are members of
CREATE POLICY "Users can view own businesses"
ON businesses
FOR SELECT
USING (
  id IN (
    SELECT business_id 
    FROM business_memberships 
    WHERE user_id = auth.uid()
  )
);

-- Users can insert businesses (for onboarding)
CREATE POLICY "Users can create businesses"
ON businesses
FOR INSERT
WITH CHECK (
  owner_id = auth.uid()
);
```

#### Client Access
```sql
-- Users can view clients from their businesses
CREATE POLICY "Users can view business clients"
ON clients
FOR SELECT
USING (
  business_id IN (
    SELECT business_id 
    FROM business_memberships 
    WHERE user_id = auth.uid()
  )
);

-- Users can insert clients for their businesses
CREATE POLICY "Users can create clients"
ON clients
FOR INSERT
WITH CHECK (
  business_id IN (
    SELECT business_id 
    FROM business_memberships 
    WHERE user_id = auth.uid()
  )
);
```

## Migration Instructions

### Initial Setup

```bash
# Install Supabase CLI
npm install -g supabase

# Initialize Supabase
supabase init

# Link to your project
supabase link --project-ref YOUR_PROJECT_REF

# Push migrations
supabase db push

# Generate types
supabase gen types typescript --local > src/types/supabase.ts
```

### Creating New Migrations

```bash
# Create a new migration
supabase migration new add_new_feature

# Edit the migration file
# Add your SQL changes

# Push the migration
supabase db push
```

### Resetting Database (Development Only)

```bash
# WARNING: This deletes all data
supabase db reset
```

### Rolling Back Migrations

```bash
# View migration history
supabase migration list

# Roll back to specific migration
supabase migration down
```

## Data Integrity

### Constraints
- All foreign keys have ON DELETE CASCADE or SET NULL as appropriate
- Unique constraints on critical fields (business name per owner, invoice numbers)
- CHECK constraints on enums and status fields
- NOT NULL constraints on required fields

### Transactions
- Use transactions for multi-table operations
- Example: Creating a quotation with line items

```sql
BEGIN;
INSERT INTO quotations (...) VALUES (...);
INSERT INTO quotation_line_items (...) VALUES (...);
COMMIT;
```

## Backup and Recovery

### Backups
- Supabase provides automated daily backups
- Point-in-time recovery available (7 days)
- Export database schema and data regularly

### Recovery
```bash
# Restore from backup via Supabase dashboard
# Or use point-in-time recovery
```

---

**Status:** Schema designed - implementation pending  
**Last updated:** October 20, 2026
