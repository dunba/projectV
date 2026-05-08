# VAST Platform MVP - Complete Setup Guide

## Overview
This is a complete VAST video hosting platform MVP built with React and Firebase.

## What's Already Created
✅ Project initialized with Create React App
✅ Dependencies installed (firebase, react-router-dom, recharts)
✅ Folder structure created
✅ Firebase services (auth, advertisers, campaigns, creatives, vastTags, events)
✅ Utility functions and custom hooks
✅ Layout components (Navbar, Sidebar)
✅ Login and Dashboard pages

## What You Need to Do Next

### 1. Set Up Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create a new project (or use existing)
3. Enable Firebase Authentication:
   - Go to Authentication > Sign-in method
   - Enable "Email/Password"
4. Create Firestore Database:
   - Go to Firestore Database > Create database
   - Start in **production mode**
5. Enable Firebase Storage:
   - Go to Storage > Get started
6. Get your Firebase config:
   - Go to Project Settings > General
   - Scroll to "Your apps" > Web app
   - Copy the firebaseConfig object

### 2. Update Firebase Configuration

Open `/src/services/firebase.js` and replace the placeholder values with your actual Firebase config:

```javascript
const firebaseConfig = {
  apiKey: "YOUR_ACTUAL_API_KEY",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT_ID.appspot.com",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

### 3. Install Firebase CLI

```bash
npm install -g firebase-tools
firebase login
```

### 4. Initialize Firebase in Project

```bash
cd vast-platform
firebase init
```

Select:
- ✅ Firestore
- ✅ Functions
- ✅ Hosting
- ✅ Storage

Follow prompts:
- Use existing project (select your Firebase project)
- Firestore rules: `firestore.rules`
- Functions language: **TypeScript**
- Use ESLint: Yes
- Install dependencies: Yes
- Public directory: `build`
- Single-page app: **Yes**
- Storage rules: `storage.rules`

### 5. Create Security Rules Files

Create `firestore.rules`:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    function isAuthenticated() {
      return request.auth != null;
    }
    
    match /organizations/{orgId}/{document=**} {
      allow read, write: if isAuthenticated();
    }
  }
}
```

Create `storage.rules`:
```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /organizations/{orgId}/creatives/{creativeId}/{filename} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && 
                      request.resource.contentType.matches('video/.*');
    }
  }
}
```

### 6. Initialize Firestore with Default Organization

Create a file `init-firestore.js` in the project root:

```javascript
const admin = require('firebase-admin');
const serviceAccount = require('./path-to-your-service-account-key.json');

admin.initializeApp({
  credential: admin.credential.cert(serviceAccount)
});

const db = admin.firestore();

async function initializeOrganization() {
  await db.collection('organizations').doc('org-default').set({
    name: 'Default Organization',
    createdAt: admin.firestore.FieldValue.serverTimestamp(),
    updatedAt: admin.firestore.FieldValue.serverTimestamp()
  });
  console.log('Default organization created!');
}

initializeOrganization().then(() => process.exit());
```

Download service account key from Firebase Console > Project Settings > Service Accounts
Run: `node init-firestore.js`

### 7. Remaining React Components to Create

I'll provide abbreviated versions due to space. You can expand these as needed.

See the attached files in the next messages for:
- Advertiser components
- Campaign components
- Creative components
- VAST Tag components
- Report components
- Firebase Cloud Functions

### 8. Run Locally

```bash
# Start React dev server
npm start

# In another terminal, start Firebase emulators (optional but recommended)
firebase emulators:start
```

### 9. Deploy to Firebase

```bash
# Build React app
npm run build

# Deploy everything
firebase deploy

# Or deploy individually
firebase deploy --only firestore:rules
firebase deploy --only storage:rules
firebase deploy --only functions
firebase deploy --only hosting
```

## Project Structure Summary

```
vast-platform/
├── src/
│   ├── components/       # React components
│   ├── pages/           # Page components
│   ├── services/        # Firebase service modules ✅
│   ├── hooks/           # Custom React hooks ✅
│   └── utils/           # Helper functions ✅
├── functions/           # Cloud Functions (to be created)
├── public/              # Static files
├── firestore.rules      # Firestore security rules
├── storage.rules        # Storage security rules
└── firebase.json        # Firebase config

