# Unified Engine Strategy
## Building a Single Core Engine for Both Platforms

**Analysis Date:** February 17, 2026
**Platforms:**
- Platform A: Multi-Tenant E-Commerce Platform (Shopify-like)
- Platform B: Tax & Invoicing Platform (GST/Invoice management)

---

## 1. Executive Answer: Yes — A Shared Engine Makes Strong Sense

The two platforms overlap significantly at the infrastructure and business-logic layer. Both are **multi-tenant SaaS products** that serve organizations, manage users with roles, handle products/services, process transactions, generate documents, send notifications, and produce reports. Building a **shared core engine** (sometimes called a "platform kernel" or "SaaS backbone") and deploying application-specific modules on top of it is the right architectural strategy.

This is the same principle used by platforms like Odoo (shared kernel, modular apps), Salesforce (core platform + app cloud), and Zoho (shared identity/org layer across 45+ products).

---

## 2. The Two-Layer Mental Model

Think of it as two layers:

```
┌─────────────────────────────────────────────────────────┐
│          E-COMMERCE APP          │    TAX & INVOICING APP│
│  Catalogues, Cart, Checkout,     │  GST, Returns, GSTR,  │
│  Shipping, Storefront themes     │  HSN codes, ITC, Tally│
├─────────────────────────────────────────────────────────┤
│                                                         │
│              SHARED CORE ENGINE                         │
│                                                         │
│  Auth · Users · Orgs · Products · Customers · Invoices  │
│  Payments · Notifications · Reports · API · Audit logs  │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

The **Shared Core Engine** is built once, maintained once, tested once, and both apps consume it via internal APIs or shared libraries. Each app then adds only its **unique domain logic** on top.

---

## 3. Complete Module-by-Module Analysis

### 3.1 ✅ FULLY SHARED — Build Once, Use in Both

These modules are **identical or near-identical** in both platforms. Build them once in the core engine.

---

#### MODULE 1: Identity & Authentication
**Why shared:** Both platforms need user login, email verification, password management, session handling, MFA, and social login. The logic is 100% identical.

| Feature | E-Commerce | Tax & Invoicing |
|---------|-----------|-----------------|
| User registration & login | ✓ | ✓ |
| Email verification | ✓ | ✓ |
| Password reset / change | ✓ | ✓ |
| Session management | ✓ | ✓ |
| Remember me | ✓ | ✓ |
| Multi-factor authentication | ✓ | ✓ |
| Social login (Google, etc.) | ✓ | ✓ |
| Login history | ✓ | ✓ |
| Account deactivation | ✓ | ✓ |

**Engine design:** Single `auth-service` with JWT tokens, OAuth 2.0, and tenant context embedded in token claims.

---

#### MODULE 2: Multi-Tenancy / Organization Management
**Why shared:** Both platforms are multi-tenant. Both allow one user to belong to multiple organizations and switch between them. The tenancy engine is identical.

| Feature | E-Commerce | Tax & Invoicing |
|---------|-----------|-----------------|
| Create organization/tenant | ✓ (Merchant account) | ✓ (Organization) |
| Multiple orgs per user | ✓ | ✓ |
| Switch between orgs | ✓ | ✓ |
| Org profile & branding | ✓ | ✓ |
| Tenant data isolation | ✓ | ✓ |
| Org settings & config | ✓ | ✓ |
| Org deletion/archiving | ✓ | ✓ |
| Subscription per org | ✓ | ✓ |

**Engine design:** Single `tenant-service` with `tenant_id` propagation across all data models. Org switcher is a shared UI component.

---

#### MODULE 3: User Roles & Permissions (RBAC)
**Why shared:** Both platforms use role-based access. The RBAC engine (roles, permissions, assignment, enforcement) is the same mechanism — only the role names and permission labels differ.

| Feature | E-Commerce | Tax & Invoicing |
|---------|-----------|-----------------|
| Role creation & assignment | ✓ | ✓ |
| Permission management | ✓ | ✓ |
| Custom roles | ✓ | ✓ |
| Access level configuration | ✓ | ✓ |
| Feature restrictions per role | ✓ | ✓ |

**Engine design:** Generic RBAC engine where permissions are string identifiers (e.g., `invoices:create`, `products:delete`). Each app registers its own permission strings. The enforcement middleware is shared.

---

#### MODULE 4: User Management (Team Members)
**Why shared:** Inviting users, managing profiles, activity logs, and user-org relationships are identical in both platforms.

| Feature | E-Commerce | Tax & Invoicing |
|---------|-----------|-----------------|
| Invite users via email | ✓ | ✓ |
| User profile management | ✓ | ✓ |
| Profile picture upload | ✓ | ✓ |
| Contact info management | ✓ | ✓ |
| User preferences | ✓ | ✓ |
| User activity logs | ✓ | ✓ |
| Remove/deactivate users | ✓ | ✓ |

---

#### MODULE 5: Customer Management
**Why shared:** Both platforms manage customers with profiles, addresses, contact info, tags, notes, payment terms, credit limits, and history. The data model and CRUD operations are essentially the same.

| Feature | E-Commerce | Tax & Invoicing |
|---------|-----------|-----------------|
| Create/edit/delete customers | ✓ | ✓ |
| Customer search & filtering | ✓ | ✓ |
| Customer profiles | ✓ | ✓ |
| Multiple addresses (billing/shipping) | ✓ | ✓ |
| Customer tags & labels | ✓ | ✓ |
| Customer notes & comments | ✓ | ✓ |
| CSV import/export | ✓ | ✓ |
| Bulk operations | ✓ | ✓ |
| Payment terms (Net 30, etc.) | ✓ | ✓ |
| Credit limit tracking | ✓ | ✓ |
| GSTIN storage | ✓ | ✓ |
| Customer groups | ✓ | ✓ |
| Archive/restore | ✓ | ✓ |

**Note:** Tax-specific fields (GSTIN validation, reverse charge eligibility) are an **extension** of the shared customer model, not a separate module.

---

#### MODULE 6: Product / Service Catalog
**Why shared:** Both platforms manage items that are sold — the E-Commerce platform calls them products, the Tax platform calls them products/services. The core catalog structure (name, description, SKU, price, tax rate, categories, images) is shared.

| Feature | E-Commerce | Tax & Invoicing |
|---------|-----------|-----------------|
| Create/edit/delete products | ✓ | ✓ |
| Product search & filtering | ✓ | ✓ |
| Categories & subcategories | ✓ | ✓ |
| Product tags | ✓ | ✓ |
| SKU management | ✓ | ✓ |
| Product images | ✓ | ✓ |
| Base price & pricing tiers | ✓ | ✓ |
| Tax rate assignment | ✓ | ✓ |
| CSV import/export | ✓ | ✓ |
| Bulk operations | ✓ | ✓ |
| Product variants | ✓ | ✓ |
| Unit of measurement | ✓ | ✓ |
| Product archive/restore | ✓ | ✓ |
| Custom attributes | ✓ | ✓ |
| Discount rules | ✓ | ✓ |

**Tax-specific extensions (Tax platform only):** HSN/SAC codes, GST rate category (0/5/12/18/28%), product type (goods/services), nil-rated flag.

---

#### MODULE 7: Invoice Engine
**Why shared:** This is the most powerful overlap. The E-Commerce platform generates invoices from orders. The Tax platform's *entire core* is invoice management. The invoice data model, PDF generation, numbering, line items, tax calculations, status workflow, delivery, and reminders are all shareable.

| Feature | E-Commerce | Tax & Invoicing |
|---------|-----------|-----------------|
| Invoice creation | ✓ (from orders) | ✓ (direct) |
| Line item management | ✓ | ✓ |
| Discounts (line & overall) | ✓ | ✓ |
| Tax calculation | ✓ | ✓ |
| Shipping/additional charges | ✓ | ✓ |
| Invoice numbering & series | ✓ | ✓ |
| Draft → Sent → Paid workflow | ✓ | ✓ |
| PDF generation | ✓ | ✓ |
| Email with PDF attachment | ✓ | ✓ |
| Invoice templates | ✓ | ✓ |
| Recurring invoices | ✓ | ✓ |
| Invoice duplication | ✓ | ✓ |
| Payment reminders | ✓ | ✓ |
| Overdue tracking | ✓ | ✓ |
| Void/cancel invoices | ✓ | ✓ |
| Multi-currency support | ✓ | ✓ |
| Invoice notes & attachments | ✓ | ✓ |
| Amount in words | ✓ | ✓ |
| Invoice listing & filtering | ✓ | ✓ |

---

#### MODULE 8: Payment Engine
**Why shared:** Recording payments, tracking partial payments, processing via gateways, generating receipts, and handling refunds is identical in both platforms.

| Feature | E-Commerce | Tax & Invoicing |
|---------|-----------|-----------------|
| Record payments | ✓ | ✓ |
| Multiple payment methods | ✓ | ✓ |
| Partial payment support | ✓ | ✓ |
| Payment allocation to invoices | ✓ | ✓ |
| Payment reference & notes | ✓ | ✓ |
| Razorpay integration | ✓ | ✓ |
| Stripe integration | ✓ | ✓ |
| PayPal integration | ✓ | ✓ |
| Payment link generation | ✓ | ✓ |
| Payment receipt generation | ✓ | ✓ |
| Refunds & credit notes | ✓ | ✓ |
| Payment history | ✓ | ✓ |
| Overpayment handling | ✓ | ✓ |

---

#### MODULE 9: Tax Calculation Engine
**Why shared:** Both platforms calculate taxes on line items. The E-Commerce platform needs GST/VAT for invoices. The Tax platform's core is GST. The calculation logic (rate lookup, CGST/SGST/IGST split, intrastate vs interstate) is shared.

| Feature | E-Commerce | Tax & Invoicing |
|---------|-----------|-----------------|
| Tax rate assignment per product | ✓ | ✓ |
| Tax-inclusive/exclusive pricing | ✓ | ✓ |
| Multi-rate tax support | ✓ | ✓ |
| Tax calculation on invoices | ✓ | ✓ |
| Tax exemption handling | ✓ | ✓ |
| Discount-adjusted tax | ✓ | ✓ |
| CGST / SGST / IGST split | ✓ | ✓ |
| Intrastate vs interstate logic | ✓ | ✓ |
| Reverse charge mechanism | ✓ | ✓ |
| Tax rounding rules | ✓ | ✓ |

---

#### MODULE 10: Notification & Communication Engine
**Why shared:** Email/SMS/WhatsApp/push notification sending, template management, delivery tracking, and preference management is identical infrastructure in both.

| Feature | E-Commerce | Tax & Invoicing |
|---------|-----------|-----------------|
| Email notifications | ✓ | ✓ |
| SMS notifications | ✓ | ✓ |
| WhatsApp messaging | ✓ | ✓ |
| Email template management | ✓ | ✓ |
| Template variables / personalization | ✓ | ✓ |
| Notification preferences per user | ✓ | ✓ |
| Delivery status tracking | ✓ | ✓ |
| Bulk/campaign sending | ✓ | ✓ |
| SendGrid / Mailgun / SMTP integration | ✓ | ✓ |
| Twilio / SMS gateway integration | ✓ | ✓ |

---

#### MODULE 11: Reporting & Analytics Engine
**Why shared:** The infrastructure for generating reports — data aggregation, chart rendering, date range filters, export (PDF/CSV/Excel), scheduled delivery — is identical. The specific report types differ by app.

| Feature | E-Commerce | Tax & Invoicing |
|---------|-----------|-----------------|
| Revenue reports | ✓ | ✓ |
| Customer-based reports | ✓ | ✓ |
| Product/service-based reports | ✓ | ✓ |
| Payment reports | ✓ | ✓ |
| Export to PDF/CSV/Excel | ✓ | ✓ |
| Scheduled report delivery | ✓ | ✓ |
| Custom date range selection | ✓ | ✓ |
| KPI dashboard widgets | ✓ | ✓ |
| Charts (bar, line, pie) | ✓ | ✓ |
| Aging reports (AR) | ✓ | ✓ |

---

#### MODULE 12: Subscription & Billing Management
**Why shared:** Both platforms are SaaS products with subscription tiers, feature limits, trial periods, billing history, and plan upgrades.

| Feature | E-Commerce | Tax & Invoicing |
|---------|-----------|-----------------|
| Subscription plans | ✓ | ✓ |
| Trial period management | ✓ | ✓ |
| Plan upgrade/downgrade | ✓ | ✓ |
| Billing history | ✓ | ✓ |
| Feature limits per plan | ✓ | ✓ |
| Renewal notifications | ✓ | ✓ |
| Usage analytics | ✓ | ✓ |
| Subscription cancellation | ✓ | ✓ |

---

#### MODULE 13: API Management & Webhooks
**Why shared:** API key generation, rate limiting, usage tracking, webhook configuration, and OAuth 2.0 are identical infrastructure.

| Feature | E-Commerce | Tax & Invoicing |
|---------|-----------|-----------------|
| API key generation | ✓ | ✓ |
| API key management | ✓ | ✓ |
| API usage tracking | ✓ | ✓ |
| Rate limiting per plan | ✓ | ✓ |
| Webhook configuration | ✓ | ✓ |
| Webhook event logs | ✓ | ✓ |
| OAuth 2.0 token management | ✓ | ✓ |
| API documentation | ✓ | ✓ |

---

#### MODULE 14: Audit Logs & Activity Tracking
**Why shared:** Logging user actions, entity changes, login history, API requests, and security events uses the same mechanism in both platforms.

| Feature | E-Commerce | Tax & Invoicing |
|---------|-----------|-----------------|
| User activity tracking | ✓ | ✓ |
| Entity modification history | ✓ | ✓ |
| Login history | ✓ | ✓ |
| API request logs | ✓ | ✓ |
| Security event logs | ✓ | ✓ |
| Data export logs | ✓ | ✓ |

---

#### MODULE 15: File & Media Management
**Why shared:** Uploading images, generating/storing PDFs, managing attachments — the storage infrastructure is shared.

| Feature | E-Commerce | Tax & Invoicing |
|---------|-----------|-----------------|
| Image upload | ✓ | ✓ |
| PDF generation & storage | ✓ | ✓ |
| Document attachments | ✓ | ✓ |
| File size limits | ✓ | ✓ |
| CDN delivery | ✓ | ✓ |

---

#### MODULE 16: Onboarding Engine
**Why shared:** Both platforms have an onboarding wizard, setup steps, guided tours, data import, and onboarding checklists. The flow is orchestrated the same way.

| Feature | E-Commerce | Tax & Invoicing |
|---------|-----------|-----------------|
| Setup wizard | ✓ | ✓ |
| Company info collection | ✓ | ✓ |
| Data import wizard (CSV) | ✓ | ✓ |
| Guided tours / tooltips | ✓ | ✓ |
| Onboarding checklist | ✓ | ✓ |
| Progress tracking | ✓ | ✓ |
| Sample data / demo mode | ✓ | ✓ |

---

#### MODULE 17: Settings & Configuration
**Why shared:** Currency, timezone, localization, branding, fiscal year, email config — the settings framework (key-value store scoped per tenant) is shared. Values differ.

| Feature | E-Commerce | Tax & Invoicing |
|---------|-----------|-----------------|
| Currency settings | ✓ | ✓ |
| Timezone & locale | ✓ | ✓ |
| Organization branding | ✓ | ✓ |
| Email SMTP config | ✓ | ✓ |
| Notification preferences | ✓ | ✓ |
| Invoice numbering schemes | ✓ | ✓ |

---

### 3.2 ⚡ PARTIALLY SHARED — Shared Core + App-Specific Extension

These modules have a significant shared foundation but each platform adds its own domain logic on top.

---

#### MODULE A: Product Catalog Extensions

| Shared Core | E-Commerce Extension | Tax Platform Extension |
|-------------|---------------------|----------------------|
| Product CRUD | Variants (size, color) | HSN/SAC codes |
| Categories, tags, SKU | Product images (gallery) | GST rate category |
| Pricing, tax assignment | Inventory tracking | Product type (goods/services) |
| CSV import/export | Shopfront display | Nil-rated / exempt flag |
| Custom attributes | Catalogue assignment | Composition scheme flag |

---

#### MODULE B: Invoice Extensions

| Shared Core | E-Commerce Extension | Tax Platform Extension |
|-------------|---------------------|----------------------|
| Invoice CRUD, line items | Generated from order/cart | Created directly |
| Status workflow | Shipping charges | GST compliance format |
| PDF generation | Order tracking link | CGST/SGST/IGST breakdown |
| Payment reminders | Catalogue reference | E-way bill integration |
| Multi-currency | Fulfillment linking | GSTR-1/2/3B data |
| Recurring invoices | — | Tax period management |

---

#### MODULE C: Customer Extensions

| Shared Core | E-Commerce Extension | Tax Platform Extension |
|-------------|---------------------|----------------------|
| Customer CRUD, addresses | Wishlist, saved carts | GSTIN validation & storage |
| Tags, groups, notes | Order history & reorder | Tax exemption certificates |
| Payment terms, credit limit | Guest checkout | Place of supply |
| CSV import/export | Social login | Reverse charge eligibility |

---

#### MODULE D: Reporting Extensions

| Shared Core | E-Commerce Extension | Tax Platform Extension |
|-------------|---------------------|----------------------|
| Revenue reports | Catalogue performance | GST summary reports |
| Payment reports | Conversion funnel | GSTR-1 / GSTR-3B |
| Customer reports | Cart abandonment | Input tax credit (ITC) |
| Product reports | Shipping reports | State-wise GST |
| Export PDF/CSV | GMV reporting | HSN-wise summary |

---

#### MODULE E: Tax Engine Extensions

| Shared Core | E-Commerce Extension | Tax Platform Extension |
|-------------|---------------------|----------------------|
| Tax rate assignment | VAT / regional tax | CGST/SGST/IGST logic |
| Tax calculation on invoices | Price-inclusive tax | Reverse charge |
| Tax exemptions | — | GSTIN management |
| Multi-rate support | — | Composition scheme |
| — | — | GST returns data |
| — | — | ITC tracking |

---

### 3.3 🔴 PLATFORM-SPECIFIC — Do NOT share, build separately

These modules are entirely unique to one platform with no meaningful overlap.

---

#### E-Commerce Platform Only

| Module | Why Unique |
|--------|-----------|
| **Shop/Storefront Management** | Subdomains, themes, catalogue shareable links — no equivalent in Tax platform |
| **Product Catalogue (browsing)** | Customer-facing catalogue UI, QR codes, public links — not present in Tax platform |
| **Shopping Cart** | Cart state management, cart persistence, abandoned cart — not applicable in Tax platform |
| **Checkout Flow** | Multi-step checkout, shipping method selection — specific to e-commerce |
| **Shipping Management** | Shipping zones, carrier integration, label printing, tracking — not in Tax platform |
| **Fulfillment Management** | Pick/pack/ship workflow, packing slips, delivery tracking |
| **Quote Request System** | Customer-submitted quote requests, quote negotiation workflow |
| **Storefront Themes/Branding** | Customer-facing UI templates, CSS customization |
| **Product Reviews** | Customer ratings and reviews system |
| **Wishlist / Saved Carts** | Customer-specific shopping behavior |
| **Reorder Templates** | Recurring cart templates for B2B |

---

#### Tax & Invoicing Platform Only

| Module | Why Unique |
|--------|-----------|
| **GST Returns (GSTR-1/2/3B)** | Regulatory-specific returns — no equivalent in e-commerce |
| **Input Tax Credit (ITC)** | Tax credit tracking — specific to GST compliance |
| **HSN/SAC Code Database** | Commodity code lookup and mapping |
| **E-way Bill Integration** | Government API for e-way bills |
| **Composition Scheme** | Special GST scheme — specific to India tax law |
| **Customer Portal (Invoice)** | Customer-facing portal for viewing/paying invoices |
| **Accounts Payable / Receivable** | Supplier invoices, payables management |
| **Fiscal Year Management** | Tax period, filing periods |
| **Tally/Accounting Integration** | Accounting software sync |
| **Credit Note Management** | GST-compliant credit notes |
| **GSTIN Verification API** | Government GSTIN lookup |

---

## 4. Summary Map

```
SHARED CORE ENGINE (Build Once)
├── Identity & Auth
├── Multi-Tenancy / Org Management
├── RBAC & Permissions
├── User Management
├── Customer Management (core)
├── Product Catalog (core)
├── Invoice Engine (core)
├── Payment Engine (core)
├── Tax Calculation Engine (core)
├── Notification Engine (email/SMS/WhatsApp)
├── Reporting Engine (infrastructure)
├── Subscription & Billing
├── API Management & Webhooks
├── Audit Logs
├── File & Media Management
├── Onboarding Engine
└── Settings & Configuration

