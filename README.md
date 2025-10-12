# 🏠 Renturo - Property Rental Management System

**Multi-platform rental management solution for property owners and renters**

---

## 📱 Applications

### 1. Admin Web Application (Full-Stack) - Multi-Tenant
- **Location:** `/main`
- **Purpose:** Web-based admin dashboard for property management
- **Backend:** Laravel 9.19 (PHP 8.3), MySQL (Multi-tenant)
- **Frontend:** React 18 + Inertia.js, TypeScript, Tailwind CSS
- **Build Tool:** Vite
- **Architecture:** Multi-tenant (Central + Tenant databases)
- **Users:** Super Admin (central), Admin (tenant)
- **Access:** 
  - **Central Login:** `http://renturo.test/login` (Super Admin - manages tenants)
  - **Tenant Login:** `http://main.renturo.test/login` (Admin - creates listings)
  - **API (Central):** `http://renturo.test/api/v1/*`
  - **API (Tenant):** `http://main.renturo.test/api/v1/*`
- **Note:** Owner and User roles use mobile apps only

### 2. Client App (Flutter - Property Owners)
- **Location:** `/client`
- **Purpose:** Mobile app for property owners to save listings
- **User Role:** Owner
- **Actions:** Save property listings (created by Admin via web)
- **Stack:** Flutter 3.7+, Dart 3.7+, BLoC pattern
- **Platform:** Android, iOS
- **Login:** `owner@main.renturo.test` / `password`

### 3. User App (Flutter - Renters)
- **Location:** `/user`
- **Purpose:** Mobile app for renters to browse and rent properties
- **User Role:** User
- **Actions:** Browse listings, rent properties, manage bookings
- **Stack:** Flutter 3.7+, Dart 3.7+, BLoC pattern
- **Platform:** Android, iOS
- **Login:** `user@main.renturo.test` / `password`

---

## 📚 Documentation

### 🚀 Getting Started
Choose the right guide for your needs:

| Document | Purpose | When to Use |
|----------|---------|-------------|
| **[QUICK_START.md](./QUICK_START.md)** | Daily startup commands | Every development session |
| **[DEVELOPMENT_SETUP.md](./DEVELOPMENT_SETUP.md)** | Complete setup guide | First-time setup & troubleshooting |
| **[DEV_CHECKLIST.md](./DEV_CHECKLIST.md)** | Development checklist | Before committing & deploying |

### 📖 Quick Links

