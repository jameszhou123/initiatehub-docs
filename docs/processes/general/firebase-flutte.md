# Firebase Setup Guide for Flutter Projects

## 1. Create New Project

```bash
flutter create --org com.initiatehub --platforms android,ios,web encyclopedia
```

**Tip:** Always use `--org` and `--platforms` when creating a new project.

## 2. Firebase Configuration

1. Run `flutterfire configure` to create a new project on Firebase.
2. Add Firebase plugins:
   ```bash
   flutter pub add firebase_plugin_name1 firebase_plugin_name2 ...
   ```
3. Run `flutterfire configure` again after adding all plugins.

## 3. Android Setup

### Update app/build.gradle

Add these dependencies:

```groovy
dependencies {
    // Import the Firebase BoM
    implementation platform('com.google.firebase:firebase-bom:33.2.0')

    // Add the dependencies for Firebase products
    implementation 'com.google.firebase:firebase-auth'
    implementation 'com.google.firebase:firebase-firestore'
    implementation 'com.google.firebase:firebase-storage'
    implementation 'com.google.firebase:firebase-analytics'

    // For Google Sign-In
    implementation 'com.google.android.gms:play-services-auth:20.7.0'

    // Add other Firebase products as needed
}
```

Sync the gradle in Android Studio after adding dependencies.

## 4. Firebase Console Setup

1. Enable password signin and Google sign-in providers.
2. Add SHA1 and SHA256 keys for Google Sign-In.

### Generate Release Keystore

In the android folder, run:

```bash
keytool -genkey -v -keystore encyclopedia-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias encyclopedia
```

### Configure Release Keystore

In app/build.gradle:

```groovy
android {
    ...
    signingConfigs {
        release {
            keyAlias 'com.initiatehub.encyclopedia'
            keyPassword 'your-key-password'
            storeFile file('encyclopedia-release-key.jks')
            storePassword 'your-keystore-password'
        }
    }
    buildTypes {
        release {
            signingConfig signingConfigs.release
            minifyEnabled false
            shrinkResources false
        }
    }
}
```

### Get Signing Report

Run in the android folder:

```bash
./gradlew signingReport
```

Add both debug and release SHA1 and SHA256 to Firebase console project settings.

## 5. Final Steps

1. Run `flutterfire configure` again to pull all Firebase config changes.
2. Copy all auth feature code to the new project.

**Note:** If prompted to upgrade Kotlin version during gradle sync, follow the system instructions.
