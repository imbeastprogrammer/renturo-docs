# 📋 Renturo Development Checklist

Use this checklist to ensure everything is properly set up before development.

---

## ✅ Initial Setup (One-Time)

### Software Installation
- [ ] XAMPP installed and configured
- [ ] Composer installed globally
- [ ] Flutter SDK installed (3.7.0+)
- [ ] Android Studio installed
- [ ] Android Emulator created
- [ ] ngrok installed and authenticated
- [ ] Git configured

### Backend Setup (main/)
- [ ] `composer install` completed
- [ ] `.env` file created and configured
- [ ] Database `renturo` created
- [ ] `php artisan migrate` run successfully
- [ ] Storage link created
- [ ] Can access: `http://localhost/renturo/main/public`

### Client App Setup (client/)
- [ ] `flutter pub get` completed
- [ ] Assets verified (fonts, images)
- [ ] Android build files configured
- [ ] `flutter doctor` shows no errors
- [ ] Debug build succeeds

### User App Setup (user/)
- [ ] `flutter pub get` completed
- [ ] Assets copied from client app
- [ ] Android build files configured (Groovy DSL)
- [ ] Directory structure matches client app
- [ ] Debug build succeeds

---

## 🔄 Daily Development Checklist

### Before Starting Work

#### 1. Services Status
- [ ] XAMPP Control Panel open
- [ ] Apache running (green light)
- [ ] MySQL running (green light)
- [ ] Backend accessible: `http://localhost/renturo/main/public`

#### 2. ngrok Tunnel
- [ ] ngrok running in terminal
- [ ] Tunnel URL copied
- [ ] Backend accessible via ngrok URL
- [ ] Test API endpoint works

#### 3. Flutter Environment
- [ ] Android emulator started
- [ ] Emulator fully booted
- [ ] `flutter devices` shows emulator
- [ ] No "doctor" errors

#### 4. Configuration Check
- [ ] API baseUrl matches ngrok URL (if using free ngrok)
- [ ] `.env` file has correct database credentials
- [ ] No pending git conflicts

---

## 🧪 Testing Checklist

### Backend API Testing
- [ ] Login endpoint works: `POST /api/v1/login`
- [ ] Register endpoint works: `POST /api/v1/register`
- [ ] OTP endpoint works: `POST /api/v1/verify/mobile`
- [ ] Database queries return expected data
- [ ] Error responses are formatted correctly

### Client App Testing
- [ ] App launches without crashes
- [ ] Logo displays correctly
- [ ] Registration form works
- [ ] Login form works
- [ ] API calls succeed
- [ ] Loading indicators show
- [ ] Error dialogs display properly
- [ ] Navigation works between screens

### User App Testing
- [ ] App launches without crashes
- [ ] Login screen displays correctly
- [ ] Form validation works
- [ ] Password visibility toggle works
- [ ] API connection successful
- [ ] Loading state shows during login
- [ ] Error handling works

---

## 🔍 Code Quality Checklist

### Before Committing
- [ ] No console errors in Flutter
- [ ] No linter warnings
- [ ] Code formatted (`flutter format .`)
- [ ] No debug print statements left
- [ ] Comments added for complex logic
- [ ] Unused imports removed
- [ ] Variables properly named

### Before Pushing
- [ ] All tests pass (if tests exist)
- [ ] App builds without errors
- [ ] No sensitive data in code (tokens, passwords)
- [ ] `.gitignore` properly configured
- [ ] Meaningful commit message
- [ ] Changes documented in PR/commit

---

## 🐛 Debugging Checklist

### Flutter App Issues
- [ ] Run `flutter clean`
- [ ] Run `flutter pub get`
- [ ] Clear app data on emulator
- [ ] Restart emulator
- [ ] Check console for error messages
- [ ] Verify API endpoint URL
- [ ] Check network connectivity

### Backend Issues
- [ ] Check Laravel logs: `storage/logs/laravel.log`
- [ ] Verify database connection
- [ ] Clear Laravel cache
- [ ] Check Apache error logs
- [ ] Verify route exists (`php artisan route:list`)
- [ ] Check middleware configuration

### ngrok Issues
- [ ] Restart ngrok tunnel
- [ ] Update app config with new URL
- [ ] Test tunnel with curl
- [ ] Check ngrok dashboard for errors
- [ ] Verify auth token is set

---

## 📦 Deployment Checklist

### Before Deploying Backend
- [ ] Environment variables set correctly
- [ ] Database migrations up to date
- [ ] Storage permissions set
- [ ] Queue workers configured (if using)
- [ ] Cron jobs set up (if needed)
- [ ] SSL certificate configured
- [ ] .env.production configured

### Before Deploying Mobile Apps
- [ ] Production API URL set
- [ ] App version incremented
- [ ] Release build succeeds
- [ ] App signing configured
- [ ] App icons added
- [ ] Splash screen configured
- [ ] App permissions reviewed
- [ ] Store listing prepared

---

## 🔐 Security Checklist

### Backend Security
- [ ] No debug mode in production
- [ ] API rate limiting enabled
- [ ] CORS properly configured
- [ ] SQL injection prevention (using Eloquent)
- [ ] XSS protection enabled
- [ ] CSRF tokens validated
- [ ] Authentication required on protected routes

### Mobile App Security
- [ ] Secure storage used for tokens
- [ ] No hardcoded credentials
- [ ] HTTPS enforced
- [ ] Certificate pinning (optional)
- [ ] ProGuard enabled (Android release)
- [ ] Code obfuscation enabled

---

## 📝 Documentation Checklist

### Code Documentation
- [ ] README files up to date
- [ ] API endpoints documented
- [ ] Setup instructions current
- [ ] Troubleshooting guide updated
- [ ] Architecture documented

### User Documentation
- [ ] User flows documented
- [ ] Screenshots updated
- [ ] Help text accurate
- [ ] Error messages clear

---

## 🎯 Performance Checklist

### Flutter App
- [ ] Images optimized
- [ ] Unnecessary rebuilds avoided
- [ ] Lazy loading implemented
- [ ] Memory leaks checked
- [ ] Animations smooth (60fps)

### Backend
- [ ] Database queries optimized
- [ ] Indexes added where needed
- [ ] N+1 queries avoided
- [ ] Caching implemented
- [ ] API response times < 500ms

---

## ✨ Final Check

### Before Demo/Presentation
- [ ] All features working
- [ ] No placeholder data
- [ ] UI polished
- [ ] Transitions smooth
- [ ] No console errors
- [ ] Test data prepared
- [ ] Demo script ready
- [ ] Backup plan prepared

---

**Use this checklist daily to ensure smooth development! ✅**

Save a copy and check off items as you complete them.

