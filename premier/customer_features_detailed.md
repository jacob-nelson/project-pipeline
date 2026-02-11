# Customer Features & Repeat Order System
## Detailed Specifications

This document provides detailed specifications for customer-facing features including account management, quote history, and repeat order functionality.

---

## 1. CUSTOMER ACCOUNT SYSTEM

### 1.1 Registration & Login

#### Registration Flow
1. Customer clicks "Create Account" or "Sign Up"
2. Registration form appears:
   - Email address (required, validated)
   - Password (min 8 chars, must include uppercase, lowercase, number)
   - Confirm password
   - Full name (required)
   - Phone number (required)
   - Company name (optional)
   - Accept terms & conditions (checkbox)
3. Submit registration
4. Verification email sent
5. Customer clicks verification link
6. Account activated
7. Auto-login after verification

#### Alternative: Register During Quote Submission
- Guest submits quote with email
- After submission, prompt: "Save this information for faster checkout next time?"
- If yes, show password creation form
- Account created with quote information
- Verification email sent

#### Login Options
- **Email/Password Login**
  - Email field
  - Password field
  - "Remember me" checkbox
  - "Forgot password?" link
  
- **Social Login (Optional)**
  - Continue with Google
  - Continue with Facebook
  - Auto-create account with social profile data

#### Password Reset
1. Click "Forgot Password?"
2. Enter email address
3. Reset link sent via email
4. Click link (valid for 1 hour)
5. Enter new password
6. Password updated
7. Auto-login

### 1.2 Customer Profile Management

#### Profile Information Fields
```
Personal Information:
- Full Name *
- Email Address * (cannot change, contact admin to change)
- Phone Number *
- Company Name
- Job Title/Position
- Tax/GST Number
- Profile Photo (upload, max 2MB)

Account Settings:
- Current Password (to change password)
- New Password
- Confirm New Password

Notification Preferences:
- Email notifications for quote updates ☑
- Email for new products ☑
- Email for promotional offers ☑
- SMS notifications ☑
```

#### Saved Addresses

**Address Structure:**
```
Address Fields:
- Address Label * (e.g., "Main Office", "Warehouse 1")
- Address Type * (Delivery / Billing / Both)
- Contact Person Name
- Street Address 1 *
- Street Address 2
- City *
- State/Province *
- Postal/ZIP Code *
- Country *
- Phone Number
- Delivery Instructions
- Set as Default Address (checkbox)
```

**Address Management:**
- View all saved addresses
- Add new address
- Edit existing address
- Delete address (cannot delete if used in active quote)
- Set default delivery address
- Set default billing address
- Use address from profile or add new during quote

---

## 2. CUSTOMER DASHBOARD

### 2.1 Dashboard Layout

```
┌─────────────────────────────────────────────────────┐
│  Welcome back, [Customer Name]!                     │
├─────────────────────────────────────────────────────┤
│                                                      │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐         │
│  │  Total   │  │ Pending  │  │ Accepted │         │
│  │  Quotes  │  │  Quotes  │  │  Quotes  │         │
│  │    15    │  │     3    │  │     8    │         │
│  └──────────┘  └──────────┘  └──────────┘         │
│                                                      │
├─────────────────────────────────────────────────────┤
│  Quick Reorder (Your Frequent Items)                │
│  ┌────────┐  ┌────────┐  ┌────────┐               │
│  │Product │  │Product │  │Product │               │
│  │  A     │  │  B     │  │  C     │               │
│  │[Reorder]│  │[Reorder]│  │[Reorder]│             │
│  └────────┘  └────────┘  └────────┘               │
├─────────────────────────────────────────────────────┤
│  Recent Quotes                                       │
│  • Quote #1245 - Pending - Jan 15, 2026            │
│  • Quote #1240 - Quoted - Jan 10, 2026 [Reorder]   │
│  • Quote #1235 - Accepted - Jan 5, 2026 [Reorder]  │
└─────────────────────────────────────────────────────┘
```

