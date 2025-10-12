# 🚀 Renturo - Quick Start Guide

**Daily startup commands for local development**

---

## ⚡ Quick Start - All Applications

### 1️⃣ Start Backend (Laravel)

**Option A: Using Valet** ⭐ Recommended
```bash
# Already running! Just access:
# http://renturo.test
```

**Option B: Using XAMPP**
```bash
# Open XAMPP Control Panel
# ✅ Start Apache
# ✅ Start MySQL
# Access: http://localhost/renturo/main/public
```

**Option C: Using Artisan**
```bash
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/main
php artisan serve
# Access: http://127.0.0.1:8000
```

### 2️⃣ Start Frontend (React + Vite)
```bash
# Terminal 1: Switch to Node 18 & Start Vite
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/main

# ⚠️ IMPORTANT: Use Node 18 (required for Vite)
nvm use 18.20.8

# Start Vite dev server
npm run dev

# ✅ Opens at http://localhost:5173
# ✅ Hot reload enabled for React changes
```

**💡 Tip:** If you get `ERR_REQUIRE_ESM` error, you're using Node 12. Run `nvm use 18.20.8` first!

### 3️⃣ Start ngrok (for mobile apps)
```bash
# Terminal 2: Start ngrok
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/main
ngrok http 80 --host-header=localhost

# Copy the https URL (e.g., https://abc123.ngrok-free.app)
```

### 4️⃣ Run Flutter Apps
```bash
# Terminal 3: Client App (Owner)
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/client
flutter run -d emulator-5554 -t lib/main_dev.dart

# Terminal 4: User App (Renter) - Optional
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/user
flutter run -d emulator-5554 -t lib/main_dev.dart
```

---

## 🌐 Access URLs

**Multi-Tenant Architecture:**

| Application | URL | Purpose | User |
|-------------|-----|---------|------|
| **Central Web** | http://renturo.test/login | Manage tenants | Super Admin |
| **Admin Web** | http://main.renturo.test/login | Create listings | Admin (Web only) |
| **Vite Dev** | http://localhost:5173 (or 5174) | Hot reload | Development |
| **API (Central)** | http://renturo.test/api/v1/* | Central API | Super Admin |
| **API (Tenant)** | http://main.renturo.test/api/v1/* | Tenant API | Admin/Owner/User |
| **Mobile (ngrok)** | https://[your-url].ngrok.app | Mobile API | Owner/User (Flutter) |

**Note:** Owner and User roles use **mobile apps only**, not the web interface.

---

## 🔧 If ngrok URL Changes

**Only needed for FREE ngrok accounts** (paid accounts have static URLs)

```bash
# 1. Update client/lib/config/config.dart
# 2. Update user/lib/config/config.dart
# Change line: apiBaseUrl: 'https://YOUR-NEW-NGROK-URL'

# 3. Hot restart the app (press 'R' in Flutter terminal)
```

---

## 💻 React/Vite Commands

While `npm run dev` is running:

```bash
# Automatic hot reload
# ✅ Edit any file in resources/js/*
# ✅ Changes appear instantly in browser
# ✅ No manual refresh needed!

# Stop Vite server
Press Ctrl + C

# Restart Vite server
npm run dev

# Build for production
npm run build
```

---

## 📱 Flutter Commands

While `flutter run` is running:

```bash
# Hot reload (quick changes)
Press 'r'

# Hot restart (full restart)
Press 'R'

# Clear screen
Press 'c'

# Quit app
Press 'q'

# Open DevTools
Press 'd'
```

---

## 👤 Test User Accounts

Use these pre-seeded accounts for testing:

```bash
# 🔐 CENTRAL LOGIN (http://renturo.test/login)
Email: super-admin@renturo.test
Password: password
Role: Super Admin
Platform: Web (Central)
Purpose: Manages all tenants

# 🏢 ADMIN LOGIN (http://main.renturo.test/login)
Email: admin@main.renturo.test
Password: password
Role: Admin
Platform: Web (Tenant)
Purpose: Creates and manages listing properties

# 📱 CLIENT APP (Flutter - Property Owners)
Email: owner@main.renturo.test
Password: password
Role: Owner
Platform: Mobile (Client App)
Purpose: Saves property listings

# 📱 USER APP (Flutter - Renters)
Email: user@main.renturo.test
Password: password
Role: User
Platform: Mobile (User App)
Purpose: Browses and rents properties
```

**💡 Important:** 
- **Super Admin** → Web: `http://renturo.test/login`
- **Admin** → Web: `http://main.renturo.test/login` (creates listings)
- **Owner** → Mobile Client App (saves listings)
- **User** → Mobile User App (rents properties)

**Need to seed database?**
```bash
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/main
php artisan migrate:fresh --seed
```

---

## 🐛 Quick Fixes

### App won't connect to backend?
```bash
# 1. Check ngrok is running
curl https://your-ngrok-url.ngrok-free.app/renturo/main/public

# 2. Check XAMPP services are running
# 3. Hot restart Flutter app (press 'R')
```

### Flutter build errors?
```bash
flutter clean
flutter pub get
flutter run
```

### Database errors?
```bash
# Check MySQL in XAMPP Control Panel
# Restart MySQL if needed
```

---

## 📊 Health Check

```bash
# ✅ XAMPP: http://localhost
# ✅ Backend: http://localhost/renturo/main/public
# ✅ ngrok: https://your-url.ngrok-free.app/renturo/main/public
# ✅ Emulator: flutter devices
```

---

## 🛑 Stop Everything

```bash
# 1. Flutter: Press 'q' in terminal
# 2. ngrok: Press Ctrl+C
# 3. XAMPP: Click 'Stop' for Apache & MySQL
```

---

**Need detailed setup?** See [DEVELOPMENT_SETUP.md](./DEVELOPMENT_SETUP.md)

