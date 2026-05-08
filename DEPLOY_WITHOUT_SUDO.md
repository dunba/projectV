# 🚀 Deploy Without Sudo - NPX Method

**Problem:** Can't install Firebase CLI globally due to permissions

**Solution:** Use `npx` or npm scripts - no global installation needed!

---

## ✅ **Method 1: Use NPM Scripts (Easiest!)**

I've added Firebase commands to your `package.json`. Now you can deploy with simple npm commands:

### **Step 1: Login to Firebase**

```bash
npm run firebase:login
```

Browser will open → Sign in with Google → Done!

### **Step 2: Initialize Firebase**

```bash
npm run firebase:init
```

**Selections:**
- What to setup? **Functions, Hosting, Firestore, Storage** (use spacebar to select)
- Use existing project? **Yes** → Select your project
- Everything else? **Press Enter** to accept defaults

### **Step 3: Deploy Functions**

```bash
npm run firebase:deploy:functions
```

This will:
- Install function dependencies
- Build TypeScript
- Deploy to Firebase
- Show your production URLs

### **Step 4: Update Function URL**

After deployment, copy the function URL from output:
```
Function URL (vast): https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net/vast
```

Create `.env` file:
```bash
echo "REACT_APP_FUNCTIONS_URL=https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net" > .env
```

Replace `YOUR_PROJECT_ID` with your actual project ID!

### **Step 5: Deploy Hosting**

```bash
npm run firebase:deploy:hosting
```

---

## 🎉 **Done!**

**Your app:** `https://YOUR_PROJECT_ID.web.app`

**VAST URL:** `https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net/vast?tagId=XXX`

---

## 📋 **All NPM Scripts Available**

| Command | What It Does |
|---------|-------------|
| `npm run firebase:login` | Login to Firebase |
| `npm run firebase:init` | Initialize Firebase project |
| `npm run firebase:deploy:functions` | Deploy only Cloud Functions |
| `npm run firebase:deploy:hosting` | Deploy only React app |
| `npm run firebase:deploy` | Deploy everything |
| `npm run firebase:logs` | View function logs |

---

## ✅ **Method 2: Direct NPX Commands**

If you prefer typing commands directly:

```bash
# Login
npx firebase-tools login

# Initialize
npx firebase-tools init

# Deploy functions
cd functions && npm install && npm run build && cd ..
npx firebase-tools deploy --only functions

# Deploy hosting
npm run build
npx firebase-tools deploy --only hosting

# View logs
npx firebase-tools functions:log
```

---

## ⚡ **Quick Deploy - Copy & Paste**

```bash
# 1. Login (one-time)
npm run firebase:login

# 2. Initialize (one-time)
npm run firebase:init

# 3. Deploy functions
npm run firebase:deploy:functions

# 4. Get function URL from output, create .env
echo "REACT_APP_FUNCTIONS_URL=https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net" > .env

# 5. Deploy hosting
npm run firebase:deploy:hosting
```

**Time:** ~10 minutes

---

## 🔧 **Why This Works**

- **npx** downloads and runs Firebase CLI temporarily
- No global installation needed
- No sudo/permissions required
- Works exactly the same as installed version
- First run downloads, subsequent runs are cached

---

## 💡 **Pro Tips**

### **Tip 1: Deploy Both at Once**

```bash
npm run firebase:deploy
```

This deploys functions AND hosting together.

### **Tip 2: Watch Logs in Real-Time**

```bash
npm run firebase:logs
```

See what's happening with your functions.

### **Tip 3: Check What's Deployed**

```bash
npx firebase-tools projects:list
npx firebase-tools functions:list
```

### **Tip 4: Deploy Only Changed Function**

```bash
npx firebase-tools deploy --only functions:vast
```

---

## 🐛 **Troubleshooting**

### **NPX is slow first time**

**Why:** It's downloading Firebase CLI

**Solution:** Wait ~30 seconds. Subsequent runs are instant.

### **"Cannot find module"**

```bash
cd functions
npm install
npm run build
cd ..
npm run firebase:deploy:functions
```

### **"Not logged in"**

```bash
npm run firebase:login
```

### **Function URL still shows localhost**

Did you create the `.env` file? Check:

```bash
cat .env
```

Should show:
```
REACT_APP_FUNCTIONS_URL=https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net
```

If not, create it:
```bash
echo "REACT_APP_FUNCTIONS_URL=https://us-central1-YOUR_ACTUAL_PROJECT_ID.cloudfunctions.net" > .env
```

Then rebuild:
```bash
npm run firebase:deploy:hosting
```

---

## 🎯 **Verification**

### **1. Check Functions Deployed**

```bash
npx firebase-tools functions:list
```

Should show: `vast`, `track`, `click`

### **2. Test VAST URL**

Open in browser:
```
https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net/vast?tagId=test
```

Should return XML or error message (not 404).

### **3. Check App Deployed**

Open:
```
https://YOUR_PROJECT_ID.web.app
```

Should show your login page.

### **4. Test Full Flow**

1. Login with Google
2. Create advertiser
3. Create campaign
4. Upload video
5. Create VAST tag
6. Copy URL - should be production URL now!

---

## 📊 **What Gets Deployed**

### **Cloud Functions**
- `vast` - Serves VAST XML
- `track` - Records events
- `click` - Handles click tracking

### **React App**
- All pages
- All components
- All assets
- Served from Firebase CDN

### **Configuration**
- Firestore rules
- Storage rules
- Hosting config

---

## 💰 **Cost**

**Free Tier:**
- 2M function invocations/month
- 10GB hosting
- 1GB downloads

**Your Usage (MVP):**
- Likely stays FREE
- Only pay if you exceed free tier

---

## 🚀 **Ready to Deploy?**

Just run:

```bash
npm run firebase:login
npm run firebase:init
npm run firebase:deploy:functions
```

Then create `.env` with your function URL and run:

```bash
npm run firebase:deploy:hosting
```

**That's it!** 🎉

---

## 📖 **Need More Help?**

- **Detailed guide:** See `DEPLOY_FUNCTIONS.md`
- **Quick reference:** See `DEPLOY_QUICK.md`
- **Firebase docs:** https://firebase.google.com/docs

---

## ✨ **Summary**

**Problem:** Can't install Firebase CLI globally

**Solution:** Use npm scripts with npx

**Commands:**
1. `npm run firebase:login`
2. `npm run firebase:init`
3. `npm run firebase:deploy:functions`
4. Create `.env` with function URL
5. `npm run firebase:deploy:hosting`

**Result:** Production VAST platform! 🌍

---

**No sudo required!** ✅