E-COMMERCE APP (Builds on core + adds)
├── Multi-Shop & Storefront
├── Catalogue Management
├── Shopping Cart & Checkout
├── Shipping & Fulfillment
├── Quote Request System
├── Storefront Themes
├── Product Reviews
├── Wishlist / Saved Carts
└── E-Commerce-Specific Reports

TAX & INVOICING APP (Builds on core + adds)
├── GST Returns (GSTR-1/2/3B)
├── Input Tax Credit (ITC)
├── HSN/SAC Code Database
├── E-way Bill Integration
├── Customer Invoice Portal
├── Composition Scheme
├── Accounts Payable/Receivable
├── Fiscal Year Management
├── Tally Integration
├── Credit Note Management
└── Tax-Specific Reports
```

---

## 5. Recommended Architecture Approach

### 5.1 Monorepo with Modular Services

Use a **monorepo** with clearly separated packages:

```
/platform-monorepo
├── /core                        ← Shared engine packages
│   ├── /auth                    ← Authentication service
│   ├── /tenancy                 ← Multi-tenant management
│   ├── /users                   ← User & role management
│   ├── /customers               ← Customer management
│   ├── /products                ← Product catalog
│   ├── /invoices                ← Invoice engine
│   ├── /payments                ← Payment processing
│   ├── /tax-engine              ← Tax calculation
│   ├── /notifications           ← Email/SMS/WhatsApp
│   ├── /reports                 ← Reporting framework
│   ├── /subscriptions           ← SaaS billing
│   ├── /api-management          ← API keys, webhooks
│   ├── /audit-logs              ← Activity tracking
│   ├── /files                   ← Media management
│   ├── /onboarding              ← Setup wizard
│   └── /settings                ← Config management
│
├── /apps
│   ├── /ecommerce               ← E-Commerce specific modules
│   │   ├── /storefront
│   │   ├── /catalogues
│   │   ├── /cart-checkout
│   │   ├── /shipping
│   │   ├── /quotes
│   │   └── /fulfillment
│   │
│   └── /tax-invoicing           ← Tax platform specific modules
│       ├── /gst-returns
│       ├── /itc
│       ├── /hsn-sac
│       ├── /eway-bill
│       ├── /customer-portal
│       └── /accounting
│
├── /frontend
│   ├── /ui-components           ← Shared UI library
│   ├── /ecommerce-dashboard
│   └── /tax-invoicing-dashboard
│
└── /infrastructure
    ├── /api-gateway
    ├── /database
    └── /deployment
