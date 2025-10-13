# 🗺️ Routes Architecture Documentation

**Renturo Multi-Tenant Platform**

This document explains the **routing structure and organization** of the Renturo platform. For specific API endpoints and their parameters, refer to the **Swagger API Documentation** at `http://main.renturo.test/api/documentation`.

---

## 📂 Directory Structure

```
routes/
├── 📄 Root Files (Laravel + Custom)
│   ├── api.php              # Central API routes (unused)
│   ├── auth.php             # Tenant authentication (Laravel Breeze)
│   ├── channels.php         # WebSocket/Broadcasting
│   ├── console.php          # Artisan CLI commands
│   ├── tenant.php           # Tenant web routes
│   └── web.php              # Central web routes (Super Admin)
│
├── 📁 apis/                 # Mobile API Routes (OAuth2/Passport)
│   ├── tenant.php           # Shared authentication API
│   └── tenants/
│       ├── client.php       # Client App API (Property Owners)
│       └── user.php         # User App API (Renters)
│
└── 📁 tenants/              # Web Dashboard Routes (Session)
    ├── admin.php            # Admin web dashboard
    ├── client.php           # Client web dashboard (Property Owners)
    └── user.php             # User web dashboard (Renters)
```

---

## 🎯 Routing Philosophy

### **Multi-Tenant Architecture**

Renturo uses the **Stancl Tenancy** package, which provides:
- **Central Domain:** `renturo.test` - Super Admin management
- **Tenant Domains:** `*.renturo.test` - Tenant-specific applications (e.g., `main.renturo.test`)

### **Separation of Concerns**

Routes are organized by:
1. **Platform Type:** Web vs Mobile (API)
2. **User Type:** Admin, Client (Owner), User (Renter)
3. **Authentication Method:** Session vs OAuth2/Passport
4. **Purpose:** Authentication, Features, Management

---

## 📄 Root Route Files

### **1. `web.php`** - Central Web Routes
**Domain:** `renturo.test` (Central)  
**Purpose:** Super Admin dashboard and tenant management  
**Authentication:** Session (`auth:central`)  
**Middleware:** `web`  
**Returns:** Inertia.js pages (React)

**Responsibilities:**
- Super Admin authentication
- Super Admin dashboard
- Tenant management (CRUD)
- Central user management
- Roles & permissions management
- Central settings

**Access:** `http://renturo.test/`

---

### **2. `tenant.php`** - Tenant Web Routes
**Domain:** `*.renturo.test` (Tenant subdomains)  
**Purpose:** Base tenant web routes and authentication  
**Authentication:** Session (`auth`)  
**Middleware:** `web`, `InitializeTenancyByDomain`, `PreventAccessFromCentralDomains`  
**Returns:** Inertia.js pages (React)

**Responsibilities:**
- Tenant authentication (includes `auth.php`)
- Mobile OTP verification (web interface)
- Tenant-specific base pages
- Includes subdirectory routes from `tenants/`

**Access:** `http://main.renturo.test/`

**Note:** This file acts as a base and includes routes from the `tenants/` subdirectory for role-specific dashboards.

---

### **3. `auth.php`** - Authentication Routes
**Domain:** `*.renturo.test` (Tenant domains)  
**Purpose:** Laravel Breeze authentication scaffolding  
**Authentication:** Mixed (`guest` and `auth`)  
**Middleware:** `web`  
**Package:** Laravel Breeze

**Responsibilities:**
- User registration
- User login/logout
- Password reset flow
- Email verification
- Password confirmation

**Access:** Included in `tenant.php`

**Note:** Standard Laravel authentication routes for tenant web users.

---

### **4. `api.php`** - Central API Routes
**Domain:** `renturo.test` (Central)  
**Purpose:** Central domain API (currently unused)  
**Authentication:** N/A (placeholder)  
**Middleware:** `api`  
**Status:** ⚠️ **Not actively used**

**Note:** Reserved for future central API endpoints if needed. Currently contains only placeholder Sanctum route (removed).

---

### **5. `channels.php`** - Broadcasting Channels
**Purpose:** WebSocket/Broadcasting channel authorization  
**Package:** Laravel Broadcasting  
**Middleware:** Broadcasting authentication

**Responsibilities:**
- Define broadcast channel authorization
- User notification channels
- Chat channel permissions
- Real-time event authorization

