# 🏠 Renturo - Property Rental Management System

**Multi-platform rental management solution for property owners and renters**

---

## 📱 Applications

### 1. Backend API (Laravel)
- **Location:** `/main`
- **Purpose:** RESTful API for all client applications
- **Stack:** Laravel 10, MySQL, PHP 8.1+

### 2. Client App (Flutter - Property Owners)
- **Location:** `/client`
- **Purpose:** Mobile app for property owners to manage listings
- **Stack:** Flutter 3.7+, Dart 3.7+, BLoC pattern

### 3. User App (Flutter - Renters)
- **Location:** `/user`
- **Purpose:** Mobile app for renters to browse and book properties
- **Stack:** Flutter 3.7+, Dart 3.7+, BLoC pattern

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
# 1. Start XAMPP (Apache + MySQL)

# 2. Start ngrok
cd main && ngrok http 80 --host-header=localhost

# 3. Run Client App
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

### Backend
- **Framework:** Laravel 10
- **Database:** MySQL 8.0
- **Server:** XAMPP (Apache)
- **API:** RESTful JSON API
- **Authentication:** Laravel Sanctum

### Mobile Apps
- **Framework:** Flutter 3.7+
- **Language:** Dart 3.7+
- **State Management:** BLoC Pattern
- **HTTP Client:** http package
- **Secure Storage:** flutter_secure_storage
- **UI Components:** Material Design 3

### Development Tools
- **Version Control:** Git
- **API Tunneling:** ngrok
- **IDE:** VS Code, Android Studio
- **Emulator:** Android Emulator

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

### 1. Clone Repository
```bash
git clone <repository-url>
cd renturo
```

### 2. Backend Setup
```bash
cd main
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate
```

### 3. Client App Setup
```bash
cd client
flutter pub get
```

### 4. User App Setup
```bash
cd user
flutter pub get
```

**Detailed setup:** [DEVELOPMENT_SETUP.md](./DEVELOPMENT_SETUP.md)

---

## 🚀 Running the Applications

### Start Backend + ngrok
```bash
# Terminal 1: Start XAMPP (GUI)

# Terminal 2: Start ngrok
cd main
ngrok http 80 --host-header=localhost
```

### Run Client App
```bash
cd client
flutter run -d emulator-5554 -t lib/main_dev.dart
```

### Run User App
```bash
cd user
flutter run -d emulator-5554 -t lib/main_dev.dart
```

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