### 2.2 Dashboard Features

**Quick Stats Cards:**
- Total quotes submitted
- Pending quotes (awaiting admin response)
- Accepted quotes
- Click to view filtered list

**Quick Reorder Section:**
- Shows 3-6 most frequently ordered products
- Based on quote history
- One-click add to cart
- Shows last order date
- Shows suggested quantity (based on average)

**Recent Activity:**
- Last 5 quotes with status
- Quick actions: View, Reorder, Download PDF
- Status color coding

**Saved Cart Reminder:**
- If cart has items, show notification
- "You have X items in your cart - Continue Shopping"

---

## 3. QUOTE HISTORY & MANAGEMENT

### 3.1 Quote History List

#### View Options
- **List View** (default)
  - Quote reference number
  - Submission date
  - Status badge
  - Total items count
  - Total amount (if quoted)
  - Quick action buttons
  
- **Grid View**
  - Card-based layout
  - Thumbnail of first product
  - Key information
  - Larger action buttons

#### Filters & Search
```
Filters:
- Status: All | Pending | Quoted | Accepted | Rejected | Expired
- Date Range: Last 7 days | Last 30 days | Last 6 months | Custom
- Sort By: Newest First | Oldest First | Highest Value | Status

Search:
- By quote reference number
- By product name
- By notes/instructions
```

### 3.2 Quote Detail View

#### Information Displayed

**Quote Header:**
- Quote Reference Number (e.g., QR-2026-001245)
- Submission Date & Time
- Status Badge (with color)
- Last Updated Date

**Customer Information:**
- Name, Email, Phone
- Company Name
- Delivery Address
- Billing Address
- Special Instructions

**Products List:**
```
┌──────────────────────────────────────────────────┐
│ Product Name              Qty  Unit Price  Total │
│ Product A (Blue, Large)    5    $10.00   $50.00 │
│ Product B                  3    $25.00   $75.00 │
│                                                   │
│ Subtotal:                              $125.00   │
│ Tax (10%):                              $12.50   │
│ Shipping:                               $15.00   │
│ ─────────────────────────────────────────────── │
│ Total:                                 $152.50   │
└──────────────────────────────────────────────────┘
```

**Admin Response (if quoted):**
- Admin notes/terms
- Quote validity date
- Attached documents (PDF quotes, specs)
- Payment terms
- Delivery timeline

**Quote Timeline:**
- Submitted: Jan 15, 2026 10:30 AM
- Viewed by admin: Jan 15, 2026 2:15 PM
- Quote sent: Jan 16, 2026 9:00 AM
- Accepted by customer: Jan 16, 2026 3:30 PM

#### Action Buttons

**Available Actions (based on status):**
- **Reorder** - Copy all items to new cart
- **Download PDF** - Get quoted PDF (if available)
- **Accept Quote** - Mark as accepted (if pending)
- **Add Note** - Communicate with admin
- **Print** - Print quote details
- **Share** - Email to colleague

---

## 4. REPEAT ORDER SYSTEM

### 4.1 Quick Reorder from Quote History

#### Reorder Button Functionality

**When customer clicks "Reorder" on a quote:**

1. **Load Quote Items**
   - Fetch all products from the quote
   - Include selected variants
   - Include original quantities

2. **Validate Products**
   - Check if products are still active
   - Check if variants are available
   - Check stock availability
   - Flag any issues

3. **Add to Cart**
   - All available items added to cart
   - Show notification: "10 items added to cart"
   - If some items unavailable, show warning

4. **Cart Review**
   - Navigate to cart
   - Highlight newly added items
   - Show message: "Reordered from Quote #1240"
   - Allow quantity adjustments
   - Remove unwanted items

5. **Proceed to Quote**
   - Pre-fill customer information
   - Use same delivery address (or allow change)
   - Add notes if needed
   - Submit new quote

#### Reorder Validation Rules