**Channels Defined:**
- `App.Models.User.{id}` - Private user channel
- `chat.{chatId}` - Chat room channel with participant validation

---

### **6. `console.php`** - Artisan Commands
**Purpose:** Custom Artisan CLI commands  
**Environment:** Command-line only  
**Package:** Laravel Console

**Responsibilities:**
- Define custom Artisan commands
- Schedule task definitions
- CLI utilities

---

## 📁 APIs Directory - Mobile App Routes

### **Purpose**
All routes in the `apis/` directory serve **Flutter mobile applications** using **OAuth2/Passport** authentication. They return **JSON responses** for consumption by mobile clients.

---

### **`apis/tenant.php`** - Shared Authentication API

**URL Prefix:** `/api/v1/`  
**Purpose:** Common authentication for ALL mobile apps  
**Authentication:** OAuth2/Passport (Bearer tokens)  
**Used By:** Client App ✅ | User App ✅  
**Middleware:** `api`, `InitializeTenancyByDomain`, `PreventAccessFromCentralDomains`  
**Returns:** JSON

**Responsibilities:**
- User login (email, phone, username)
- User registration
- OTP verification (mobile number)
- OTP resend
- Password management
- User logout
- User profile retrieval
- MPIN management

**Access:** `https://renturo.ngrok.app/api/v1/`

**Key Principle:** This file contains ONLY authentication-related endpoints shared by both Client and User apps. All feature-specific endpoints are in their respective app files.

---

### **`apis/tenants/client.php`** - Client App API

**URL Prefix:** `/api/client/v1/`  
**Purpose:** Features for Property Owners (Client App)  
**Authentication:** OAuth2/Passport + Mobile verified  
**Used By:** Client App ✅ (Property Owners only)  
**Middleware:** `api`, `auth:api`, `verifiedMobileNumber`, `InitializeTenancyByDomain`  
**Returns:** JSON

**Responsibilities:**
- Property listing management (stores)
- Dynamic form submissions
- Bank account management
- Property categories
- Real-time chat/messaging
- Form availability management
- File uploads

**Access:** `https://renturo.ngrok.app/api/client/v1/`

**User Role:** `OWNER` (Property Owners who create/manage listings)

**Feature Categories:**
- 🏪 **Stores:** Create and manage property listings
- 📋 **Forms:** Submit property information forms
- 🏦 **Banks:** Manage payment accounts
- 🏷️ **Categories:** Browse property categories
- 💬 **Chat:** Communicate with potential renters
- 💌 **Messages:** Send/receive messages
- 📅 **Availability:** Manage property availability

---

### **`apis/tenants/user.php`** - User App API

**URL Prefix:** `/api/user/v1/`  
**Purpose:** Features for Renters (User App)  
**Authentication:** OAuth2/Passport + Mobile verified  
**Used By:** User App ✅ (Renters only)  
**Middleware:** `api`, `auth:api`, `verifiedMobileNumber`, `InitializeTenancyByDomain`  
**Returns:** JSON

**Responsibilities:**
- Browse available listings (forms)
- View listing details
- ⚠️ **Create bookings** (to be implemented)
- ⚠️ **Manage bookings** (to be implemented)
- ⚠️ **Reviews & ratings** (to be implemented)
- ⚠️ **Favorites** (to be implemented)

**Access:** `https://renturo.ngrok.app/api/user/v1/`

**User Role:** `USER` (Renters who browse and book properties)

**Status:** ⚠️ **Minimal Implementation** - Only form browsing is currently implemented. Booking and management features need to be added.

---

## 📁 Tenants Directory - Web Dashboard Routes

### **Purpose**
All routes in the `tenants/` directory serve **web dashboards** accessed via **browser** using **session authentication**. They return **Inertia.js pages** (React components) for web interfaces.

---

### **`tenants/admin.php`** - Admin Dashboard

**URL Prefix:** `/admin/`  
**Purpose:** Tenant admin dashboard for managing listings, users, and content  
**Authentication:** Session (`auth`)  
**Used By:** Admins via web browser  
**Middleware:** `web`, `auth`, `verifiedMobileNumber`, `InitializeTenancyByDomain`  
**Returns:** Inertia.js pages (React)

**Responsibilities:**
- Post management (properties, ads, promotions, bookings)
- Category management (categories, sub-categories)
- Dynamic form builder
- User management (admins, owners, sub-owners, users)
- Reports and analytics
- Settings and configuration

