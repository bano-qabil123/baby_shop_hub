# 🛒 BabyShopHub - Baby E-Commerce App

[![Flutter](https://img.shields.io/badge/Flutter-3.8.1-blue?logo=flutter)](https://flutter.dev/)
[![Firebase](https://img.shields.io/badge/Firebase-FFCA28?logo=firebase&logoColor=black)](https://firebase.google.com/)
[![Dart](https://img.shields.io/badge/Dart-3.x-blue?logo=dart)](https://dart.dev/)
[![Provider](https://img.shields.io/badge/State%20Management-Provider-green)](https://pub.dev/packages/provider)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

> A modern Flutter e-commerce app for baby products, built as an APTECH semester-end project.

## ⚠️ Important: Firebase Setup Required

**This app requires a Firebase project to run.** Firebase is not pre-configured. Follow the setup guide below to get started in about 5 minutes.

## 📱 Features

- **User Authentication**: Secure login & registration via Firebase
- **Product Browsing**: Browse baby products by category
- **Shopping Cart**: Add, remove, and manage cart items
- **Wishlist**: Save favorite products
- **Order Management**: Track your orders in real-time
- **Admin Panel**: Full store management dashboard with analytics
- **Real-time Updates**: Live notifications via Firebase Cloud Messaging

## 🚀 Quick Start (Firebase Setup)

### Prerequisites
- Flutter 3.8.1+ ([Install here](https://flutter.dev/docs/get-started/install))
- Git
- A Google account (for Firebase)

### Step 1: Clone the Repository
```bash
git clone https://github.com/bano-qabil123/baby_shop_hub.git
cd baby_shop_hub/baby_shophub
```

### Step 2: Create a Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click **"Add project"**
3. Enter a project name (e.g., "BabyShopHub-Dev")
4. Click **"Create project"** and wait for it to complete

### Step 3: Register Your Android App with Firebase

1. In Firebase Console, click **"Add app"** → Select **"Android"**
2. **Package Name**: Enter `com.example.babyshophub` (or your desired package name)
   - *(Find this in `android/app/build.gradle` if you need to verify)*
3. Click **"Register app"**
4. **Download `google-services.json`**
5. Place the file in: `android/app/google-services.json`
6. Click **"Next"** through the remaining steps (Flutter handles SDK setup automatically)

### Step 4: Enable Firebase Services

In Firebase Console, go to the **"Build"** section (left sidebar) and enable:

- ✅ **Authentication**
  - Click on "Authentication" → "Get started"
  - Enable **"Email/Password"** sign-in method
  
- ✅ **Firestore Database**
  - Click on "Firestore Database" → "Create database"
  - Start in **"Test mode"** (for development)
  - Choose your region and click "Create"
  
- ✅ **Storage**
  - Click on "Storage" → "Get started"
  - Start in **"Test mode"** and click "Create"
  
- ✅ **Cloud Messaging**
  - No specific setup needed; enabled by default

### Step 5: Configure Security Rules

**⚠️ Important for Production**: These test rules are permissive for development. Update them before deploying to production.

**For Firestore**, go to **Firestore Database** → **"Rules"** tab and paste:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users can only read/write their own data
    match /users/{userId} {
      allow read, write: if request.auth.uid == userId;
    }
    
    // Public product catalog (anyone can read, only admins can write)
    match /products/{document=**} {
      allow read: if true;
      allow write: if request.auth.token.admin == true;
    }
    
    // Default: authenticated users can read/write
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

Click **"Publish"**

**For Storage**, go to **Storage** → **"Rules"** tab and paste:

```
service firebase.storage {
  match /b/{bucket}/o {
    // Product images: public read, admin write only
    match /product_images/{allPaths=**} {
      allow read: if true;
      allow write: if request.auth.token.admin == true;
    }
    
    // User uploads: private per user
    match /user_uploads/{userId}/{allPaths=**} {
      allow read, write: if request.auth.uid == userId;
    }
  }
}
```

Click **"Publish"**

### Step 6: Install Dependencies & Run

```bash
# Install Flutter dependencies
flutter pub get

# For iOS (if developing on macOS)
cd ../ios && pod install && cd ../

# Run the app
flutter run
```

That's it! 🎉 The app should now be running with Firebase connected.

## 🛠️ Tech Stack

| Component | Technology |
|-----------|-----------|
| **Frontend** | Flutter 3.8.1 + Dart |
| **State Management** | Provider pattern |
| **Backend** | Firebase (Firestore, Auth, Storage, Messaging) |
| **Design System** | Material Design |

## 📊 Project Structure

```
baby_shop_hub/
├── baby_shophub/
│   ├── lib/
│   │   ├── models/        # Data models (User, Product, Cart, Order)
│   │   ├── providers/     # State management with Provider
│   │   ├── screens/       # UI screens and pages
│   │   ├── services/      # Firebase services
│   │   ├── widgets/       # Reusable UI components
│   │   ├── theme/         # App theming
│   │   ├── utils/         # Utilities and constants
│   │   └── main.dart      # App entry point
│   ├── android/           # Android config (add google-services.json here)
│   ├── ios/              # iOS config
│   ├── assets/           # App images and resources
│   └── pubspec.yaml      # Dependencies
└── README.md
```

## 🤝 Contributing

We welcome contributions! Whether you're fixing bugs, adding features, or improving the UI—all help is appreciated.

### How to Contribute:

1. **Fork** the repository
2. **Create** a feature branch: `git checkout -b feature/your-amazing-feature`
3. **Make** your changes
4. **Commit** with clear messages: `git commit -m 'feat: Add amazing feature'`
5. **Push** to your branch: `git push origin feature/your-amazing-feature`
6. **Open a Pull Request** with a description of your changes

### Guidelines:
- Follow Flutter's [Effective Dart](https://dart.dev/guides/language/effective-dart) guidelines
- Write clear, descriptive commit messages
- Test your changes before submitting
- Update documentation as needed

## ❓ FAQ

**Q: Do I need to set up Firebase?**  
A: Yes, this app requires Firebase. The setup takes about 5 minutes—follow Step 2-5 above.

**Q: Can I use a different backend?**  
A: The app is built specifically for Firebase, but you could fork and modify for another backend.

**Q: Can I contribute?**  
A: Absolutely! All contributions are welcome. See the Contributing section.

**Q: What payment methods are supported?**  
A: Currently Cash on Delivery. Payment gateway integration is planned for future versions.

**Q: Does it work offline?**  
A: Basic features (like browsing cached products) work offline, but purchases and real-time data require internet.

**Q: Where do I report bugs?**  
A: Use [GitHub Issues](https://github.com/bano-qabil123/baby_shop_hub/issues) to report bugs or request features.

## 📖 Documentation

- [Flutter Docs](https://flutter.dev/docs)
- [Firebase Docs](https://firebase.google.com/docs)
- [Provider State Management](https://pub.dev/packages/provider)

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2024 BabyShopHub

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, and/or sell copies of the
Software, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
```

## 🙏 Acknowledgments

- **Flutter & Dart Teams**: For the incredible framework and language
- **Firebase**: For robust backend services
- **Open Source Community**: For amazing packages and support
- **Contributors**: Thank you for helping make this project better

## 📞 Support & Contact

- 🐛 **Report Bugs**: [GitHub Issues](https://github.com/bano-qabil123/baby_shop_hub/issues)
- 💬 **Discussions**: [GitHub Discussions](https://github.com/bano-qabil123/baby_shop_hub/discussions)
- 🌟 **Like it?** Star this repository!

---

**Built with ❤️ for an APTECH semester-end project**

#Flutter #Firebase #ECommerce #MobileApp #OpenSource #Dart