```
Product Checks:
✓ Product is Active
✓ Product is in current catalogue
✓ Variant exists and is active
✓ Sufficient stock available

If Product Inactive:
→ Skip and notify customer
→ "Product X is no longer available"

If Variant Changed:
→ Show alternative variants
→ Allow customer to select

If Out of Stock:
→ Add to cart but show warning
→ "Limited stock - please check with us"
```

### 4.2 Frequent Items Section

#### Identification Logic

**How to identify frequent items:**
1. Analyze quote history (last 6 months)
2. Count product appearances across quotes
3. Products ordered 3+ times = frequent
4. Sort by frequency and recency
5. Show top 6 items

**Display:**
```
┌────────────────────────────────────┐
│  Product Image                     │
│  Product Name                      │
│  Last ordered: 15 days ago         │
│  Usual quantity: 10 units          │
│  ┌──────────────┐                  │
│  │ Quick Add    │                  │
│  │   to Cart    │                  │
│  └──────────────┘                  │
└────────────────────────────────────┘
```

**Quick Add Features:**
- One-click add with suggested quantity
- Option to adjust quantity before adding
- Instant cart update
- Toast notification

### 4.3 Reorder Templates (Advanced Feature)

#### Creating a Template

**Template Creation Process:**
1. From quote history, select quote
2. Click "Save as Template"
3. Name the template (e.g., "Monthly Office Supplies")
4. Add description (optional)
5. Edit items if needed:
   - Remove items
   - Adjust default quantities
   - Add notes
6. Save template

**Template Structure:**
```
Template Details:
- Template Name
- Description
- Created Date
- Last Used Date
- Number of Items

Items in Template:
- Product Name
- Variant
- Default Quantity
- Notes
```

#### Using a Template

1. Navigate to "My Templates"
2. Select template
3. Click "Load to Cart"
4. All items added with default quantities
5. Review and adjust in cart
6. Proceed to quote submission

#### Template Management

**Actions:**
- View all templates
- Edit template (add/remove items, change quantities)
- Delete template
- Duplicate template
- Share template with colleague (optional)

---

## 5. FAVORITES / WISHLIST

### 5.1 Adding to Favorites

**From Product Page:**
- Heart icon next to "Add to Cart"
- Click to add to favorites
- Icon fills when favorited
- Toast: "Added to favorites"

**From Catalogue:**
- Heart icon on product card
- Quick add without opening product

### 5.2 Favorites Management

#### My Favorites Page

**Features:**
- Grid view of favorite products
- Search favorites
- Sort by: Date added | Name | Price
- Create multiple lists (optional):
  - Default Favorites
  - Seasonal Items
  - Backup Supplies
  - etc.

**Product Card in Favorites:**
```
┌────────────────────────────┐
│  Product Image             │
│  Product Name              │
│  Price: $25.00             │
│  Added: 10 days ago        │
│  ┌─────┐  ┌────────────┐  │
│  │ ♥   │  │ Add to Cart│  │
│  └─────┘  └────────────┘  │
└────────────────────────────┘
```

**Actions:**
- Add to cart (one or multiple)
- Remove from favorites
- Add note to favorite
- Move to different list
- Share favorites list

### 5.3 Price Drop Alerts (Optional)

- Customer opts in for price alerts
- System tracks price changes
- Email sent when favorite item price drops
- Shows old price vs. new price

---

## 6. SHOPPING CART ENHANCEMENTS

### 6.1 Persistent Cart

#### Guest Users
- Cart saved in browser session
- Lost when browser closed
- Prompt to register to save cart

#### Registered Users
- Cart saved to database
- Syncs across devices
- Available on mobile and desktop
- Auto-save every change

### 6.2 Saved Carts

**Save for Later:**
- Customer can save current cart
- Name the saved cart (e.g., "February Order")
- Load saved cart anytime
- Multiple saved carts allowed

**Use Cases:**
- Planning future orders
- Comparing different order combinations
- Seasonal orders
- Budget planning