**Access:** `http://main.renturo.test/admin/`

**User Role:** `ADMIN` (Tenant administrators who manage the platform)

**Feature Categories:**
- 📝 **Posts:** Manage listings, advertisements, promotions
- 🏷️ **Categories:** Create and organize property categories
- 📋 **Forms:** Build dynamic property submission forms
- 👥 **Users:** Manage all user types
- 📊 **Reports:** View user reports and analytics
- ⚙️ **Settings:** Configure personal settings and passwords

---

### **`tenants/client.php`** - Client Dashboard

**URL Prefix:** `/client/`  
**Purpose:** Web dashboard for Property Owners  
**Authentication:** Session (`auth`)  
**Used By:** Property Owners via web browser  
**Middleware:** `web`, `auth`, `verifiedMobileNumber`, `InitializeTenancyByDomain`  
**Returns:** Inertia.js pages (React)

**Responsibilities:**
- View dashboard overview
- Manage property listings (post management)
- View analytics (ads, listings, promotions)
- Manage calendar/availability
- User management
- Account settings

**Access:** `http://main.renturo.test/client/`

**User Role:** `OWNER` (Property Owners who manage their listings via web)

**Note:** This is the **web version** of what the Client App does on mobile. Property Owners can choose to use either the mobile app or web dashboard.

**Feature Categories:**
- 📊 **Dashboard:** Overview of properties and performance
- 🏪 **Post Management:** List and manage properties
- 📈 **Analytics:** View ads, listings, and promotion analytics
- 📅 **Calendar:** Manage property availability
- 👥 **User Management:** Manage sub-users if applicable
- ⚙️ **Settings:** Account configuration

---

### **`tenants/user.php`** - User Dashboard

**URL Prefix:** `/user/`  
**Purpose:** Web dashboard for Renters  
**Authentication:** Session (`auth`)  
**Used By:** Renters via web browser  
**Middleware:** `web`, `auth`, `verifiedMobileNumber`, `InitializeTenancyByDomain`  
**Returns:** Inertia.js pages (React)

**Status:** ⚠️ **Empty** - No routes defined yet

**Planned Responsibilities:**
- Browse available properties
- View booking history
- Manage favorites
- View reviews
- Account settings

**Access:** `http://main.renturo.test/user/`

**User Role:** `USER` (Renters who use the platform via web)

**Note:** Currently empty. This will be the web version of the User App. Most renters will likely use the mobile app, but a web interface should be provided for accessibility.

---

## 🔐 Authentication Methods

### **Session-Based (Web)**

**Used By:** `web.php`, `tenant.php`, `tenants/*.php`  
**Authentication:** Laravel's built-in session authentication  
**Guard:** `auth` or `auth:central`  
**Storage:** Server-side sessions with cookies  
**Login Flow:**
1. User submits credentials
2. Server creates session
3. Session ID stored in cookie
4. Subsequent requests include session cookie

**Best For:** Traditional web applications with server-side rendering (Inertia.js)

---

### **OAuth2/Passport (Mobile)**

**Used By:** `apis/tenant.php`, `apis/tenants/*.php`  
**Authentication:** Laravel Passport (OAuth2 server)  
**Guard:** `auth:api`  
**Storage:** Client-side bearer tokens  
**Login Flow:**
1. User submits credentials to `/api/v1/login`
2. Server returns `access_token` (JWT)
3. Client stores token in `FlutterSecureStorage`
4. Subsequent requests include `Authorization: Bearer {token}` header

**Best For:** Mobile applications, SPAs, API consumption

**Token Features:**
- Refresh tokens for extended sessions
- Scopes for permission management
- Multi-tenant client support
- Server-side revocation

---

## 🔄 Request Flow Examples

### **Web Authentication Flow (Admin Login)**

```
1. Browser → GET http://main.renturo.test/login
   ├─ Route: tenant.php (includes auth.php)
   └─ Returns: Inertia.js login page

2. Browser → POST http://main.renturo.test/login
   ├─ Route: tenant.php (AuthenticatedSessionController)
   ├─ Validates credentials
   ├─ Creates session
   ├─ Sends OTP via email
   └─ Redirects to: /admin/ (based on role)

3. Browser → GET http://main.renturo.test/admin/
   ├─ Route: tenants/admin.php
   ├─ Middleware: auth, verifiedMobileNumber
   └─ Returns: Admin dashboard page
```

