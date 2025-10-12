# 🚀 Renturo - Quick Start Guide

**Daily startup commands for local development**

---

## ⚡ Quick Start (3 Steps)

### 1️⃣ Start XAMPP
```bash
# Open XAMPP Control Panel
# ✅ Start Apache
# ✅ Start MySQL
```

### 2️⃣ Start ngrok
```bash
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/main
ngrok http 80 --host-header=localhost

# Copy the https URL (e.g., https://abc123.ngrok-free.app)
```

### 3️⃣ Run Flutter App
```bash
# For Client App (Owner)
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/client
flutter run -d emulator-5554 -t lib/main_dev.dart

# OR For User App (Renter)
cd /Applications/XAMPP/xamppfiles/htdocs/renturo/user
flutter run -d emulator-5554 -t lib/main_dev.dart
```

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

## 📱 Flutter Commands

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

