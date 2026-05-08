# Troubleshooting Guide

Common issues and solutions for the VAST Platform.

## Build & Compilation Issues

### ✅ FIXED: Tailwind CSS PostCSS Error

**Error:**
```
Error: It looks like you're trying to use `tailwindcss` directly as a PostCSS plugin.
```

**Solution:**
This has been fixed! The project now uses Tailwind CSS v3.4.1 which is compatible with Create React App.

If you still see this error:
```bash
npm uninstall tailwindcss
npm install -D tailwindcss@3.4.1 postcss@8.4.35 autoprefixer@10.4.17
```

### Module Not Found Errors

**Error:**
```
Module not found: Can't resolve './components/...'
```

**Solution:**
Check that all files exist and imports are correct:
```bash
# Verify file exists
ls src/components/Layout/Navbar.jsx

# Check for typos in import paths
grep -r "from.*Navbar" src/
```

### React Hook Warnings

**Warning:**
```
React Hook useEffect has a missing dependency
```

**Solution:**
Already handled with `// eslint-disable-next-line` comments where appropriate. These warnings don't break the build.

## Firebase Issues

### Firebase Not Configured

**Error:**
```
Firebase: Error (auth/invalid-api-key)
```

**Solution:**
1. Open `src/services/firebase.js`
2. Replace ALL placeholder values with your actual Firebase config
3. Save and reload

**How to get Firebase config:**
1. Go to Firebase Console
2. Project Settings > General
3. Scroll to "Your apps"
4. Copy the `firebaseConfig` object

### Authentication Errors

**Error:**
```
Firebase: Error (auth/email-already-in-use)
```

**Solution:**
This means the email is already registered. Use login instead of register.

**Error:**
```
Firebase: Error (auth/wrong-password)
```

**Solution:**
Check your password. Firebase requires at least 6 characters.

**Error:**
```
Firebase: Error (auth/user-not-found)
```

**Solution:**
User doesn't exist. Register first, then login.

### Firestore Permission Denied

**Error:**
```
FirebaseError: Missing or insufficient permissions
```

**Solution:**
1. Deploy Firestore rules:
```bash
firebase deploy --only firestore:rules
```

2. Verify rules in Firebase Console:
   - Go to Firestore Database > Rules
   - Should allow authenticated users

3. Make sure you're logged in (check auth state in browser console)

### Storage Upload Fails

**Error:**
```
FirebaseError: User does not have permission to access
```

**Solution:**
1. Deploy Storage rules:
```bash
firebase deploy --only storage:rules
```

2. Check file type:
   - Must be video/mp4, video/quicktime, or video/x-msvideo
   - Max size: 100MB

3. Verify Firebase Storage is enabled in console

## Cloud Functions Issues

### Functions Return 404

**Error:**
VAST URL returns 404 Not Found

**Solution:**
Functions aren't deployed yet. This is expected for local development.

To fix:
```bash
cd functions
npm install
npm run build
cd ..
firebase deploy --only functions
```

Then update the function URL in `src/services/vastTags.js` or `.env`

### Function Deployment Fails

**Error:**
```
Error: HTTP Error: 400, Billing account not configured
```

**Solution:**
1. Firebase requires Blaze (pay-as-you-go) plan for Cloud Functions
2. Go to Firebase Console > Upgrade
3. Add payment method (free tier covers most development)

**Error:**
```
Build failed: npm install failed
```

**Solution:**
```bash
cd functions
rm -rf node_modules package-lock.json
npm install
npm run build
cd ..
firebase deploy --only functions
```

### CORS Errors

**Error:**
```
Access to fetch has been blocked by CORS policy
```

**Solution:**
Functions already include CORS headers. If you still see this:
1. Check the function URL is correct
2. Clear browser cache
3. Try incognito/private browsing
4. Check browser console for actual error

## Runtime Issues

### Video Upload Fails

**Symptoms:**
- Upload progress stuck
- Upload returns error
- Video doesn't appear

**Solutions:**

1. **Check file size:**
```javascript
// Max 100MB
if (file.size > 100 * 1024 * 1024) {
  alert('File too large');
}
```

2. **Check file type:**
```javascript
// Must be video/*
const validTypes = ['video/mp4', 'video/quicktime', 'video/x-msvideo'];
if (!validTypes.includes(file.type)) {
  alert('Invalid file type');
}
```

