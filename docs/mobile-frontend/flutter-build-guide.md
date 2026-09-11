# Release Build Guide & Size Optimization

This guide provides step-by-step instructions to compile and build release packages for **Android (APK, AAB)**, **iOS (IPA)**, and **Web** with maximum size optimizations and obfuscation.

---

## 🛠️ Step 1: Clean, Resolve Dependencies & Firebase

Before compiling a production release, it is best practice to clean the build caches and regenerate the assets/icons:

```bash
# 1. Clean the build cache (removes temporary files & old caches)
flutter clean

# 2. Fetch package dependencies
flutter pub get

# 3. Generate App Launcher Icons (uses configuration in pubspec.yaml)
dart run flutter_launcher_icons

# 4. Configure Firebase (Generates lib/firebase_options.dart)
dart pub global run flutterfire_cli:flutterfire configure
```

---

## 🤖 Android Build Commands

For Android, we use R8 code shrinking (minification) and resource shrinking, which are configured in `android/app/build.gradle.kts`.

### 1. Build Android App Bundle (AAB) - *Recommended for Google Play*
This creates an `.aab` package. Google Play uses it to automatically generate optimized APKs tailored to each user's device architecture.
```bash
flutter build appbundle --release --obfuscate --split-debug-info=build/app/outputs/symbols
```

### 2. Build Split APKs - *Recommended for Direct Sharing*
By default, `flutter build apk` builds a "fat" APK that contains compiled binaries for all CPU architectures (arm64, arm, x64), making the file size large. 
Use the `--split-per-abi` flag to output separate, lightweight APKs optimized for each CPU architecture:
```bash
flutter build apk --release --split-per-abi --obfuscate --split-debug-info=build/app/outputs/symbols
```
*Outputs will be generated under `build/app/outputs/flutter-apk/`:*
- `app-armeabi-v7a-release.apk` (For older 32-bit ARM devices)
- `app-arm64-v8a-release.apk` (For modern 64-bit ARM devices - most common)
- `app-x86_64-release.apk` (For tablets/emulators using x86 CPUs)

---

## 🍎 iOS Build Commands

For iOS, Xcode automatically strips dead code and optimizes compilation. We use Dart obfuscation to further reduce file size and secure the code.

### 1. Build iOS IPA Archive
```bash
flutter build ipa --release --obfuscate --split-debug-info=build/ios/outputs/symbols
```
*This compiles the archive and outputs the `.ipa` package under `build/ios/ipa/` ready for App Store Connect upload.*

---

## 🌐 Web Build Commands

To compile for web deployment with optimal performance:
```bash
flutter build web --release --web-renderer canvaskit
```

---

## ⚙️ Optimization Flags Explained

| Flag | Description | Size Impact |
| :--- | :--- | :--- |
| `--release` | Compiles Dart code to native machine code with compiler optimizations turned on, and strips debugging tools. | **Extremely High** (Reduces size by 70%+) |
| `--obfuscate` | Obfuscates the Dart code (renames function/class names to short identifiers) to protect intellectual property. | **Medium** (Saves 5-10% size) |
| `--split-debug-info=<path>` | Extracts debug symbols (used for resolving crash stack traces) and outputs them to a separate directory instead of packaging them inside the app. | **High** (Saves 10-15% size) |
| `--split-per-abi` | Splits the fat APK into individual APKs for each CPU architecture. | **Very High** (Saves 60%+ per APK file) |

---

## 📊 Analyzing Build Sizes

To see exactly what files and packages are contributing to your app size, you can run:

```bash
# Analyze Android APK size
flutter build apk --analyze-size

# Analyze Android App Bundle size
flutter build appbundle --analyze-size

# Analyze iOS IPA size
flutter build ipa --analyze-size
```
This launches a browser-based visualization tool (Dart DevTools) showing a detailed breakdown of libraries, assets, and binaries.

---

## 🔑 Production Signing Setup

Before distributing to Google Play Store or Apple App Store, you must sign your release binaries.

### Android Code Signing Setup
1. **Generate a keystore**:
   ```bash
   keytool -genkey -v -keystore ~/upload-keystore.jks -keyalg RSA -keysize 2048 -validity 10000 -alias key
   ```
2. **Configure properties**:
   Create a file named `android/key.properties` (do not commit this file to Git):
   ```properties
   storePassword=<keystore-password>
   keyPassword=<key-password>
   keyAlias=key
   storeFile=<path-to-upload-keystore.jks>
   ```
3. **Configure Gradle**:
   Gradle in `android/app/build.gradle.kts` will automatically use this `key.properties` config when compiling release builds if configured.

### iOS Signing Setup
1. Open the project in Xcode:
   ```bash
   open ios/Runner.xcworkspace
   ```
2. Select the **Runner** project in the left sidebar.
3. Select the **Signing & Capabilities** tab.
4. Check **Automatically manage signing** and select your Apple Developer account Team.
