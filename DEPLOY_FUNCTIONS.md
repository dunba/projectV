# 🚀 Deploy Cloud Functions to Production

Your VAST tag currently shows:
```
http://localhost:5001/YOUR_PROJECT_ID/us-central1/vast?tagId=Fvfys5M171OR8dfOsVS4
```

Let's get it working on a real domain!

---

## ✅ **What You'll Get**

After deployment, your URL will be:
```
https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net/vast?tagId=Fvfys5M171OR8dfOsVS4
```

This will work from anywhere in the world! 🌍

---

## 🚀 **Quick Deploy (5 Minutes)**

### **Step 1: Install Firebase CLI**

Open your terminal and run:

```bash
# Install Firebase CLI globally
sudo npm install -g firebase-tools

# Or without sudo (recommended):
npm install -g firebase-tools

# Verify installation
firebase --version
```

**Expected output:** Something like `13.0.0`

### **Step 2: Login to Firebase**

```bash
firebase login
```

This will:
1. Open your browser
2. Ask you to sign in with Google
3. Grant permissions to Firebase CLI

### **Step 3: Initialize Firebase Project**

```bash
cd /Users/Dunba.Tehinse/Documents/Project\ V/vast-platform

firebase init
```

**Select these options:**

1. **What do you want to set up?**
   - Select: ☑ Functions, ☑ Hosting, ☑ Firestore, ☑ Storage
   - Use spacebar to select, Enter to continue

2. **Use existing project or create new one?**
   - Select: **Use an existing project**
   - Choose your Firebase project from the list

3. **Firestore Setup:**
   - Firestore rules file: `firestore.rules` (already exists)
   - Firestore indexes: `firestore.indexes.json` (already exists)

4. **Functions Setup:**
   - Language: **TypeScript** (already configured)
   - Use ESLint: **Yes**
   - Install dependencies now: **Yes**

5. **Hosting Setup:**
   - Public directory: `build`
   - Configure as single-page app: **Yes**
   - Set up automatic builds: **No**

6. **Storage Setup:**
   - Storage rules: `storage.rules` (already exists)

### **Step 4: Build Functions**

```bash
cd functions
npm install
npm run build
cd ..
```

### **Step 5: Deploy Cloud Functions**

```bash
firebase deploy --only functions
```

This will:
- Upload your functions to Firebase
- Provide you with the production URLs
- Take about 2-3 minutes

**Expected output:**
```
✔  functions: Finished running predeploy script.
i  functions: preparing codebase default for deployment
i  functions: ensuring required API cloudfunctions.googleapis.com is enabled...
i  functions: ensuring required API cloudbuild.googleapis.com is enabled...
✔  functions: required API cloudfunctions.googleapis.com is enabled
✔  functions: required API cloudbuild.googleapis.com is enabled
i  functions: Loading and analyzing source code for codebase default to determine what to deploy
Serving at port 9005

i  functions: preparing functions directory for uploading...
i  functions: packaged /Users/.../functions (XX MB) for uploading
✔  functions: functions folder uploaded successfully
i  functions: creating Node.js 18 function vast(us-central1)...
i  functions: creating Node.js 18 function track(us-central1)...
i  functions: creating Node.js 18 function click(us-central1)...
✔  functions[vast(us-central1)] Successful create operation.
✔  functions[track(us-central1)] Successful create operation.
✔  functions[click(us-central1)] Successful create operation.

Function URL (vast): https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net/vast
Function URL (track): https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net/track
Function URL (click): https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net/click

✔  Deploy complete!
```

### **Step 6: Update Your App with Production URLs**

Copy the function URLs from the deployment output.

**Option A: Use Environment Variable**

Create `.env` in your project root:

```bash
# In /Users/Dunba.Tehinse/Documents/Project V/vast-platform/
cat > .env << 'EOF'
REACT_APP_FUNCTIONS_URL=https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net
EOF
```

Replace `YOUR_PROJECT_ID` with your actual Firebase project ID.

**Option B: Edit `src/services/vastTags.js`**

Open the file and update line 16:

```javascript
// Before:
const baseUrl = process.env.REACT_APP_FUNCTIONS_URL || 'http://localhost:5001/YOUR_PROJECT_ID/us-central1';

// After:
const baseUrl = process.env.REACT_APP_FUNCTIONS_URL || 'https://us-central1-YOUR_ACTUAL_PROJECT_ID.cloudfunctions.net';
```

### **Step 7: Rebuild and Deploy React App**

```bash
npm run build
firebase deploy --only hosting
```

**Your app will be live at:**
```
https://YOUR_PROJECT_ID.web.app
```

or

```
https://YOUR_PROJECT_ID.firebaseapp.com
```

---

## ✅ **Verification**

### **Test Your VAST URL**

1. Open your app at `https://YOUR_PROJECT_ID.web.app`
2. Go to VAST Tags page
3. Copy the VAST URL
4. It should now look like:
   ```
   https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net/vast?tagId=Fvfys5M171OR8dfOsVS4
   ```
5. Open in browser - should return XML!

### **Test in VAST Validator**

1. Go to https://googleads.github.io/googleads-ima-html5/vsi/
2. Paste your VAST URL
3. Click "Test Ad"
4. Video should play! 🎉

---

## 🎯 **Quick Reference Commands**

```bash
# One-time setup
npm install -g firebase-tools
firebase login
cd vast-platform
firebase init

# Deploy functions only
cd functions && npm install && npm run build && cd ..
firebase deploy --only functions

# Deploy everything (functions + hosting)
npm run build
firebase deploy

# View logs
firebase functions:log

# Check what's deployed
firebase functions:list
```