3. **Check Storage rules:**
```bash
firebase deploy --only storage:rules
```

4. **Check browser console for errors**

### Events Not Tracking

**Symptoms:**
- Reports page shows 0 events
- VAST tag works but no data

**Solutions:**

1. **Check Cloud Functions are deployed:**
```bash
firebase functions:log
```

2. **Test tracking endpoint directly:**
```bash
curl "https://YOUR_REGION-YOUR_PROJECT.cloudfunctions.net/track?event=impression&tagId=test&creativeId=test"
```

Should return a 1x1 GIF (binary data)

3. **Check Firestore rules allow writes:**
```bash
firebase deploy --only firestore:rules
```

4. **Verify events collection exists:**
   - Open Firestore console
   - Look for `organizations/org-default/events/`

### Dashboard Shows Wrong Data

**Symptoms:**
- Counts are incorrect
- Old data showing

**Solutions:**

1. **Hard refresh browser:**
   - Chrome/Firefox: Ctrl+Shift+R (Cmd+Shift+R on Mac)
   - Safari: Cmd+Option+R

2. **Check Firestore data directly:**
   - Open Firebase Console
   - Go to Firestore Database
   - Verify data is actually there

3. **Check browser console for errors**

## Development Server Issues

### Port Already in Use

**Error:**
```
Something is already running on port 3000
```

**Solution:**
```bash
# Kill process on port 3000
lsof -ti:3000 | xargs kill -9

# Or use different port
PORT=3001 npm start
```

### Hot Reload Not Working

**Symptoms:**
- Make changes but page doesn't update
- Have to manually refresh

**Solutions:**

1. **Check if server is running:**
```bash
ps aux | grep react-scripts
```

2. **Restart dev server:**
```bash
pkill -f "react-scripts"
npm start
```

3. **Clear cache:**
```bash
rm -rf node_modules/.cache
npm start
```

### Build Fails with Memory Error

**Error:**
```
FATAL ERROR: Reached heap limit Allocation failed - JavaScript heap out of memory
```

**Solution:**
```bash
# Increase Node memory
export NODE_OPTIONS="--max-old-space-size=4096"
npm run build
```

Or add to package.json:
```json
{
  "scripts": {
    "build": "NODE_OPTIONS=--max-old-space-size=4096 react-scripts build"
  }
}
```

## Deployment Issues

### Firebase Init Fails

**Error:**
```
Error: Failed to authenticate
```

**Solution:**
```bash
firebase logout
firebase login
firebase projects:list
```

### Hosting Deploy Shows 404

**Symptoms:**
- Deploy succeeds but site shows 404
- Only index.html works

**Solution:**
Make sure `firebase.json` has rewrites:
```json
{
  "hosting": {
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```

### Functions Deploy Timeout

**Error:**
```
Error: Functions deploy had errors with the following functions: vast, track, click
```

**Solution:**
```bash
# Deploy functions one at a time
firebase deploy --only functions:vast
firebase deploy --only functions:track
firebase deploy --only functions:click
```

## Browser-Specific Issues

### Safari Issues

**Issue:** IndexedDB errors

**Solution:**
Enable "Prevent Cross-Site Tracking" in Safari preferences

### Chrome Issues

**Issue:** CORS errors only in Chrome

**Solution:**
1. Clear Chrome cache
2. Disable Chrome extensions
3. Try incognito mode

### Firefox Issues

**Issue:** Video upload fails

**Solution:**
Check Firefox privacy settings - may block storage access

## Performance Issues

### Slow Video Upload

**Symptoms:**
- Upload takes very long
- Progress bar moves slowly

**Solutions:**

1. **Check video size:**
   - Recommended: < 50MB
   - Maximum: 100MB
   - Larger files = slower upload

2. **Check internet connection**

3. **Check Firebase Storage region:**
   - Storage should be in same region as your users
   - Can't change after creation

### Slow Reports Page

**Symptoms:**
- Reports page takes long to load
- Charts are slow

**Solutions:**

1. **Limit date range:**
   - Use date filters
   - Don't query all-time data

2. **Limit events returned:**
```javascript
// In events.js
const events = await getEvents({ limit: 100 });
```

