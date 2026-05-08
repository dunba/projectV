# ⚡ Deploy Functions - Quick Guide

## Your Current Situation

VAST URL shows:
```
http://localhost:5001/YOUR_PROJECT_ID/us-central1/vast?tagId=Fvfys5M171OR8dfOsVS4
```

You need it to be:
```
https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net/vast?tagId=Fvfys5M171OR8dfOsVS4
```

---

## 🚀 Quick Deploy (Copy & Paste)

### Step 1: Install Firebase CLI (One-Time)

```bash
npm install -g firebase-tools
```

If permission error:
```bash
sudo npm install -g firebase-tools
```

### Step 2: Login

```bash
firebase login
```

Browser will open → Sign in with Google → Done!

### Step 3: Initialize

```bash
cd /Users/Dunba.Tehinse/Documents/Project\ V/vast-platform
firebase init
```

**Selections:**
- What to setup? **Functions, Hosting, Firestore, Storage**
- Project? **Use existing project** → Select yours
- Functions language? **TypeScript** ✓
- ESLint? **Yes** ✓
- Install dependencies? **Yes** ✓
- Public directory? **build** ✓
- Single-page app? **Yes** ✓

### Step 4: Deploy Functions

```bash
cd functions
npm install
npm run build
cd ..
firebase deploy --only functions
```

⏱️ Takes 2-3 minutes

### Step 5: Copy Function URLs

From output, copy:
```
Function URL (vast): https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net/vast
```

### Step 6: Update App

Create `.env`:
```bash
echo "REACT_APP_FUNCTIONS_URL=https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net" > .env
```

Replace `YOUR_PROJECT_ID` with your actual project ID!

### Step 7: Deploy App

```bash
npm run build
firebase deploy --only hosting
```

---

## ✅ Done!

**Your app:** `https://YOUR_PROJECT_ID.web.app`

**VAST URL:** `https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net/vast?tagId=XXX`

---

## 🧪 Test It

1. Open your app
2. Go to VAST Tags
3. Copy VAST URL
4. Paste in browser → Should see XML!
5. Test in https://googleads.github.io/googleads-ima-html5/vsi/

---

## 💰 Cost

**Free Tier:**
- 2M function calls/month
- 10GB hosting/month
- 5GB storage

**Your usage (testing):**
- Usually stays FREE! 🎉

---

## 🐛 Quick Fixes

**"Billing account required"**
→ Upgrade to Blaze plan (still mostly free)

**"Firebase CLI not found"**
→ Run: `npm install -g firebase-tools` again

**"Module not found"**
→ Run: `cd functions && npm install && cd ..`

**VAST URL still localhost**
→ Did you create `.env` file in Step 6?

---

## 📋 Full Commands

```bash
# One-time setup
npm install -g firebase-tools
firebase login
firebase init

# Deploy
cd functions && npm install && npm run build && cd ..
firebase deploy --only functions
echo "REACT_APP_FUNCTIONS_URL=https://us-central1-YOUR_ID.cloudfunctions.net" > .env
npm run build
firebase deploy --only hosting
```

---

## 🆘 Problems?

See detailed guide: **DEPLOY_FUNCTIONS.md**

Or run:
```bash
firebase functions:log  # View logs
firebase --help         # Get help
```

---

**Time to Deploy:** ~10 minutes total

**Result:** Production VAST platform! 🚀
