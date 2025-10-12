# Renturo - Complete Development Setup Guide

**Last Updated:** October 2025  
**Version:** 1.0

This guide provides step-by-step instructions for setting up and running the complete Renturo application suite locally.

---

## 📋 Table of Contents

1. [System Requirements](#system-requirements)
2. [Prerequisites Installation](#prerequisites-installation)
3. [Project Structure](#project-structure)
4. [Backend Setup (Laravel)](#backend-setup-laravel)
5. [Frontend Setup (React + Inertia.js)](#frontend-setup-react--inertiajs)
6. [Client App Setup (Flutter - Owner App)](#client-app-setup-flutter---owner-app)
7. [User App Setup (Flutter - Renter App)](#user-app-setup-flutter---renter-app)
8. [Running All Applications](#running-all-applications)
9. [Testing the Complete Flow](#testing-the-complete-flow)
10. [Troubleshooting](#troubleshooting)
11. [Common Issues](#common-issues)

---

## 🖥️ System Requirements

### Minimum Requirements
- **OS:** macOS 11.0+, Windows 10+, or Linux
- **RAM:** 8GB minimum, 16GB recommended
- **Storage:** 10GB free space
- **Internet:** Stable connection for package downloads

### Required Software Versions

**Backend & Frontend:**
- **PHP:** 8.3 or higher
- **Composer:** 2.x
- **Node.js:** 16.x or higher (18.x recommended)
- **npm:** 8.x or higher
- **Laravel:** 9.19
- **React:** 18.2
- **Vite:** 4.0

**Mobile Apps:**
- **Flutter:** 3.7.0 or higher
- **Dart SDK:** 3.7.0 or higher
- **Android Studio:** Latest stable version
- **Xcode:** 14.0+ (macOS only, for iOS)

---

## 📦 Prerequisites Installation

### 1. XAMPP (Backend Server)

**macOS:**
```bash
# Download from: https://www.apachefriends.org/download.html
# Install XAMPP to: /Applications/XAMPP

# Verify installation
/Applications/XAMPP/xamppfiles/bin/php -v
```

**Components needed:**
- ✅ Apache
- ✅ MySQL
- ✅ PHP 8.1+

### 2. Composer (PHP Package Manager)

```bash
# macOS/Linux
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer

# Verify installation
composer --version
```

### 3. Node.js & nvm (Node Version Manager)

**⚠️ IMPORTANT:** This project requires **Node.js 18.x** for Vite to work properly.

#### Install nvm

```bash
# macOS/Linux
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# Or using Homebrew
brew install nvm

# Add to your shell profile (~/.zshrc or ~/.bash_profile)
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"

# Reload shell
source ~/.zshrc  # or source ~/.bash_profile

# Verify installation
nvm --version
```

#### Install Node 18

```bash
# Install Node 18 LTS (Hydrogen)
nvm install 18.20.8

# Use Node 18 for this project
nvm use 18.20.8

# Verify versions
node --version  # Should show v18.20.8
npm --version   # Should show 10.x

# (Optional) Set Node 18 as default for all projects
nvm alias default 18.20.8
```

#### Project-Specific Node Version

```bash
# Always use Node 18 when working on the main/ directory
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/main
nvm use 18.20.8

# Create .nvmrc file to auto-switch versions
echo "18.20.8" > .nvmrc

# Now when you cd into main/, nvm will auto-switch (if configured)
```

**Why Node 18?**
- ✅ Vite 4.0 requires Node 16+ (ESM support)
- ✅ Node 12 is too old (causes `ERR_REQUIRE_ESM` error)
- ✅ Node 18 is LTS (Long Term Support)
- ✅ Better performance and compatibility

### 4. Flutter SDK

```bash
# macOS (using Homebrew)
brew install flutter

# Or download from: https://flutter.dev/docs/get-started/install

# Verify installation
flutter doctor

# Expected output should show:
# ✓ Flutter (Channel stable, 3.7.0+)
# ✓ Android toolchain
# ✓ Xcode (macOS only)
# ✓ Android Studio
```

### 5. ngrok (API Tunneling)

```bash
# macOS (using Homebrew)
brew install ngrok

# Or download from: https://ngrok.com/download

# Sign up for free account: https://dashboard.ngrok.com/signup
# Get your auth token from: https://dashboard.ngrok.com/get-started/your-authtoken

# Configure ngrok
ngrok config add-authtoken YOUR_AUTH_TOKEN

# Verify installation
ngrok version
```

### 6. Android Emulator Setup

1. Open **Android Studio**
2. Go to **Tools → Device Manager**
3. Create a new virtual device:
   - **Device:** Pixel 5
   - **System Image:** Android 13 (API 33) or higher
   - **Name:** RenturoTestDevice

**Start the emulator:**
```bash
# List available emulators
flutter emulators

# Launch emulator
flutter emulators --launch <emulator_id>

# Or start from Android Studio
```

---

## 📁 Project Structure

```
renturo/
├── main/                    # Full-Stack Web Application
│   ├── app/                 # Laravel backend (PHP)
│   ├── database/
│   ├── routes/
│   ├── resources/
│   │   ├── js/              # React + Inertia.js frontend
│   │   │   ├── app.tsx      # React entry point
│   │   │   ├── components/  # React components
│   │   │   ├── pages/       # Inertia pages
│   │   │   ├── layouts/
│   │   │   └── types/       # TypeScript definitions
│   │   ├── css/             # Tailwind CSS
│   │   └── views/           # Blade templates
│   ├── .env
│   ├── composer.json        # PHP dependencies
│   ├── package.json         # Node.js dependencies
│   └── vite.config.js       # Vite configuration
│
├── client/                  # Flutter App (Property Owners)
│   ├── lib/
│   │   ├── blocs/           # State management
│   │   ├── screens/         # UI screens
│   │   ├── services/        # API services
│   │   └── widgets/         # Reusable widgets
│   ├── android/
│   ├── ios/
│   └── pubspec.yaml
│
├── user/                    # Flutter App (Renters)
│   ├── lib/
│   │   ├── blocs/           # State management
│   │   ├── screens/         # UI screens
│   │   ├── services/        # API services
│   │   └── widgets/         # Reusable widgets
│   ├── android/
│   ├── ios/
│   └── pubspec.yaml
│
├── DEVELOPMENT_SETUP.md     # This file
├── QUICK_START.md
└── DEV_CHECKLIST.md
```

---

## 🔧 Backend Setup (Laravel)

### Step 1: Navigate to Backend Directory

```bash
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/main
```

### Step 2: Install PHP Dependencies

```bash
composer install

# If you encounter memory issues:
php -d memory_limit=-1 /usr/local/bin/composer install
```

### Step 3: Environment Configuration

```bash
# Copy environment file
cp .env.example .env

# Edit .env file with your settings
nano .env
```

**Required .env Configuration:**
```env
APP_NAME=Renturo
APP_ENV=local
APP_KEY=                    # Will generate in next step
APP_DEBUG=true
APP_URL=http://localhost

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=renturo
DB_USERNAME=root
DB_PASSWORD=                # Leave empty for XAMPP default

BROADCAST_DRIVER=log
CACHE_DRIVER=file
FILESYSTEM_DRIVER=local
QUEUE_CONNECTION=sync
SESSION_DRIVER=file
SESSION_LIFETIME=120
```

### Step 4: Generate Application Key

```bash
php artisan key:generate
```

### Step 5: Database Setup

1. **Start XAMPP:**
   - Open XAMPP Control Panel
   - Click **Start** for Apache
   - Click **Start** for MySQL

2. **Create Database:**
   ```bash
   # Access phpMyAdmin: http://localhost/phpmyadmin
   # Create new database named: renturo
   # Collation: utf8mb4_unicode_ci
   ```

   **Or via command line:**
   ```bash
   /Applications/XAMPP/xamppfiles/bin/mysql -u root -e "CREATE DATABASE renturo CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"
   ```

### Step 6: Run Migrations

```bash
php artisan migrate

# If you need to reset:
php artisan migrate:fresh

# With seeders (recommended for development):
php artisan migrate:fresh --seed
```

### Step 7: Seed Test Users & Data

The database seeders create test accounts for all user roles:

```bash
# Run migrations and seeders together
php artisan migrate:fresh --seed
```

**Test User Accounts Created:**

| Role | Email | Username | Password | Platform | Purpose |
|------|-------|----------|----------|----------|---------|
| **Super Admin** | super-admin@renturo.test | (auto) | `password` | Web (central) | Manages all tenants |
| **Admin** | admin@main.renturo.test | beastadmin1234 | `password` | Web (tenant) | Creates & manages listings |
| **Owner** | owner@main.renturo.test | beastowner1234 | `password` | Client App | Saves property listings |
| **User** | user@main.renturo.test | beastuser1234 | `password` | User App | Rents properties |
| **Partner** | ads-partner@main.renturo.test | beastpartner1234 | `password` | Web (tenant) | Manages ads |

**What Gets Seeded:**

1. **Central Database:**
   - Super Admin user
   - Laravel Passport personal access client
   - Test tenant company

2. **Tenant Database:**
   - Admin, Owner, User, and Partner accounts
   - Mobile verification records (pre-verified)
   - Tenant-specific Passport client
   - Form systems, categories, and dynamic forms
   - Basketball arena form templates

**Testing Login:**

```bash
# Web (Central) - http://renturo.test/login
Super Admin: super-admin@renturo.test / password
Purpose: Manage all tenants

# Web (Tenant) - http://main.renturo.test/login
Admin: admin@main.renturo.test / password
Purpose: Create and manage listing properties

# Client App (Mobile Flutter) - owner@main.renturo.test
Owner: owner@main.renturo.test / password
Purpose: Save property listings

# User App (Mobile Flutter) - user@main.renturo.test
User: user@main.renturo.test / password
Purpose: Browse and rent properties
```

**Seeder Files:**
- `database/seeders/DatabaseSeeder.php` - Main seeder
- `database/seeders/TenantSeeder.php` - Creates tenant and users
- `database/seeders/TenantCategorySeeder.php` - Property categories
- `database/seeders/TenantFormSystemSeeder.php` - Form systems
- `database/seeders/TenantDynamicFormSeeder.php` - Dynamic forms

### Step 8: Generate Storage Link

```bash
php artisan storage:link
```

### Step 9: Set Permissions (macOS/Linux)

```bash
chmod -R 775 storage
chmod -R 775 bootstrap/cache
```

### Step 10: Verify Backend is Running

**Option A: Using Valet**
```bash
# Test Valet site
curl http://renturo.test

# Or visit in browser:
# http://renturo.test
```

**Option B: Using XAMPP**
```bash
# Test XAMPP site
curl http://localhost/renturo/main/public

# Or visit in browser:
# http://localhost/renturo/main/public
```

### Step 11: Understanding Multi-Tenant Architecture

**⚠️ IMPORTANT:** Renturo uses a **multi-tenant architecture** with two types of domains:

#### Central Domain (Super Admin)
```bash
# Central domain for system administration
URL: http://renturo.test
Login: http://renturo.test/login

# Test Account
Email: super-admin@renturo.test
Password: password
Role: Super Admin (manages all tenants)
```

#### Tenant Domain (Tenant-Specific Users)
```bash
# Tenant-specific domain (created during seeding)
URL: http://main.renturo.test
Login: http://main.renturo.test/login

# Test Accounts
Admin:   admin@main.renturo.test   / password (Web - Creates listings)
Owner:   owner@main.renturo.test   / password (Client App - Saves listings)
User:    user@main.renturo.test    / password (User App - Rents properties)
Partner: ads-partner@main.renturo.test / password (Ads)
```

**User Role Breakdown:**
- **Admin (Web)**: Accesses `main.renturo.test` to create and manage listing properties
- **Owner (Mobile)**: Uses Client App to save property listings
- **User (Mobile)**: Uses User App to browse and rent properties
- **Partner**: Manages advertisements

**How It Works:**
1. **Central Database** (`renturo`)
   - Stores: Tenants, Super Admins, Global Settings
   - Domain: `renturo.test`

2. **Tenant Database** (e.g., `tenant_abc123`)
   - Stores: Users, Properties, Bookings (per tenant)
   - Domain: `<tenant-id>.renturo.test` (e.g., `main.renturo.test`)

**Verify Both Domains:**
```bash
# Central domain
curl http://renturo.test/login

# Tenant domain
curl http://main.renturo.test/login

# Both should return 200 OK (or show login page)
```

**Configure Valet for Both Domains:**
```bash
# If main.renturo.test doesn't work:
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/main
valet link renturo

# Valet automatically creates:
# - renturo.test (central)
# - *.renturo.test (all subdomains including main.renturo.test)
```

### Step 12: Understanding Authentication System

**⚠️ IMPORTANT:** Renturo uses **Laravel Passport** for API authentication.

#### Authentication Methods

| Platform | Method | Package | Used By |
|----------|--------|---------|---------|
| **Web (Central)** | Session Cookies | Laravel (built-in) | Super Admin |
| **Web (Tenant)** | Session Cookies | Laravel (built-in) | Admin |
| **Mobile Apps** | **OAuth2 Tokens** | **Laravel Passport** | Owner, User |

#### Laravel Passport (OAuth2 Server)

**What is it?**
- Full OAuth2 server implementation
- Personal Access Tokens for mobile apps
- Token refresh mechanism
- Scopes/Permissions support
- Multi-tenant OAuth clients

**Why Passport?**
1. ✅ **Token Refresh** - Mobile apps can refresh tokens without re-login
2. ✅ **Scopes** - Different permissions for Owner vs User
3. ✅ **OAuth2 Standard** - Industry standard for API authentication
4. ✅ **Multi-tenant Support** - Each tenant has its own Passport client

**Passport Tables:**
```sql
oauth_access_tokens              -- Active access tokens
oauth_auth_codes                 -- Authorization codes
oauth_clients                    -- OAuth2 clients (per tenant)
oauth_personal_access_clients    -- Personal access client config
oauth_refresh_tokens             -- Refresh tokens for token renewal
```

**Authentication Guards:**
```php
// config/auth.php
'guards' => [
    'web' => [
        'driver' => 'session',    // Web (Tenant) - Admin
        'provider' => 'users',
    ],
    'central' => [
        'driver' => 'session',    // Web (Central) - Super Admin
        'provider' => 'centrals',
    ],
    'api' => [
        'driver' => 'passport',   // Mobile Apps - Owner/User
        'provider' => 'users',
    ],
],
```

#### Mobile App Authentication Flow

**1. Login (Flutter App → Backend)**
```bash
POST /api/v1/login
Body: { "email": "owner@main.renturo.test", "password": "password" }

Response:
{
  "message": "success",
  "body": {
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGc...",
    "user": { "id": 1, "email": "owner@main.renturo.test", "role": "OWNER" }
  }
}
```

**2. Store Token (Flutter App)**
```dart
// Store in FlutterSecureStorage
await secureStorage.write(key: 'access_token', value: accessToken);
await secureStorage.write(key: 'user_details', value: jsonEncode(user));
```

**3. Authenticated Requests**
```bash
GET /api/v1/listings
Headers: {
  "Authorization": "Bearer eyJ0eXAiOiJKV1QiLCJhbGc...",
  "Accept": "application/json"
}
```

**4. Token Refresh (Optional)**
```bash
POST /api/v1/token/refresh
Body: { "refresh_token": "..." }
```

#### Passport Client Setup (Seeded Automatically)

**Central Passport Client:**
```php
// database/seeders/DatabaseSeeder.php
Artisan::call('passport:client', [
    '--personal' => true,
    '--name' => 'Renturo Personal Access Client'
]);
```

**Tenant Passport Client:**
```php
// database/seeders/TenantSeeder.php (runs per tenant)
$tenant->run(function () use ($tenant) {
    Artisan::call("tenants:run passport:client 
        --option='personal=personal' 
        --option='name={$tenant->id}' 
        --tenants={$tenant->id}"
    );
});
```

#### Verify Passport Installation

```bash
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/main

# Check Passport is installed
composer show laravel/passport

# Check OAuth tables exist
php artisan tinker
>>> \DB::table('oauth_clients')->count();

# List Passport routes
php artisan route:list | grep oauth
```

**Expected Passport Routes:**
- `POST /oauth/token` - Issue tokens
- `GET /oauth/tokens` - List user tokens
- `DELETE /oauth/tokens/{token_id}` - Revoke token
- `POST /oauth/personal-access-tokens` - Create personal token
- `DELETE /oauth/personal-access-tokens/{token_id}` - Delete personal token

#### Security Best Practices

1. **Token Storage:**
   - ✅ Flutter: Use `FlutterSecureStorage` (encrypted)
   - ❌ Never store tokens in SharedPreferences or plain storage

2. **Token Expiration:**
   - Access tokens expire (configurable in `config/passport.php`)
   - Use refresh tokens to get new access tokens

3. **HTTPS Required:**
   - ✅ Production: Always use HTTPS
   - ✅ Development: ngrok provides HTTPS tunnel

4. **Token Revocation:**
   - Logout should revoke tokens on server
   - User can view/revoke active tokens

---

## 🎨 Frontend Setup (React + Inertia.js)

### Overview
The `main/` directory contains both the Laravel backend **and** a React + Inertia.js frontend.

**Tech Stack:**
- React 18.2 (TypeScript)
- Inertia.js 1.0 (SPA without API)
- Vite 4.0 (Build tool with HMR)
- Tailwind CSS 3.2 (Styling)
- Headless UI (Component library)

### Step 1: Verify Node.js and npm

```bash
# Check Node.js version (should be 16+)
node --version

# Check npm version
npm --version

# If not installed, install Node.js from: https://nodejs.org/
```

### Step 2: Install Frontend Dependencies

```bash
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/main

# Install all npm packages
npm install

# This installs:
# - React & React DOM
# - Inertia.js
# - Vite
# - Tailwind CSS
# - TypeScript
# - All other dependencies from package.json
```

### Step 3: Verify Frontend File Structure

```bash
# Check resources directory
ls -la resources/js/

# Should contain:
# - app.tsx (React entry point)
# - components/ (React components)
# - pages/ (Inertia pages)
# - layouts/ (Page layouts)
# - types/ (TypeScript definitions)
```

### Step 4: Configure Vite

```bash
# Check vite.config.js exists
cat vite.config.js
```

Should contain Laravel Vite plugin configuration.

### Step 5: Start Vite Development Server

```bash
# Start Vite with hot reload
npm run dev

# You should see:
# ✓ Vite running at http://localhost:5173
# ✓ Laravel development server detected
```

**What this does:**
- ✅ Starts Vite dev server on `http://localhost:5173`
- ✅ Enables Hot Module Replacement (HMR)
- ✅ Auto-compiles React/TypeScript changes
- ✅ Auto-refreshes browser on file changes
- ✅ Proxies requests to Laravel backend

### Step 6: Access the Admin Dashboard

Open your browser and visit:

**Using Valet:**
```
http://renturo.test
```

**Using XAMPP:**
```
http://localhost/renturo/main/public
```

You should see the React/Inertia.js admin interface.

### Step 7: Verify Hot Reload

1. Keep `npm run dev` running
2. Edit any file in `resources/js/`
3. Save the file
4. Browser should auto-refresh with changes
5. Check terminal for compilation status

### Step 8: Build for Production (Optional)

```bash
# Create optimized production build
npm run build

# This creates:
# - public/build/manifest.json
# - public/build/assets/*.js
# - public/build/assets/*.css
```

### Frontend Development Workflow

```bash
# Terminal 1: Backend (Valet or XAMPP already running)
# http://renturo.test

# Terminal 2: Frontend dev server
cd main && npm run dev
# http://localhost:5173 (with hot reload)

# Now edit files in resources/js/
# Changes appear instantly in browser!
```

### Common npm Commands

```bash
# Install dependencies
npm install

# Start dev server (hot reload)
npm run dev

# Build for production
npm run build

# Check for outdated packages
npm outdated

# Update packages
npm update
```

---

## 📱 Client App Setup (Flutter - Owner App)

### Step 1: Navigate to Client Directory

```bash
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/client
```

### Step 2: Install Flutter Dependencies

```bash
flutter pub get

# If you encounter version conflicts:
flutter pub upgrade
```

### Step 3: Verify Flutter Configuration

```bash
# Check for issues
flutter doctor

# Run Flutter diagnostics
flutter doctor -v
```

### Step 4: Configure API Endpoint

```bash
# Edit: lib/config/config.dart
nano lib/config/config.dart
```

**Verify the dev configuration:**
```dart
factory Config.dev() {
  return Config(
    apiBaseUrl: 'https://renturo.ngrok.app'  // Will set up in next section
  );
}
```

### Step 5: Build Configuration Check

Verify Android build configuration:
```bash
# Check: android/app/build.gradle
cat android/app/build.gradle | grep -A 5 "defaultConfig"

# Expected output should include:
# minSdkVersion 26
# targetSdkVersion 35
# compileSdkVersion 35
```

### Step 6: Clean and Rebuild

```bash
# Clean previous builds
flutter clean

# Get dependencies again
flutter pub get

# Build for Android
flutter build apk --debug
```

---

## 📱 User App Setup (Flutter - Renter App)

### Step 1: Navigate to User Directory

```bash
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/user
```

### Step 2: Install Flutter Dependencies

```bash
flutter pub get
```

### Step 3: Configure API Endpoint

```bash
# Edit: lib/config/config.dart
nano lib/config/config.dart
```

**Verify the dev configuration:**
```dart
factory Config.dev() {
  return Config(
    apiBaseUrl: 'https://renturo.ngrok.app'  // Must match client app
  );
}
```

### Step 4: Verify Assets

```bash
# Check that assets exist
ls -la assets/images/
ls -la assets/inter/static/
```

### Step 5: Clean and Rebuild

```bash
flutter clean
flutter pub get
flutter build apk --debug
```

---

## 🚀 Running All Applications

### Complete Startup Sequence

#### Step 1: Start Backend Services

**Option A: Using Valet** ⭐ Recommended
```bash
# Already running!
# Just verify:
curl http://renturo.test

# Should return the app's homepage
```

**Option B: Using XAMPP**
```bash
# Open XAMPP Control Panel
# Start Apache
# Start MySQL

# Verify services are running:
curl http://localhost/renturo/main/public
# Should return the app's homepage
```

**Option C: Using Artisan**
```bash
# Open terminal
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/main
php artisan serve

# Runs at http://127.0.0.1:8000
```

#### Step 2: Start Frontend Dev Server (React + Vite)

```bash
# Open a new terminal window (Terminal 1)
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/main

# Start Vite development server
npm run dev

# You should see:
# ✓ Local:   http://localhost:5173
# ✓ ready in XXX ms

# Keep this terminal running!
# This enables hot reload for React development
```

**What this does:**
- ✅ Compiles React + TypeScript
- ✅ Enables Hot Module Replacement (HMR)
- ✅ Auto-refreshes browser on file changes
- ✅ Proxies API requests to Laravel backend

**Access Admin Dashboard:**
- Valet: http://renturo.test
- XAMPP: http://localhost/renturo/main/public
- Dev Server: http://localhost:5173 (with HMR)

#### Step 3: Start ngrok Tunnel (for mobile apps)

```bash
# Open a new terminal window
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/main

# Option A: Free ngrok (URL changes each restart)
ngrok http 80 --host-header=localhost

# Option B: Paid ngrok with custom domain (URL stays same)
ngrok http 80 --host-header=localhost --domain=renturo.ngrok.app

# You'll see output like:
# Forwarding  https://abc123.ngrok-free.app -> http://localhost:80
```

**Important:** Copy the `https://` URL provided by ngrok.

#### Step 4: Update App Configurations (Free ngrok only)

If using free ngrok (URL changes), update both mobile apps:

```bash
# Update client app
nano /Applications/XAMPP/xamppfiles/htdocs/renturo/client/lib/config/config.dart

# Update user app
nano /Applications/XAMPP/xamppfiles/htdocs/renturo/user/lib/config/config.dart

# Change apiBaseUrl to your new ngrok URL:
apiBaseUrl: 'https://your-new-ngrok-url.ngrok-free.app'
```

#### Step 5: Test Backend Connection

```bash
# Test the ngrok tunnel
curl https://your-ngrok-url.ngrok-free.app/api/v1/login

# Expected: JSON response (even if error, means it's working)
```

#### Step 6: Start Android Emulator

```bash
# List available emulators
flutter emulators

# Start your emulator
flutter emulators --launch <emulator_id>

# Or use Android Studio:
# Tools → Device Manager → Click Play button
```

**Wait for emulator to fully boot** (30-60 seconds)

#### Step 7: Run Client App (Owner App)

```bash
# Open terminal 3
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/client

# Run in dev mode
flutter run -d emulator-5554 -t lib/main_dev.dart

# Wait for "Flutter run key commands" message
# App should launch on emulator
```

#### Step 8: Run User App (Renter App)

```bash
# Open terminal 4
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/user

# Run in dev mode
flutter run -d emulator-5554 -t lib/main_dev.dart

# Note: Only one app can run at a time on the same emulator
# You'll need to stop the client app first or use a second emulator
```

### 📊 Running Summary - All Terminals

Here's what you should have running:

| Terminal | Application | Command | Status Check |
|----------|-------------|---------|--------------|
| **Background** | Backend (Valet/XAMPP) | `(running)` or `php artisan serve` | http://renturo.test |
| **Terminal 1** | Frontend (Vite) | `npm run dev` | http://localhost:5173 |
| **Terminal 2** | ngrok | `ngrok http 80 --host-header=localhost` | Check ngrok URL |
| **Terminal 3** | Client App | `flutter run -d emulator-5554 -t lib/main_dev.dart` | See app on emulator |
| **Terminal 4** | User App | `flutter run -d emulator-5554 -t lib/main_dev.dart` | See app on emulator |

### 🌐 Access Points Summary

| Application | URL | Purpose | User Role |
|-------------|-----|---------|-----------|
| **Central Web** | http://renturo.test/login | Manage tenants | Super Admin |
| **Admin Web** | http://main.renturo.test/login | Create listings | Admin (Web only) |
| **Vite Dev Server** | http://localhost:5173 (or 5174) | Hot reload development | Dev |
| **API (Central)** | http://renturo.test/api/v1/* | Central backend API | Super Admin |
| **API (Tenant)** | http://main.renturo.test/api/v1/* | Tenant backend API | All tenant users |
| **Mobile (ngrok)** | https://[your-url].ngrok.app | Mobile API access | Owner/User (Flutter) |
| **MySQL** | localhost:3306 | Database | Backend |

**Platform Usage:**
- **Web (Central):** Super Admin only
- **Web (Tenant):** Admin only (creates listings)
- **Mobile Client App:** Owner (saves listings)
- **Mobile User App:** User (rents properties)

---

## 🧪 Testing the Complete Flow

### Test 1: Backend API Health Check

```bash
# Test backend is accessible
curl http://localhost/renturo/main/public

# Test through ngrok
curl https://your-ngrok-url.ngrok-free.app/renturo/main/public/api/v1/login \
  -X POST \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{"email":"test@example.com","password":"password123"}'

# Expected: JSON response with user data or error message
```

### Test 2: Client App Registration Flow

1. Launch client app
2. Navigate to Register screen
3. Fill in registration form:
   - First name: Test
   - Last name: Owner
   - Email: owner@test.com
   - Phone: +639123456789
   - Password: password123
4. Click REGISTER
5. Check console for API calls:
   ```
   I/flutter: Calling API baseUrl...https://your-ngrok-url
   I/flutter: API Response: {...}
   ```

### Test 3: User App Login Flow

1. Launch user app
2. Enter credentials:
   - Email: user@test.com
   - Password: password123
3. Click LOGIN
4. Verify loading indicator shows
5. Check console for API response

### Test 4: Database Verification

```bash
# Access database
/Applications/XAMPP/xamppfiles/bin/mysql -u root renturo

# Check users table
SELECT * FROM users;

# Check access tokens
SELECT * FROM personal_access_tokens;

# Exit
exit;
```

---

## 🔍 Troubleshooting

### Backend Issues

#### Issue: "Class 'DOMDocument' not found"
**Solution:**
```bash
# Edit php.ini
nano /Applications/XAMPP/xamppfiles/etc/php.ini

# Uncomment this line:
extension=dom

# Restart Apache in XAMPP
```

#### Issue: "SQLSTATE[HY000] [2002] Connection refused"
**Solution:**
```bash
# 1. Verify MySQL is running in XAMPP
# 2. Check database credentials in .env
# 3. Test connection:
/Applications/XAMPP/xamppfiles/bin/mysql -u root -p

# 4. If password issues:
/Applications/XAMPP/xamppfiles/bin/mysql -u root
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('');
FLUSH PRIVILEGES;
```

#### Issue: "Route [login] not defined"
**Solution:**
```bash
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/main

# Clear cache
php artisan cache:clear
php artisan route:clear
php artisan config:clear
php artisan view:clear

# Regenerate
php artisan route:list
```

### Frontend Issues (React + Vite)

#### Issue: "npm: command not found"
**Solution:**
```bash
# Install Node.js from: https://nodejs.org/
# Or use Homebrew:
brew install node

# Verify installation
node --version
npm --version
```

#### Issue: "Cannot find module 'react'" or dependencies missing
**Solution:**
```bash
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/main

# Remove node_modules and reinstall
rm -rf node_modules package-lock.json
npm install

# If still failing, clear npm cache
npm cache clean --force
npm install
```

#### Issue: "Vite: Port 5173 already in use"
**Solution:**
```bash
# Option 1: Kill process on port 5173
lsof -ti:5173 | xargs kill -9

# Option 2: Use different port
npm run dev -- --port 5174

# Or update vite.config.js to use different default port
```

#### Issue: "Vite HMR not working / Changes not reflecting"
**Solution:**
```bash
# 1. Stop Vite (Ctrl + C)
# 2. Clear Vite cache
rm -rf node_modules/.vite

# 3. Restart Vite
npm run dev

# 4. Hard refresh browser
# macOS: Cmd + Shift + R
# Windows: Ctrl + Shift + R
```

#### Issue: "Inertia.js page not rendering"
**Solution:**
```bash
# 1. Check browser console for errors
# 2. Verify Vite is running: npm run dev
# 3. Check network tab for failed asset loads
# 4. Clear Laravel and Vite caches:

cd /Applications/XAMPP/xamppfiles/htdocs/renturo/main
php artisan cache:clear
php artisan view:clear
rm -rf node_modules/.vite
npm run dev
```

#### Issue: "Module parse failed: Unexpected token" (TypeScript errors)
**Solution:**
```bash
# 1. Ensure TypeScript is installed
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/main
npm install --save-dev typescript @types/react @types/react-dom

# 2. Restart Vite
npm run dev
```

#### Issue: "Cannot access admin dashboard / 404 error"
**Solution:**
```bash
# Verify both backend AND frontend are running:

# Terminal 1: Backend (should be running)
# Valet: http://renturo.test
# OR XAMPP: Start Apache in control panel

# Terminal 2: Frontend (must be running)
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/main
npm run dev

# Access via: http://renturo.test or http://localhost:5173
```

### Flutter Issues

#### Issue: "Gradle build failed with exit code 1"
**Solution:**
```bash
cd client  # or user
flutter clean
flutter pub get
cd android
./gradlew clean
cd ..
flutter run
```

#### Issue: "Waiting for another flutter command to release the startup lock"
**Solution:**
```bash
# Kill Flutter processes
killall -9 dart

# Delete lock file
rm -rf /Applications/XAMPP/xamppfiles/htdocs/renturo/client/bin/cache/lockfile
rm -rf /Applications/XAMPP/xamppfiles/htdocs/renturo/user/bin/cache/lockfile
```

#### Issue: "MissingPluginException"
**Solution:**
```bash
flutter clean
flutter pub get
flutter run --no-sound-null-safety  # If needed
```

#### Issue: "Unable to load asset"
**Solution:**
```bash
# Verify assets exist
ls -la assets/

# Check pubspec.yaml formatting
cat pubspec.yaml | grep -A 10 "assets:"

# Rebuild
flutter clean && flutter pub get
```

### ngrok Issues

#### Issue: "ngrok not found"
**Solution:**
```bash
# Install ngrok
brew install ngrok

# Or download manually
curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc | \
  sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null && \
  echo "deb https://ngrok-agent.s3.amazonaws.com buster main" | \
  sudo tee /etc/apt/sources.list.d/ngrok.list && \
  sudo apt update && sudo apt install ngrok
```

#### Issue: "ngrok tunnel closed"
**Solution:**
```bash
# Restart ngrok
pkill ngrok
ngrok http 80 --host-header=localhost

# Update apps with new URL if using free tier
```

#### Issue: "ERR_NGROK_3200 - Tunnel limit reached"
**Solution:**
- Free ngrok accounts have 1 tunnel limit
- Close other ngrok sessions
- Or upgrade to paid plan

### Emulator Issues

#### Issue: "No devices found"
**Solution:**
```bash
# List devices
flutter devices

# Check Android SDK
flutter doctor -v

# Restart ADB
adb kill-server
adb start-server

# List emulators
flutter emulators
```

#### Issue: "Emulator is slow/laggy"
**Solution:**
1. Close other applications
2. Allocate more RAM to emulator:
   - Android Studio → Device Manager
   - Edit device → Show Advanced Settings
   - Increase RAM to 4GB+
3. Enable hardware acceleration:
   - Check: System Preferences → Security & Privacy
   - Allow "Android Emulator"

#### Issue: "Process system isn't responding"
**Solution:**
- Click "Wait" - This is normal in debug mode
- Or run in release mode:
  ```bash
  flutter run --release
  ```

---

## 📊 System Health Checklist

Before running apps, verify all services:

### ✅ Pre-Flight Checklist

```bash
# 1. XAMPP Services
curl http://localhost
# Expected: XAMPP dashboard

# 2. MySQL Connection
/Applications/XAMPP/xamppfiles/bin/mysql -u root -e "SELECT 1"
# Expected: 1

# 3. Laravel Backend
curl http://localhost/renturo/main/public
# Expected: HTML or JSON response

# 4. ngrok Tunnel
curl https://your-ngrok-url.ngrok-free.app/renturo/main/public
# Expected: HTML or JSON response

# 5. Flutter Installation
flutter doctor
# Expected: All green checks

# 6. Android Emulator
flutter devices
# Expected: At least one emulator listed

# 7. Flutter Packages
cd client && flutter pub get && cd ..
cd user && flutter pub get && cd ..
# Expected: No errors
```

---

## 📝 Quick Start Commands

### Start Everything (Sequential)

```bash
# Terminal 1: Start ngrok
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/main
ngrok http 80 --host-header=localhost

# Terminal 2: Start client app
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/client
flutter run -d emulator-5554 -t lib/main_dev.dart

# Terminal 3: Start user app (use different emulator or stop client first)
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/user
flutter run -d emulator-5554 -t lib/main_dev.dart
```

### Stop Everything

```bash
# Stop Flutter apps: Press 'q' in terminal

# Stop ngrok: Press Ctrl+C

# Stop XAMPP: Use XAMPP Control Panel
```

---

## 🆘 Getting Help

### Resources
- **Flutter Docs:** https://docs.flutter.dev
- **Laravel Docs:** https://laravel.com/docs
- **ngrok Docs:** https://ngrok.com/docs

### Logs Location
```bash
# Laravel logs
tail -f /Applications/XAMPP/xamppfiles/htdocs/renturo/main/storage/logs/laravel.log

# Apache logs
tail -f /Applications/XAMPP/xamppfiles/logs/error_log

# Flutter logs
# Displayed in terminal where flutter run is executed
```

### Debug Commands
```bash
# Check Flutter configuration
flutter doctor -v

# Check connected devices
flutter devices

# Check running processes
ps aux | grep flutter
ps aux | grep ngrok

# Check ports
lsof -i :80
lsof -i :3306
```

---

## 📞 Contact & Support

- **Project Repository:** [GitHub Link]
- **Issue Tracker:** [GitHub Issues]
- **Documentation:** This file

---

**Last Updated:** October 12, 2025  
**Maintained By:** Renturo Development Team