---

### **Mobile API Flow (Client App - Create Listing)**

```
1. Client App → POST https://renturo.ngrok.app/api/v1/login
   ├─ Route: apis/tenant.php
   ├─ Body: { email, password }
   ├─ Validates credentials
   ├─ Creates Passport token
   └─ Returns: { access_token, user }

2. Client App → PUT https://renturo.ngrok.app/api/v1/verify/mobile
   ├─ Route: apis/tenant.php
   ├─ Headers: { Authorization: Bearer {token} }
   ├─ Body: { code }
   ├─ Verifies OTP
   └─ Returns: { message: success }

3. Client App → POST https://renturo.ngrok.app/api/client/v1/store
   ├─ Route: apis/tenants/client.php
   ├─ Headers: { Authorization: Bearer {token} }
   ├─ Middleware: auth:api, verifiedMobileNumber
   ├─ Body: { name, description, category_id, ... }
   └─ Returns: { message: success, store: {...} }
```

---

## 🎨 Middleware Overview

### **Multi-Tenancy Middleware**

| Middleware | Purpose | Applied To |
|------------|---------|------------|
| `InitializeTenancyByDomain` | Identify and initialize tenant based on domain | All tenant routes |
| `PreventAccessFromCentralDomains` | Block central domain from accessing tenant routes | All tenant routes |
| `RedirectIfTenantActivated` | Redirect if tenant is not active | All tenant routes |

### **Authentication Middleware**

| Middleware | Purpose | Applied To |
|------------|---------|------------|
| `auth` | Require session authentication | Web routes |
| `auth:api` | Require Passport token authentication | API routes |
| `auth:central` | Require central session authentication | Central web routes |
| `guest` | Only allow unauthenticated users | Login, register pages |

### **Verification Middleware**

| Middleware | Purpose | Applied To |
|------------|---------|------------|
| `verifiedMobileNumber` | Require mobile OTP verification | Protected routes |
| `signed` | Require signed URL | Email verification |
| `throttle` | Rate limiting | API, verification routes |

---

## 📊 Route Organization Matrix

| User Type | Platform | Auth | Routes File | URL Prefix | Purpose |
|-----------|----------|------|-------------|------------|---------|
| **Super Admin** | Web | Session | `web.php` | `/super-admin/` | Manage tenants |
| **Admin** | Web | Session | `tenants/admin.php` | `/admin/` | Manage tenant content |
| **Owner (Client)** | Web | Session | `tenants/client.php` | `/client/` | Manage properties (web) |
| **Owner (Client)** | Mobile | Passport | `apis/tenants/client.php` | `/api/client/v1/` | Manage properties (mobile) |
| **User (Renter)** | Web | Session | `tenants/user.php` | `/user/` | Browse/book (web) |
| **User (Renter)** | Mobile | Passport | `apis/tenants/user.php` | `/api/user/v1/` | Browse/book (mobile) |
| **All Mobile** | Mobile | Passport | `apis/tenant.php` | `/api/v1/` | Authentication |

---

## 🔧 Adding New Routes

### **For Mobile API Endpoints**

**If endpoint is for authentication (shared by all apps):**
```php
// File: routes/apis/tenant.php
Route::post('/your-endpoint', [YourController::class, 'method']);
```

**If endpoint is for Client App only:**
```php
// File: routes/apis/tenants/client.php
Route::middleware(['auth:api', 'verifiedMobileNumber'])->group(function () {
    Route::post('/your-endpoint', [YourController::class, 'method']);
});
```

**If endpoint is for User App only:**
```php
// File: routes/apis/tenants/user.php
Route::middleware(['auth:api', 'verifiedMobileNumber'])->group(function () {
    Route::post('/your-endpoint', [YourController::class, 'method']);
});
```

---

### **For Web Dashboard Pages**

**If page is for Admin:**
```php
// File: routes/tenants/admin.php
Route::get('/your-page', function () {
    return Inertia::render('tenants/admin/your-page/index');
});
```

**If page is for Client (Owner):**
```php
// File: routes/tenants/client.php
Route::get('/your-page', function () {
    return Inertia::render('tenants/client/your-page/index');
});
```