### 6.3 Cart Sharing

**Share Cart via Email:**
1. Click "Share Cart"
2. Enter colleague's email
3. Add message
4. Send
5. Recipient gets link to view cart
6. Can load items to their own cart

---

## 7. NOTIFICATIONS SYSTEM

### 7.1 Email Notifications

**Triggered Emails:**

| Event | Subject | When |
|-------|---------|------|
| Welcome | Welcome to [Business Name]! | Registration complete |
| Quote Submitted | Quote Request #XXX Submitted | Quote submitted |
| Quote Received | Quote #XXX Ready for Review | Admin sends quote |
| Quote Accepted | Quote #XXX Accepted | Customer accepts |
| Quote Expiring | Quote #XXX Expires Soon | 3 days before expiry |
| New Products | Check Out Our New Products! | Admin adds new products (weekly digest) |
| Price Drop | Price Drop Alert on Your Favorites | Favorite item price reduced |
| Cart Reminder | You Left Items in Your Cart | 24 hours after cart abandonment |

### 7.2 In-App Notifications

**Notification Bell Icon:**
- Badge count of unread notifications
- Dropdown list of recent notifications
- Mark as read/unread
- Clear all notifications

**Notification Types:**
- Quote status updates
- Admin responses
- System announcements
- Promotional messages (opt-in)

### 7.3 Notification Preferences

**Customer Controls:**
```
Email Preferences:
☑ Quote updates (required)
☑ New products and catalogues
☐ Promotional offers
☐ Price drop alerts
☑ Cart reminders

Frequency:
● Instant notifications
○ Daily digest
○ Weekly digest

Communication Channel:
☑ Email
☑ SMS (optional)
☐ Push notifications (mobile app)
```

---

## 8. USER EXPERIENCE FLOWS

### 8.1 First-Time Customer Journey

1. **Discover Catalogue**
   - Receives link from business
   - Opens catalogue as guest
   - Browses products

2. **Browse & Add to Cart**
   - Searches for products
   - Adds items to cart
   - Views cart

3. **Submit Quote (Guest)**
   - Clicks "Request Quote"
   - Fills in information
   - Submits quote
   - Receives confirmation

4. **Post-Submission Prompt**
   - "Create account for faster checkout?"
   - Creates account
   - Future quotes pre-filled

### 8.2 Returning Customer Journey

1. **Login**
   - Visits catalogue link
   - Logs in with saved credentials

2. **Quick Reorder**
   - Sees dashboard with frequent items
   - Clicks "Reorder" on previous quote
   - Items added to cart

3. **Review & Adjust**
   - Modifies quantities
   - Adds/removes items
   - Reviews total

4. **Submit Quote**
   - Information pre-filled
   - Selects saved address
   - Adds special notes
   - Submits in seconds

5. **Track Status**
   - Views quote in history
   - Gets notification when quoted
   - Reviews quote
   - Accepts quote

---

## 9. TECHNICAL IMPLEMENTATION

### 9.1 Database Schema for Customer Features

**Customers Table:**
```sql
CREATE TABLE customers (
    id UUID PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255),
    full_name VARCHAR(255) NOT NULL,
    phone VARCHAR(50) NOT NULL,
    company_name VARCHAR(255),
    job_title VARCHAR(255),
    tax_number VARCHAR(100),
    profile_photo_url TEXT,
    email_verified BOOLEAN DEFAULT FALSE,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP,
    updated_at TIMESTAMP,
    last_login_at TIMESTAMP
);
```

**Customer_Addresses Table:**
```sql
CREATE TABLE customer_addresses (
    id UUID PRIMARY KEY,
    customer_id UUID REFERENCES customers(id),
    address_label VARCHAR(100) NOT NULL,
    address_type VARCHAR(20), -- 'delivery', 'billing', 'both'
    contact_person VARCHAR(255),
    street_address_1 VARCHAR(255) NOT NULL,
    street_address_2 VARCHAR(255),
    city VARCHAR(100) NOT NULL,
    state VARCHAR(100) NOT NULL,
    postal_code VARCHAR(20) NOT NULL,
    country VARCHAR(100) NOT NULL,
    phone VARCHAR(50),
    delivery_instructions TEXT,
    is_default_delivery BOOLEAN DEFAULT FALSE,
    is_default_billing BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP,
    updated_at TIMESTAMP
);
```

