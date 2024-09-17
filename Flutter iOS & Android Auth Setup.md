### Flutter iOS and Android Auth Setup 
---

# Authentication Setup Guide

This guide outlines the steps to implement **Google Sign-In** for Android and **Sign in with Apple** for iOS using Firebase Authentication in your Flutter mobile application.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Google Sign-In for Android](#google-sign-in-for-android)
   - [1. Firebase Console Configuration](#1-firebase-console-configuration)
   - [2. Android Project Configuration](#2-android-project-configuration)
   - [3. Flutter Dependencies](#3-flutter-dependencies)
3. [Sign in with Apple for iOS](#sign-in-with-apple-for-ios)
   - [1. Firebase Console Configuration](#1-firebase-console-configuration-1)
   - [2. Apple Developer Account Configuration](#2-apple-developer-account-configuration)
   - [3. iOS Project Configuration](#3-ios-project-configuration)
   - [4. Flutter Dependencies](#4-flutter-dependencies-1)
4. [Implementing Authentication Logic](#implementing-authentication-logic)
5. [Testing and Troubleshooting](#testing-and-troubleshooting)
6. [Project Structure Overview](#project-structure-overview)

---

## Prerequisites

- **Flutter SDK** installed and configured.
- A **Firebase** project set up with both Android and iOS apps registered.
- **Apple Developer Account** (required for iOS Sign in with Apple).
- Basic understanding of Flutter and Firebase integration.

---

## Google Sign-In for Android

### 1. Firebase Console Configuration

1. **Enable Google Sign-In:**
   - Navigate to the [Firebase Console](https://console.firebase.google.com/).
   - Select your project.
   - Go to **Authentication** > **Sign-in method**.
   - Enable **Google** as a sign-in provider and configure required fields.

2. **Add Android App to Firebase:**
   - If not already added, register your Android app in Firebase.
   - Provide the **package name** and add the **SHA-1** fingerprint:
     - Obtain SHA-1 using:
       ```bash
       keytool -list -v -alias androiddebugkey -keystore ~/.android/debug.keystore
       ```
     - Default password is `android`.

3. **Download `google-services.json`:**
   - After registering, download the `google-services.json` file.
   - Place it in the `android/app/` directory of your Flutter project.

### 2. Android Project Configuration

1. **Update `android/build.gradle`:**
   - Add the Google Services plugin.
     ```gradle
     dependencies {
         classpath 'com.google.gms:google-services:4.3.10' // Check for latest version
     }
     ```

2. **Update `android/app/build.gradle`:**
   - Apply the Google Services plugin at the bottom.
     ```gradle
     apply plugin: 'com.google.gms.google-services'
     ```

3. **Ensure Proper Dependencies:**
   - Verify that `firebase_auth` and `google_sign_in` are included in your `pubspec.yaml`.

### 3. Flutter Dependencies

Add the following dependencies to your `pubspec.yaml`:

```yaml
dependencies:
  firebase_core: ^3.4.1
  firebase_auth: ^5.2.1
  google_sign_in: ^5.4.0
  # ... other dependencies
```

Run:

```bash
flutter pub get
```

---

## Sign in with Apple for iOS

### 1. Firebase Console Configuration

1. **Enable Apple Sign-In:**
   - In the [Firebase Console](https://console.firebase.google.com/), navigate to **Authentication** > **Sign-in method**.
   - Enable **Apple** as a sign-in provider and save.

### 2. Apple Developer Account Configuration

1. **Sign in to Apple Developer:**
   - Visit the [Apple Developer](https://developer.apple.com/) website and sign in with your Apple ID.

2. **Configure App Identifier:**
   - Go to **Certificates, Identifiers & Profiles**.
   - Select **Identifiers** > **App IDs**.
   - Choose your app or create a new one.
   - Enable the **Sign In with Apple** capability.

3. **Create a Services ID (Optional for Web):**
   - If needed, create a **Services ID** for advanced configurations.
   - Enable **Sign In with Apple** for the Services ID and configure return URLs.

### 3. iOS Project Configuration

1. **Open Project in Xcode:**
   - Navigate to the `ios/` directory in your Flutter project.
   - Run `pod install` to ensure CocoaPods dependencies are installed.
   - Open the project in Xcode:
     ```bash
     open Runner.xcworkspace
     ```

2. **Enable Sign In with Apple Capability:**
   - Select the **Runner** project in Xcode.
   - Go to **Signing & Capabilities**.
   - Click **+ Capability** and add **Sign In with Apple**.

3. **Update `Info.plist`:**
   - Ensure `CFBundleURLTypes` includes the reversed client ID from `GoogleService-Info.plist`.
   - Add any necessary usage descriptions, such as `NSUserTrackingUsageDescription`.

### 4. Flutter Dependencies

Add the following dependencies to your `pubspec.yaml`:

```yaml
dependencies:
  sign_in_with_apple: ^4.2.0
  # ... other dependencies
```

Run:

```bash
flutter pub get
```

---

## Implementing Authentication Logic

1. **Authentication Wrapper:**
   - Use a `StreamBuilder` to listen to Firebase Authentication state changes.
   - Display the `LoginPage` for unauthenticated users and the main app for authenticated users.

2. **Login Page:**
   - Display **"Login with Google"** button for Android.
   - Display **"Sign in with Apple"** button for iOS.
   - Implement platform-specific sign-in logic using `google_sign_in` and `sign_in_with_apple` packages.

3. **Sign-Out Functionality:**
   - Provide a way for users to sign out, which resets the authentication state and redirects to the `LoginPage`.

*Note: Detailed coding steps are omitted as per the guide's scope. Refer to the implementation section in the project for code specifics.*

---

## Testing and Troubleshooting

1. **Android Testing:**
   - Run the app on an Android device or emulator.
   - Ensure Google Sign-In works seamlessly.

2. **iOS Testing:**
   - Test on a physical iOS device for best results.
   - Verify that Sign in with Apple prompts appear and function correctly.
   - Ensure all capabilities and configurations in Xcode are correctly set.

3. **Common Issues:**
   - **Sign-In Failures:**
     - Verify that `google-services.json` and `GoogleService-Info.plist` are correctly placed.
     - Ensure SHA-1 fingerprints are added in Firebase Console for Android.
     - Check bundle identifiers and capabilities in Apple Developer portal for iOS.
   - **Dependency Conflicts:**
     - Run `flutter pub get` after updating `pubspec.yaml`.
     - Use `flutter pub outdated` to check for outdated packages.

---

## Project Structure Overview

Ensure your project adheres to the following structure for optimal configuration:

```
lib/
├── main.dart
├── pages/
│   ├── home_page.dart
│   ├── cv_page.dart
│   ├── cover_letters_page.dart
│   └── login_page.dart
└── firebase_options.dart
assets/
└── google_logo.png
android/
├── app/
│   └── google-services.json
└── build.gradle
ios/
├── Runner.xcworkspace
├── Runner.xcodeproj
├── Runner/
│   ├── AppDelegate.swift
│   ├── Info.plist
│   └── ... (other iOS files)
└── GoogleService-Info.plist
pubspec.yaml
```

**Key Points:**

- **Assets:**
  - Place `google_logo.png` in the `assets/` directory and reference it in `pubspec.yaml`.

- **Firebase Configuration Files:**
  - `google-services.json` for Android in `android/app/`.
  - `GoogleService-Info.plist` for iOS in `ios/Runner/`.

- **Dependencies:**
  - Ensure all necessary packages are listed in `pubspec.yaml` and properly imported in your Dart files.

---

## Conclusion

By following this guide, you can effectively set up **Google Sign-In** for Android and **Sign in with Apple** for iOS in your Flutter application using Firebase Authentication. This ensures a secure and user-friendly authentication experience across both major mobile platforms.

For detailed implementation and code references, please refer to the respective sections in the project’s source code.

---

*Feel free to customize and expand this guide based on your project's specific needs and updates.*