**If page is for User (Renter):**
```php
// File: routes/tenants/user.php
Route::get('/your-page', function () {
    return Inertia::render('tenants/user/your-page/index');
});
```

---

## 🎯 Best Practices

### **1. Separation of Concerns**
- ✅ Keep authentication routes separate from feature routes
- ✅ Separate mobile API routes from web dashboard routes
- ✅ Group routes by user type (Admin, Client, User)

### **2. Consistent Naming**
- ✅ Use `/client/` for Property Owners (consistent with Client App)
- ✅ Use `/user/` for Renters
- ✅ Use `/admin/` for Tenant Administrators
- ✅ Use `/super-admin/` for Platform Administrators

### **3. Middleware Application**
- ✅ Always apply multi-tenancy middleware to tenant routes
- ✅ Require mobile verification for protected features
- ✅ Use appropriate authentication guard (`auth` vs `auth:api`)

### **4. URL Structure**
```
# Mobile API (Passport)
/api/v1/*              → Shared authentication
/api/client/v1/*       → Client App features
/api/user/v1/*         → User App features

# Web Dashboard (Session)
/super-admin/*         → Central admin
/admin/*               → Tenant admin
/client/*              → Property owner dashboard
/user/*                → Renter dashboard
```

### **5. Documentation**
- ✅ Document specific endpoints in **Swagger** (`/api/documentation`)
- ✅ Document route architecture in this file
- ✅ Keep route files clean and well-commented
- ✅ Use descriptive route names for easy reference

---

## 📚 Related Documentation

- **API Endpoints:** See Swagger at `http://main.renturo.test/api/documentation`
- **Development Setup:** See `DEVELOPMENT_SETUP.md`
- **Quick Start:** See `QUICK_START.md`
- **Authentication:** See `DEVELOPMENT_SETUP.md` (Step 12: Understanding Authentication System)

---

## 🔄 Route File Loading

### **How Routes Are Loaded**

**1. Laravel's RouteServiceProvider**
```php
// app/Providers/RouteServiceProvider.php
public function boot()
{
    $this->routes(function () {
        // Loads api.php
        Route::middleware('api')
            ->prefix('api')
            ->group(base_path('routes/api.php'));

        // Loads web.php (for central domains)
        $this->mapWebRoutes();
    });
}
```

**2. Tenancy Package**
- Automatically loads `tenant.php` for tenant domains
- Routes in `tenant.php` include subdirectory files
- Multi-tenancy middleware applied automatically

**3. Custom API Routes**
- API routes in `apis/` directory are manually included
- Loaded via custom configuration in tenancy package

---

## ⚠️ Important Notes

### **Domain-Based Routing**

1. **Central Domain** (`renturo.test`)
   - Loads: `web.php`, `api.php`
   - Purpose: Super Admin management
   - Users: Super Admins only

2. **Tenant Domains** (`*.renturo.test`)
   - Loads: `tenant.php` (which includes `auth.php` and `tenants/*.php`)
   - Purpose: Tenant-specific applications
   - Users: Admins, Clients (Owners), Users (Renters)

3. **API Access**
   - Available on all domains with `/api/` prefix
   - Mobile apps use ngrok tunnel for local development
   - Production uses dedicated API subdomain

### **Role-Based Access**

Routes don't enforce role-based access by default. Controllers must check user roles:

```php
if (Auth::user()->role !== User::ROLE_ADMIN) {
    abort(403, 'Unauthorized');
}
```

Consider creating role middleware for route-level protection.

---

## 🚀 Future Improvements

### **Planned Route Additions**

1. **User App API** (`apis/tenants/user.php`)
   - Browse/search listings endpoints
   - Booking creation and management
   - Reviews and ratings
   - Favorites management

2. **User Web Dashboard** (`tenants/user.php`)
   - Browse properties interface
   - Booking history
   - Account management

3. **Partner Routes** (if needed)
   - Ads partner API
   - Ads partner dashboard (currently exists but minimal)

### **Architecture Improvements**

1. **API Versioning**
   - Current: `/api/v1/`
   - Future: Support `/api/v2/` when breaking changes occur

2. **Role Middleware**
   - Create dedicated middleware for each role
   - Simplify route protection

3. **Route Caching**
   - Optimize route loading in production
   - `php artisan route:cache`

---

**Last Updated:** October 12, 2025  
**Version:** 1.0.0  
**Maintained By:** Renturo Development Team