3. **Add Firestore indexes:**
```bash
firebase deploy --only firestore:indexes
```

## Data Issues

### Organization Not Found

**Error:**
```
Organization 'org-default' not found
```

**Solution:**
Create the default organization in Firestore:
1. Open Firebase Console
2. Go to Firestore Database
3. Create collection: `organizations`
4. Create document with ID: `org-default`
5. Add fields:
   - `name`: "Default Organization"
   - `createdAt`: (timestamp)
   - `updatedAt`: (timestamp)

### Creative Has No Video URL

**Symptoms:**
- Creative created but no video
- Video player shows error

**Solutions:**

1. **Check if upload completed:**
   - Look for `mediaFileUrl` field in Firestore
   - If missing, re-upload

2. **Check Storage bucket:**
   - Go to Firebase Storage
   - Look for `organizations/org-default/creatives/`
   - Video file should be there

3. **Check Storage rules:**
```bash
firebase deploy --only storage:rules
```

## Testing Issues

### VAST Validator Shows Errors

**Error:**
"Invalid VAST: Missing required element"

**Solutions:**

1. **Check creative has all required fields:**
   - mediaFileUrl
   - width, height
   - duration
   - clickThroughUrl

2. **Test VAST URL directly in browser:**
   - Should return XML, not JSON or error

3. **Check Cloud Functions logs:**
```bash
firebase functions:log --only vast
```

### Video Player Won't Load Ad

**Symptoms:**
- VAST URL works in browser
- But player shows no ad

**Solutions:**

1. **Check VAST version compatibility:**
   - Some old players only support VAST 2.0
   - Default is VAST 4.0

2. **Check video format:**
   - Must be progressive MP4
   - H.264 video codec
   - AAC audio codec

3. **Check CORS:**
   - Video URL must allow cross-origin requests
   - Firebase Storage already handles this

4. **Check video URL is accessible:**
   - Copy `mediaFileUrl` from creative
   - Open in browser
   - Should play directly

## Getting More Help

### Enable Debug Logging

**Firebase:**
```javascript
// In firebase.js
import { setLogLevel } from 'firebase/firestore';
setLogLevel('debug');
```

**React:**
```javascript
// In index.jsx
console.log('React app starting...');
```

### Check Logs

**Firebase Functions:**
```bash
firebase functions:log
firebase functions:log --only vast
firebase functions:log --limit 50
```

**Browser Console:**
- Press F12
- Go to Console tab
- Look for errors (red text)

**Network Tab:**
- Press F12
- Go to Network tab
- Reload page
- Check for failed requests (red)

### Verify Environment

```bash
# Check versions
node --version    # Should be 18+
npm --version     # Should be 9+
firebase --version

# Check Firebase project
firebase use
firebase projects:list

# Check dependencies
npm list firebase
npm list react
npm list tailwindcss
```

### Clean Install

If all else fails:
```bash
# Complete clean reinstall
rm -rf node_modules package-lock.json
rm -rf functions/node_modules functions/package-lock.json
npm install
cd functions && npm install && cd ..

# Clear build cache
rm -rf build
rm -rf node_modules/.cache

# Rebuild
npm start
```

## Still Having Issues?

1. **Check documentation:**
   - README.md
   - SETUP_GUIDE.md
   - DEPLOYMENT_CHECKLIST.md

2. **Check Firebase status:**
   - https://status.firebase.google.com/

3. **Search Firebase docs:**
   - https://firebase.google.com/docs

4. **Check browser console for errors**

5. **Check Firebase Console for errors**

6. **Try in different browser**

7. **Try on different machine**

## Quick Diagnostic

Run this to check your setup:

```bash
cd vast-platform

echo "=== Checking Node/npm ==="
node --version
npm --version

echo "=== Checking Firebase ==="
firebase --version
firebase use

echo "=== Checking Files ==="
ls -la src/services/firebase.js
ls -la functions/src/

echo "=== Checking Dependencies ==="
npm list --depth=0 | grep -E "firebase|react|tailwind"

echo "=== Checking Build ==="
npm run build 2>&1 | head -20
```

---

**Most Common Issue:** Firebase not configured in `src/services/firebase.js`

**Second Most Common:** Cloud Functions not deployed (needed for VAST serving)

**Third Most Common:** Default organization not created in Firestore
