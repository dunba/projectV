# VAST Video Hosting Platform - MVP

A complete VAST (Video Ad Serving Template) video hosting platform built with React and Firebase.

## Features

- ✅ User Authentication (Firebase Auth)
- ✅ Advertiser Management (CRUD)
- ✅ Campaign Management (CRUD)
- ✅ Creative Management with Video Upload
- ✅ VAST Tag Generation
- ✅ VAST XML Serving (Cloud Functions)
- ✅ Event Tracking (impression, start, quartiles, complete, click)
- ✅ Real-time Analytics Dashboard
- ✅ Reporting with Charts

## Setup Instructions

### 1. Firebase Project Setup

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create a new project
3. Enable: Authentication (Email/Password), Firestore, Storage, Functions

### 2. Configure Firebase

Edit `src/services/firebase.js` with your Firebase config

### 3. Install Dependencies

```bash
npm install
cd functions && npm install && cd ..
```

### 4. Deploy

```bash
firebase login
firebase init
firebase deploy --only firestore:rules,storage:rules
npm run build
firebase deploy
```

### 5. Run Locally

```bash
npm start
```

Visit `http://localhost:3000` and register a new account.

## Usage Flow

1. Create Advertiser
2. Create Campaign
3. Upload Creative (video file)
4. Create VAST Tag
5. Copy VAST URL
6. Test in VAST player/validator
7. View Reports

## VAST URL Format

```
https://YOUR_REGION-YOUR_PROJECT.cloudfunctions.net/vast?tagId=TAG_ID
```

## Cloud Functions

- `/vast` - Serves VAST XML
- `/track` - Tracks events (impression, quartiles, etc.)
- `/click` - Logs clicks and redirects

See SETUP_GUIDE.md and code comments for detailed documentation.

## Tech Stack

React • Firebase • TypeScript • Tailwind CSS • Recharts

---

For detailed setup, deployment, and architecture information, see [SETUP_GUIDE.md](./SETUP_GUIDE.md)