```

### 5.2 Shared Database with App Namespacing

Single database with tenant isolation, using namespaced table prefixes or a `app_context` column where needed:

- Core tables: `users`, `organizations`, `customers`, `products`, `invoices`, `payments`
- E-Commerce tables: `shops`, `catalogues`, `carts`, `orders`, `shipments`, `quotes`
- Tax tables: `gst_returns`, `hsn_codes`, `itc_entries`, `eway_bills`, `credit_notes`

### 5.3 Internal API Design

The core engine exposes **internal service APIs** that both apps consume:

```
Auth Service:       POST /auth/login, /auth/register, /auth/verify
Tenant Service:     POST /orgs, GET /orgs/:id, PUT /orgs/:id
User Service:       CRUD on /users, /users/:id/roles
Customer Service:   CRUD on /customers, /customers/:id/addresses
Product Service:    CRUD on /products, /products/:id/variants
Invoice Service:    CRUD on /invoices, POST /invoices/:id/send
Payment Service:    POST /payments, POST /payments/:id/refund
Tax Service:        POST /tax/calculate, GET /tax/rates
Notify Service:     POST /notify/email, /notify/sms, /notify/whatsapp
Report Service:     GET /reports/:type, POST /reports/export
```

Each app-specific module then adds its own routes on top.

### 5.4 Feature Flags by App Context

Use a `app_context` or `product_key` in the tenant settings to determine which modules are enabled:

```json
{
  "tenant_id": "org_123",
  "enabled_products": ["ecommerce"],
  "plan": "business"
}
```

This means a single tenant could later subscribe to both products (e-commerce + tax) and get a **unified platform experience** — a major future upsell opportunity.

---

## 6. Key Benefits of This Approach

### Development Efficiency
- Core modules built once, not twice (~40% reduction in total code)
- Bug fixed in one place benefits both apps
- Security patches applied once
- Onboarding team members is faster — one codebase to learn

### Consistency
- Identical auth experience across both products
- Same customer record can be shared across apps
- Unified notifications and audit trail
- Consistent API conventions and documentation

### Future Opportunities
- **Single sign-on across both products**: One login, access both apps
- **Unified customer data**: A customer in your Tax platform is the same record in your E-Commerce platform
- **Cross-sell**: Tax platform users can activate E-Commerce, and vice versa — no re-onboarding needed
- **Suite pricing**: Offer both products together at a discount
- **Shared data insights**: Customer who buys via E-Commerce auto-populates Tax platform's customer list

### Cost Savings
- One infrastructure to maintain (servers, CDN, monitoring)
- One authentication system to secure
- One compliance audit (GDPR, SOC 2)
- Shared payment gateway contracts

---

## 7. Risks & Mitigations

| Risk | Impact | Mitigation |
|------|--------|-----------|
| Core changes break both apps | High | Strict versioning of core APIs; comprehensive test suites; contract testing |
| Core becomes too complex ("big ball of mud") | Medium | Enforce module boundaries; no cross-module direct DB access; use internal APIs |
| Different scaling needs per app | Medium | Deploy core services independently; app-specific scaling groups |
| Tax-specific requirements creeping into core | Medium | Strict "core vs extension" review process; separate Tax Engine extension clearly |
| Team ownership confusion | Low | Assign clear ownership: "Core Team" vs "E-Commerce Team" vs "Tax Team" |

---

## 8. Recommended Build Sequence

### Foundation (Months 1–4) — Build the Core Engine
1. Multi-tenancy + Auth + RBAC
2. User & Organization management
3. Customer management (core)
4. Product catalog (core)
5. Invoice engine (core)
6. Payment engine (core)
7. Tax calculation engine (core)
8. Notification engine
9. Settings & configuration
10. Onboarding framework
11. Audit logs

### Parallel Track A (Months 3–8) — E-Commerce App on Core
- Build Shop/Storefront, Catalogue, Cart, Checkout, Shipping, Quotes using the core

### Parallel Track B (Months 3–8) — Tax Platform on Core
- Build GST Returns, ITC, HSN/SAC, Customer Portal, Accounting integrations using the core

### Convergence (Month 9+)
- Unified SSO, shared customer database, cross-app reporting, suite packaging

---

## 9. The Golden Rule

> **If the feature would make sense in ANY multi-tenant SaaS product — it belongs in the core.**
>
> **If the feature is specific to selling goods online — it belongs in the E-Commerce app.**
>
> **If the feature is specific to Indian tax compliance — it belongs in the Tax app.**

When in doubt, start by putting new logic in the app layer. Move it to the core only when you find yourself writing the same code for the second app.

---

## 10. Quick Reference Checklist

### ✅ Put in Core Engine
- [ ] Authentication and session management
- [ ] Organization and tenant management
- [ ] User profiles and RBAC
- [ ] Customer CRUD and addresses
- [ ] Product catalog (core fields)
- [ ] Invoice creation and management
- [ ] Payment recording and gateways
- [ ] Tax rate assignment and calculation
- [ ] Email/SMS/WhatsApp notifications
- [ ] Report generation infrastructure
- [ ] PDF generation
- [ ] Subscription/billing management
- [ ] API keys and webhooks
- [ ] Audit logs
- [ ] File/media management
- [ ] Onboarding wizard
- [ ] Settings framework

### ⚡ Extend in App Layer
- [ ] Product variants with inventory (E-Commerce)
- [ ] HSN/SAC codes on products (Tax)
- [ ] Invoice with order/shipping link (E-Commerce)
- [ ] Invoice with GSTR data (Tax)
- [ ] Customer with wishlist/cart (E-Commerce)
- [ ] Customer with GSTIN/ITC eligibility (Tax)

### 🔴 Build Exclusively in App Layer
**E-Commerce:** Shop, Catalogue, Cart, Checkout, Shipping, Fulfillment, Quotes, Themes
**Tax:** GSTR-1/2/3B, ITC, E-way Bill, Composition Scheme, Customer Portal, Accounting sync

---

*This unified engine approach is the most efficient and strategically sound path forward. It positions both products for faster development, easier maintenance, and eventual suite convergence — while keeping the domain-specific logic cleanly separated.*