- **New Developer?** → Start with [DEVELOPMENT_SETUP.md](./DEVELOPMENT_SETUP.md)
- **Need Quick Start?** → Use [QUICK_START.md](./QUICK_START.md)
- **Daily Development?** → Check [DEV_CHECKLIST.md](./DEV_CHECKLIST.md)
- **Troubleshooting?** → See [DEVELOPMENT_SETUP.md#troubleshooting](./DEVELOPMENT_SETUP.md#troubleshooting)

---

## ⚡ Quick Start

```bash
# 1. Start Backend (Laravel via Valet or XAMPP)
# Valet: Already running at http://renturo.test
# OR XAMPP: Start Apache + MySQL in XAMPP Control Panel

# 2. Start Frontend Dev Server (React + Vite)
cd main && npm run dev

# 3. Start ngrok (for mobile apps)
ngrok http 80 --host-header=localhost

# 4. Run Client App
cd client && flutter run -d emulator-5554 -t lib/main_dev.dart

# OR Run User App
cd user && flutter run -d emulator-5554 -t lib/main_dev.dart
```

**Need more details?** See [QUICK_START.md](./QUICK_START.md)

---

## 🏗️ Project Structure

```
renturo/
├── main/                       # Laravel Backend
│   ├── app/
│   ├── database/
│   ├── routes/
│   └── public/
│
├── client/                     # Flutter Owner App
│   ├── lib/
│   │   ├── blocs/             # State management
│   │   ├── models/            # Data models
│   │   ├── screens/           # UI screens
│   │   ├── services/          # API services
│   │   └── widgets/           # Reusable components
│   ├── android/
│   └── ios/
│
├── user/                       # Flutter Renter App
│   ├── lib/
│   │   ├── blocs/             # State management
│   │   ├── models/            # Data models
│   │   ├── screens/           # UI screens
│   │   ├── services/          # API services
│   │   └── widgets/           # Reusable components
│   ├── android/
│   └── ios/
│
└── Documentation/
    ├── QUICK_START.md
    ├── DEVELOPMENT_SETUP.md
    └── DEV_CHECKLIST.md
```

---

## 🛠️ Tech Stack

### Admin Web Application (main/)
**Backend:**
- **Framework:** Laravel 9.19
- **Database:** MySQL 8.0
- **Server:** Laravel Valet / XAMPP (Apache)
- **API:** RESTful JSON API
- **Authentication:** 
  - **Web:** Session-based (built-in Laravel)
  - **Mobile API:** **Laravel Passport** (OAuth2)
- **Multi-Tenancy:** Stancl/Tenancy
- **PHP:** 8.3

**Frontend:**
- **Framework:** React 18.2
- **Routing:** Inertia.js 1.0
- **Language:** TypeScript
- **Styling:** Tailwind CSS 3.2
- **Build Tool:** Vite 4.0
- **UI Components:** Headless UI
- **State:** React Hooks

### Mobile Apps (client/ & user/)
- **Framework:** Flutter 3.7+
- **Language:** Dart 3.7+
- **State Management:** BLoC Pattern
- **HTTP Client:** http package
- **Secure Storage:** flutter_secure_storage
- **UI Components:** Material Design 3
- **Platform:** Android, iOS

### Development Tools
- **Version Control:** Git, GitHub
- **API Tunneling:** ngrok
- **Package Managers:** Composer (PHP), npm (Node.js), pub (Flutter)
- **IDE:** VS Code, Android Studio, PHPStorm
- **Emulator:** Android Emulator
- **Hot Reload:** Vite HMR, Flutter Hot Reload

---

## 🎯 Features

### Backend (API)
✅ User authentication (register, login, logout)  
✅ OTP verification  
✅ Password management  
✅ Property listing management  
✅ Booking system  
✅ User profile management  
✅ Multi-tenant support  

### Client App (Owners)
✅ User registration & authentication  
✅ Store/business management  
✅ Property listing creation  
✅ Dynamic form handling  
✅ Image uploads  
✅ Booking management  
✅ MPIN security  

### User App (Renters)
✅ User registration & authentication  
✅ Property browsing  
✅ Search & filters  
🚧 Booking system (in progress)  
🚧 Favorites (in progress)  
🚧 Reviews & ratings (planned)  

---

## 🔧 Prerequisites

- **PHP** 8.1+
- **Composer** 2.x
- **Flutter** 3.7.0+
- **Dart** 3.7.0+
- **XAMPP** (Apache + MySQL)
- **ngrok** (for API tunneling)
- **Android Studio** (with Android SDK)

**Full installation guide:** [DEVELOPMENT_SETUP.md#prerequisites-installation](./DEVELOPMENT_SETUP.md#prerequisites-installation)

---

## 📦 Installation

### 1. Clone Repositories
```bash
# Backend & Web App
git clone https://github.com/imbeastprogrammer/renturo-main.git main
cd main
composer install
npm install
cp .env.example .env
php artisan key:generate
php artisan migrate

# Client App (Owner)
git clone https://github.com/imbeastprogrammer/renturo-client.git client
cd client
flutter pub get

# User App (Renter)
git clone https://github.com/imbeastprogrammer/renturo-user.git user
cd user
flutter pub get
```

### 2. Quick Setup (if already cloned)
```bash
# Backend
cd main
composer install
npm install

# Frontend dependencies
cd main && npm install

# Flutter apps
cd client && flutter pub get
cd user && flutter pub get
```

**Detailed setup:** [DEVELOPMENT_SETUP.md](./DEVELOPMENT_SETUP.md)

---

## 🚀 Running the Applications

### Complete Startup (All 4 Applications)

```bash
# Terminal 1: Backend (Laravel)
# Option A: Valet (already running)
http://renturo.test
# Option B: XAMPP (start Apache + MySQL)
# Option C: Artisan
cd main && php artisan serve

# Terminal 2: Frontend (React + Vite)
cd main && npm run dev
# Runs at http://localhost:5173 with hot reload

# Terminal 3: ngrok (for mobile apps)
ngrok http 80 --host-header=localhost
# Copy the https:// URL for mobile apps

# Terminal 4: Client App (Owner)
cd client && flutter run -d emulator-5554 -t lib/main_dev.dart

# Terminal 5: User App (Renter)
cd user && flutter run -d emulator-5554 -t lib/main_dev.dart
```

### Access Points

| Application | URL | Purpose |
|-------------|-----|---------|
| **Admin Dashboard** | http://renturo.test | Web interface (React + Inertia) |
| **Vite Dev Server** | http://localhost:5173 | Hot reload for React dev |
| **API Endpoints** | http://renturo.test/api/v1/* | Backend API |
| **Mobile Access** | https://[ngrok-url] | Flutter apps via ngrok |

---

## 🧪 Testing

### Test Backend
```bash
cd main
php artisan test
```

### Test Flutter Apps
```bash
cd client  # or user
flutter test
```

### Manual Testing
See [DEVELOPMENT_SETUP.md#testing-the-complete-flow](./DEVELOPMENT_SETUP.md#testing-the-complete-flow)

---

## 📝 Development Workflow

1. **Start Development Session**
   - Follow [QUICK_START.md](./QUICK_START.md)
   - Check [DEV_CHECKLIST.md](./DEV_CHECKLIST.md)

2. **Make Changes**
   - Follow code style guidelines
   - Write tests for new features
   - Update documentation

3. **Before Committing**
   - Run linter: `flutter analyze`
   - Format code: `flutter format .`
   - Check [DEV_CHECKLIST.md#before-committing](./DEV_CHECKLIST.md#before-committing)

4. **Commit & Push**
   - Write meaningful commit messages
   - Reference issue numbers
   - Create pull request

---

## 🐛 Troubleshooting

### Common Issues

**App won't connect to backend?**
- Check ngrok is running
- Verify API URL in config
- Test backend directly

**Flutter build fails?**
```bash
flutter clean
flutter pub get
flutter run
```

**Database connection error?**
- Check MySQL is running
- Verify .env credentials
- Test connection manually

**Full troubleshooting guide:** [DEVELOPMENT_SETUP.md#troubleshooting](./DEVELOPMENT_SETUP.md#troubleshooting)

---

## 📚 Additional Resources

### Documentation
- [Laravel Documentation](https://laravel.com/docs)
- [Flutter Documentation](https://docs.flutter.dev)
- [BLoC Pattern Guide](https://bloclibrary.dev)

### Community
- GitHub Issues: [Report bugs]
- Pull Requests: [Contribute code]
- Discussions: [Ask questions]

---

## 🔐 Environment Variables

### Backend (.env)
```env
APP_NAME=Renturo
APP_ENV=local
APP_DEBUG=true
DB_DATABASE=renturo
DB_USERNAME=root
DB_PASSWORD=
```

### Mobile Apps (config.dart)
```dart
// Development
apiBaseUrl: 'https://renturo.ngrok.app'

// Production
apiBaseUrl: 'https://api.renturo.com'
```

---

## 📄 License

[License Type] - See LICENSE file for details

---

## 👥 Team

- **Backend Development:** [Team Members]
- **Mobile Development:** [Team Members]
- **UI/UX Design:** [Team Members]
- **Project Management:** [Team Members]

---

## 📞 Support

- **Documentation:** See `/docs` folder
- **Issues:** GitHub Issues
- **Email:** support@renturo.com

---

## 🎉 Getting Help

1. **Check Documentation:** Start with relevant .md files
2. **Search Issues:** Look for similar problems
3. **Ask Team:** Reach out on communication channels
4. **Create Issue:** Provide detailed information

---

**Ready to start?** → Open [QUICK_START.md](./QUICK_START.md) 🚀

