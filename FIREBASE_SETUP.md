# Firebase Setup for Flutter Project

This Flutter project has been configured with Firebase Authentication, Firestore, and Storage.

## Dependencies Added

The following dependencies have been added to `pubspec.yaml`:

```yaml
dependencies:
  firebase_core: ^3.6.0
  firebase_auth: ^5.3.1
  cloud_firestore: ^5.4.3
  firebase_storage: ^12.3.2
  file_picker: ^8.0.0+1

dev_dependencies:
  flutterfire_cli: ^0.2.0+1
```

## Firebase Configuration

The project is already configured with Firebase options in `lib/firebase_options.dart`. This file contains the configuration for all platforms (Android, iOS, Web, macOS, Windows).

## Firebase Services

### 1. Firebase Authentication
- **File**: `lib/firebase_service.dart`
- **Features**: Sign up, sign in, sign out
- **Methods**:
  - `signUpWithEmailAndPassword()`
  - `signInWithEmailAndPassword()`
  - `signOut()`
  - `currentUser` (getter)
  - `authStateChanges` (stream)

### 2. Cloud Firestore
- **File**: `lib/firebase_service.dart`
- **Features**: CRUD operations for documents
- **Methods**:
  - `addDocument()`
  - `getDocuments()`
  - `getDocument()`
  - `updateDocument()`
  - `deleteDocument()`

### 3. Firebase Storage
- **File**: `lib/firebase_service.dart` and `lib/storage_example.dart`
- **Features**: File upload, download, and deletion
- **Methods**:
  - `uploadFile()`
  - `deleteFile()`
  - `getDownloadUrl()`

## Usage Examples

### Authentication
```dart
// Sign up
final result = await FirebaseService.signUpWithEmailAndPassword(
  email: 'user@example.com',
  password: 'password123',
);

// Sign in
final result = await FirebaseService.signInWithEmailAndPassword(
  email: 'user@example.com',
  password: 'password123',
);

// Sign out
await FirebaseService.signOut();
```

### Firestore
```dart
// Add a document
await FirebaseService.addDocument(
  collection: 'users',
  data: {
    'name': 'John Doe',
    'email': 'john@example.com',
  },
);

// Get documents
Stream<QuerySnapshot> users = FirebaseService.getDocuments('users');
```

### Storage
```dart
// Upload a file
String? downloadUrl = await FirebaseService.uploadFile(
  path: 'uploads/file.jpg',
  file: File('path/to/file.jpg'),
);

// Delete a file
await FirebaseService.deleteFile('uploads/file.jpg');
```

## Running the App

1. Make sure you have Flutter installed and configured
2. Run `flutter pub get` to install dependencies
3. Run `flutter run` to start the app

## Firebase Console Setup

To use these services, you need to:

1. Go to the [Firebase Console](https://console.firebase.google.com/)
2. Create a new project or use an existing one
3. Enable the following services:
   - **Authentication**: Enable Email/Password authentication
   - **Firestore Database**: Create a database in test mode
   - **Storage**: Create a storage bucket

## Security Rules

Make sure to set up proper security rules for Firestore and Storage:

### Firestore Rules
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### Storage Rules
```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

## Project Structure

```
lib/
├── main.dart                 # Main app with Firebase initialization
├── firebase_options.dart     # Firebase configuration
├── firebase_service.dart     # Firebase service methods
└── storage_example.dart      # Storage usage example
```

## Features Implemented

1. **Authentication UI**: Sign up, sign in, and sign out forms
2. **Firestore UI**: Add messages and view them in real-time
3. **Storage UI**: Upload and manage files
4. **Real-time updates**: Messages update automatically using Firestore streams

The app demonstrates all three Firebase services with a user-friendly interface.
