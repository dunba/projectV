# Quick Start Guide

Get the VAST Platform running in 10 minutes.

## Prerequisites

- Node.js 18+ installed
- Firebase account
- 10 minutes of your time

## Step-by-Step Setup

### 1. Firebase Project (3 minutes)

1. Go to https://console.firebase.google.com/
2. Click "Create a project"
3. Name it (e.g., "vast-platform-demo")
4. Disable Google Analytics (optional for MVP)
5. Click "Create project"

### 2. Enable Services (2 minutes)

**Authentication:**
- Click "Authentication" in sidebar
- Click "Get started"
- Click "Sign-in method" tab
- Click "Email/Password"
- Enable the toggle
- Click "Save"

**Firestore:**
- Click "Firestore Database" in sidebar
- Click "Create database"
- Select "Start in production mode"
- Choose location (e.g., us-central)
- Click "Enable"

**Storage:**
- Click "Storage" in sidebar
- Click "Get started"
- Click "Next" (use production rules)
- Select same location as Firestore
- Click "Done"

### 3. Get Firebase Config (1 minute)

1. Click gear icon ⚙️ > "Project settings"
2. Scroll to "Your apps" section
3. Click the web icon `</>`
4. App nickname: "VAST Platform"
5. Don't check "Firebase Hosting"
6. Click "Register app"
7. **Copy the firebaseConfig object**

### 4. Configure the App (1 minute)

Open `vast-platform/src/services/firebase.js` in a text editor.

Replace this:
```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  // ...
};
```

With your actual config from step 3.

### 5. Install & Run (3 minutes)

```bash
cd vast-platform

# Install dependencies
npm install

# Start the app
npm start
```

Browser will open to `http://localhost:3000`

### 6. Create Account

1. Click "Register"
2. Enter email: `admin@example.com`
3. Enter password: (anything, at least 6 chars)
4. Click "Register"
5. You're in! 🎉

### 7. Create Your First VAST Tag

**Create Advertiser:**
1. Click "Advertisers" in sidebar
2. Click "+ New Advertiser"
3. Name: "Demo Advertiser"
4. Click "Create Advertiser"

**Create Campaign:**
1. Click "Campaigns" in sidebar
2. Click "+ New Campaign"
3. Name: "Demo Campaign"
4. Select "Demo Advertiser"
5. Click "Create Campaign"

**Upload Creative:**
1. Click "Creatives" in sidebar
2. Click "+ Upload Creative"
3. Name: "Demo Video"
4. Select "Demo Campaign"
5. Click-through URL: `https://example.com`
6. Choose a video file (MP4, max 100MB)
7. Click "Upload Creative"
8. Wait for upload to complete

**Create VAST Tag:**
1. Click "VAST Tags" in sidebar
2. Click "+ New VAST Tag"
3. Name: "Demo Tag"
4. Select "Demo Campaign"
5. Select "Demo Video"
6. Click "Create VAST Tag"
7. Click "Copy URL"

### 8. Test Your VAST Tag

**Option A: Browser**
- Paste the VAST URL in your browser
- You should see XML

**Option B: VAST Validator**
1. Go to https://googleads.github.io/googleads-ima-html5/vsi/
2. Paste your VAST URL
3. Click "Test Ad"
4. Video should play!

**Option C: IAB Validator**
1. Go to https://vastvalidator.iabtechlab.com/
2. Paste your VAST URL
3. Click "Validate"
4. Should show "Valid VAST"

### 9. View Analytics

1. Click "Reports" in sidebar
2. You should see impression and start events
3. Try different filters
4. View charts

---

## What Just Happened?

You built a complete video ad serving platform that:
- ✅ Hosts video ads in the cloud
- ✅ Generates IAB-compliant VAST XML
- ✅ Tracks impressions, views, and clicks
- ✅ Shows real-time analytics
- ✅ Works with any VAST-compatible player

## Next Steps

### Deploy to Production

See [DEPLOYMENT_CHECKLIST.md](./DEPLOYMENT_CHECKLIST.md) for step-by-step deployment.

### Test with Real Video Players

Try your VAST URL in:
- JW Player
- Video.js with IMA plugin
- Google IMA SDK
- Any VAST 4.0 compatible player

### Customize

- Edit components in `src/components/`
- Modify Firebase services in `src/services/`
- Update Cloud Functions in `functions/src/`
- Style with Tailwind classes

### Add Features

Future enhancements:
- Multi-tenancy
- User roles and permissions
- Video transcoding
- Dynamic creative optimization
- A/B testing
- Advanced analytics
- Fraud detection

## Troubleshooting

**"Firebase not configured"**
- Check `src/services/firebase.js` has correct config

**Video upload fails**
- File must be < 100MB
- File must be video/mp4, .mov, or .avi
- Check Storage is enabled in Firebase

**VAST tag returns 404**
- Cloud Functions not deployed yet
- This is OK for local development
- Deploy functions: `firebase deploy --only functions`

**Events not showing in Reports**
- Events only appear after VAST tag is viewed
- Test tag in VAST validator first
- Check browser console for errors

## Getting Help

- 📖 Full docs: [README.md](./README.md)
- 🚀 Deployment: [DEPLOYMENT_CHECKLIST.md](./DEPLOYMENT_CHECKLIST.md)
- 🔧 Setup: [SETUP_GUIDE.md](./SETUP_GUIDE.md)
- 📝 Example VAST: [VAST_EXAMPLE.xml](./VAST_EXAMPLE.xml)

## Need Cloud Functions Working?

For full functionality (VAST serving, event tracking), deploy to Firebase:

```bash
# Install Firebase CLI
npm install -g firebase-tools

# Login
firebase login

# Initialize (follow prompts)
firebase init

# Deploy functions
cd functions && npm install && cd ..
firebase deploy --only functions

# Note the function URLs
# Update src/services/vastTags.js with your URLs
```

---

**Congratulations! 🎉**

You now have a working VAST video ad platform. Start experimenting!