**Customer_Favorites Table:**
```sql
CREATE TABLE customer_favorites (
    id UUID PRIMARY KEY,
    customer_id UUID REFERENCES customers(id),
    product_id UUID REFERENCES products(id),
    notes TEXT,
    created_at TIMESTAMP
);
```

**Saved_Carts Table:**
```sql
CREATE TABLE saved_carts (
    id UUID PRIMARY KEY,
    customer_id UUID REFERENCES customers(id),
    cart_name VARCHAR(255),
    cart_data JSON, -- stores products, quantities, variants
    created_at TIMESTAMP,
    updated_at TIMESTAMP
);
```

**Reorder_Templates Table:**
```sql
CREATE TABLE reorder_templates (
    id UUID PRIMARY KEY,
    customer_id UUID REFERENCES customers(id),
    template_name VARCHAR(255) NOT NULL,
    description TEXT,
    template_data JSON, -- stores products, quantities, notes
    last_used_at TIMESTAMP,
    created_at TIMESTAMP,
    updated_at TIMESTAMP
);
```

**Notifications Table:**
```sql
CREATE TABLE notifications (
    id UUID PRIMARY KEY,
    customer_id UUID REFERENCES customers(id),
    type VARCHAR(50), -- 'quote_update', 'new_product', etc.
    title VARCHAR(255),
    message TEXT,
    link VARCHAR(255),
    is_read BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP
);
```

### 9.2 API Endpoints

**Authentication:**
- POST /api/customer/register
- POST /api/customer/login
- POST /api/customer/logout
- POST /api/customer/forgot-password
- POST /api/customer/reset-password
- POST /api/customer/verify-email

**Profile:**
- GET /api/customer/profile
- PUT /api/customer/profile
- PUT /api/customer/change-password

**Addresses:**
- GET /api/customer/addresses
- POST /api/customer/addresses
- PUT /api/customer/addresses/:id
- DELETE /api/customer/addresses/:id

**Quote History:**
- GET /api/customer/quotes
- GET /api/customer/quotes/:id
- POST /api/customer/quotes/:id/reorder
- POST /api/customer/quotes/:id/accept
- POST /api/customer/quotes/:id/notes

**Favorites:**
- GET /api/customer/favorites
- POST /api/customer/favorites
- DELETE /api/customer/favorites/:id

**Cart:**
- GET /api/customer/cart
- POST /api/customer/cart/add
- PUT /api/customer/cart/update
- DELETE /api/customer/cart/remove
- POST /api/customer/cart/save
- GET /api/customer/saved-carts
- POST /api/customer/cart/load/:savedCartId

**Templates:**
- GET /api/customer/templates
- POST /api/customer/templates
- PUT /api/customer/templates/:id
- DELETE /api/customer/templates/:id
- POST /api/customer/templates/:id/load

---

## 10. SECURITY CONSIDERATIONS

### 10.1 Customer Data Protection
- Encrypt passwords with bcrypt
- Secure session management
- HTTPS only
- CSRF tokens on forms
- Rate limiting on login attempts
- Email verification required

### 10.2 Privacy
- Clear privacy policy
- GDPR compliance
- Right to data deletion
- Right to data export
- Cookie consent
- Marketing opt-in (not opt-out)

---

## CONCLUSION

This comprehensive customer feature set transforms the catalogue from a simple browsing tool into a full customer portal with account management, order history, and intelligent reordering capabilities. The repeat order system significantly reduces friction for returning customers, making it easy to reorder frequently purchased items with just a few clicks.