---

## 📊 **Your URLs After Deployment**

| Service | URL |
|---------|-----|
| **Web App** | `https://YOUR_PROJECT_ID.web.app` |
| **VAST XML** | `https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net/vast?tagId=X` |
| **Track Events** | `https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net/track?event=X&tagId=Y&creativeId=Z` |
| **Click Redirect** | `https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net/click?tagId=X&creativeId=Y&redirect=URL` |

All served over HTTPS automatically! 🔒

---

## 🔧 **Troubleshooting**

### **Issue: "Billing account required"**

Cloud Functions require the **Blaze (Pay as you go)** plan.

**Solution:**
1. Go to Firebase Console
2. Click "Upgrade" in left sidebar
3. Add credit card (you won't be charged much)
4. Firebase free tier covers most development usage

**Cost estimate:**
- First 2M function calls/month: FREE
- After that: $0.40 per million calls
- For testing: Usually stays free

### **Issue: "Firebase CLI not found"**

**Solution:**
```bash
# Try without sudo first
npm install -g firebase-tools

# If permission error, use sudo
sudo npm install -g firebase-tools

# Verify
firebase --version
```

### **Issue: "Functions deploy failed"**

**Check Node version:**
```bash
node --version
```

Should be 18 or higher. If not:
```bash
# Install Node 18
brew install node@18  # Mac
# or download from nodejs.org
```

**Rebuild functions:**
```bash
cd functions
rm -rf node_modules lib
npm install
npm run build
cd ..
firebase deploy --only functions
```

### **Issue: "Cannot find module"**

**Solution:**
```bash
cd functions
npm install firebase-admin firebase-functions
npm run build
cd ..
firebase deploy --only functions
```

### **Issue: VAST URL still shows localhost**

**Solution:**
Update `src/services/vastTags.js`:

```javascript
export const generateVastUrl = (tagId) => {
  // Replace with your actual Cloud Functions URL
  const baseUrl = 'https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net';
  return `${baseUrl}/vast?tagId=${tagId}`;
};
```

Then rebuild:
```bash
npm run build
firebase deploy --only hosting
```

---

## 💡 **Pro Tips**

### **Tip 1: Deploy Only What Changed**

```bash
# Only functions
firebase deploy --only functions

# Only hosting
firebase deploy --only hosting

# Only specific function
firebase deploy --only functions:vast

# Multiple services
firebase deploy --only functions,hosting
```

### **Tip 2: Test Functions Locally First**

```bash
# Start emulator
firebase emulators:start --only functions

# Test with curl
curl "http://localhost:5001/YOUR_PROJECT_ID/us-central1/vast?tagId=test"
```

### **Tip 3: View Live Logs**

```bash
# All functions
firebase functions:log

# Specific function
firebase functions:log --only vast

# Follow logs in real-time
firebase functions:log --limit 50
```

### **Tip 4: Environment Variables**

Set environment variables for functions:

```bash
firebase functions:config:set app.mode="production"
firebase deploy --only functions
```

Access in functions:
```typescript
const mode = functions.config().app.mode;
```

---

## 🚀 **Complete Deployment Script**

Save this as `deploy.sh` for one-command deployment:

```bash
#!/bin/bash

echo "🚀 Deploying VAST Platform..."

# Build functions
echo "📦 Building Cloud Functions..."
cd functions
npm install
npm run build
cd ..

# Build React app
echo "⚛️  Building React app..."
npm run build

# Deploy to Firebase
echo "☁️  Deploying to Firebase..."
firebase deploy

echo "✅ Deployment complete!"
echo "🌍 Your app is live at: https://YOUR_PROJECT_ID.web.app"
```

Make it executable:
```bash
chmod +x deploy.sh
./deploy.sh
```

---

## 🎉 **Success!**

After following these steps:

1. ✅ Your functions are deployed to Firebase
2. ✅ Your VAST tags work from anywhere
3. ✅ URLs are production-ready with HTTPS
4. ✅ App is hosted on Firebase Hosting
5. ✅ Everything works globally

**Your VAST URL:**
```
https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net/vast?tagId=Fvfys5M171OR8dfOsVS4
```

**Test it:**
- Open in browser (should see XML)
- Use in VAST validator
- Use in video player

---

## 📖 **Next Steps**

1. **Update Documentation** - Update any docs with production URLs
2. **Test Thoroughly** - Test all VAST tags work
3. **Monitor Logs** - `firebase functions:log` to watch activity
4. **Set Up Custom Domain** (Optional) - See Firebase Hosting docs
5. **Enable Monitoring** - Firebase Console → Functions → Usage tab

---

## 🆘 **Need Help?**

**Firebase Documentation:**
- Functions: https://firebase.google.com/docs/functions
- Hosting: https://firebase.google.com/docs/hosting
- CLI: https://firebase.google.com/docs/cli

**Check Status:**
```bash
firebase projects:list
firebase functions:list
firebase hosting:sites:list
```

**Rollback if Needed:**
```bash
# List deployments
firebase hosting:rollback

# Roll back functions
firebase functions:delete functionName
```

---

## ✨ **Summary**

**Steps to Deploy:**

1. `npm install -g firebase-tools`
2. `firebase login`
3. `firebase init`
4. `cd functions && npm install && npm run build && cd ..`
5. `firebase deploy --only functions`
6. Update URLs in `src/services/vastTags.js`
7. `npm run build && firebase deploy --only hosting`

**Total Time:** ~10 minutes

**Result:** Production-ready VAST platform with global URLs! 🌍

---

**Ready to deploy?** Start with Step 1 and work through the checklist!
