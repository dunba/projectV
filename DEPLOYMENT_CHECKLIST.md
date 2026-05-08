# Deployment Checklist

Complete checklist for deploying the VAST Platform to production.

## Pre-Deployment

### 1. Firebase Project Setup

- [ ] Create Firebase project at https://console.firebase.google.com/
- [ ] Note project ID: `__________________`
- [ ] Enable Blaze plan (pay-as-you-go) for Cloud Functions

### 2. Enable Firebase Services

- [ ] **Authentication**
  - [ ] Go to Authentication > Sign-in method
  - [ ] Enable Email/Password provider
  
- [ ] **Firestore Database**
  - [ ] Go to Firestore Database
  - [ ] Create database in production mode
  - [ ] Select region (e.g., us-central1)
  
- [ ] **Storage**
  - [ ] Go to Storage
  - [ ] Get started
  - [ ] Select same region as Firestore
  
- [ ] **Functions**
  - [ ] Automatically enabled with first deployment

### 3. Get Firebase Configuration

- [ ] Go to Project Settings > General
- [ ] Scroll to "Your apps"
- [ ] Click web icon (</>)
- [ ] Register app (name: "VAST Platform")
- [ ] Copy firebaseConfig object
- [ ] Paste into `src/services/firebase.js`

### 4. Install Firebase CLI

```bash
npm install -g firebase-tools
firebase --version
```

- [ ] Firebase CLI installed
- [ ] Version: `__________________`

### 5. Login and Initialize

```bash
firebase login
cd vast-platform
firebase init
```

Select:
- [ ] Firestore
- [ ] Functions
- [ ] Hosting
- [ ] Storage

- [ ] Use existing project
- [ ] Select your project
- [ ] Firestore rules: `firestore.rules`
- [ ] Firestore indexes: `firestore.indexes.json`
- [ ] Functions language: TypeScript
- [ ] Use ESLint: Yes
- [ ] Install dependencies now: Yes
- [ ] Hosting public directory: `build`
- [ ] Configure as SPA: Yes
- [ ] Storage rules: `storage.rules`

### 6. Create Default Organization

Option A: Firebase Console
- [ ] Go to Firestore Database
- [ ] Click "Start collection"
- [ ] Collection ID: `organizations`
- [ ] Document ID: `org-default`
- [ ] Add fields:
  - `name` (string): "Default Organization"
  - `createdAt` (timestamp): Current time
  - `updatedAt` (timestamp): Current time

Option B: Firebase Admin SDK (create script)
- [ ] Create init script
- [ ] Run script
- [ ] Verify in Firestore console

## Deployment Steps

### 7. Install Dependencies

```bash
# Root dependencies
npm install

# Functions dependencies
cd functions
npm install
cd ..
```

- [ ] Root dependencies installed
- [ ] Functions dependencies installed

### 8. Build Functions

```bash
cd functions
npm run build
cd ..
```

- [ ] Functions TypeScript compiled successfully
- [ ] No TypeScript errors

### 9. Deploy Security Rules

```bash
firebase deploy --only firestore:rules
firebase deploy --only storage:rules
```

- [ ] Firestore rules deployed
- [ ] Storage rules deployed
- [ ] Rules test passed in Firebase console

### 10. Deploy Cloud Functions

```bash
firebase deploy --only functions
```

- [ ] Functions deployed successfully
- [ ] Note function URLs:
  - VAST: `__________________________________________________`
  - Track: `__________________________________________________`
  - Click: `__________________________________________________`

### 11. Update Functions URL in App

Edit `src/services/vastTags.js`:

```javascript
const baseUrl = 'https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net';
```

Or create `.env`:

```
REACT_APP_FUNCTIONS_URL=https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net
```

- [ ] Functions URL updated
- [ ] .env file created (if using)

### 12. Build React App

```bash
npm run build
```

- [ ] Build completed successfully
- [ ] Build folder created
- [ ] No build errors

### 13. Deploy Hosting

```bash
firebase deploy --only hosting
```

- [ ] Hosting deployed
- [ ] Hosting URL: `__________________________________________________`

### 14. Deploy Everything (Alternative)

```bash
npm run build
firebase deploy
```

- [ ] All services deployed together

## Post-Deployment Testing

### 15. Test Authentication

- [ ] Visit hosting URL
- [ ] Register new account
- [ ] Verify email received (if email verification enabled)
- [ ] Login successful
- [ ] Dashboard loads

### 16. Test CRUD Operations

- [ ] Create advertiser
- [ ] Create campaign
- [ ] Upload creative (test video file)
  - [ ] Video uploads to Storage
  - [ ] Creative appears in list
- [ ] Create VAST tag
  - [ ] Tag created successfully
  - [ ] VAST URL generated

### 17. Test VAST XML

- [ ] Copy VAST URL
- [ ] Open in browser
- [ ] XML displays correctly
- [ ] MediaFile URL works
- [ ] Tracking URLs present

### 18. Test in VAST Validator

- [ ] Go to https://googleads.github.io/googleads-ima-html5/vsi/
- [ ] Paste VAST URL
- [ ] Click "Test Ad"
- [ ] Verify:
  - [ ] Ad loads
  - [ ] Video plays
  - [ ] Tracking events fire
  - [ ] Click works

### 19. Test Event Tracking

- [ ] Play video in VAST player
- [ ] Go to Reports page
- [ ] Verify events recorded:
  - [ ] Impression
  - [ ] Start
  - [ ] First Quartile
  - [ ] Midpoint
  - [ ] Third Quartile
  - [ ] Complete
  - [ ] Click (if clicked)

### 20. Test Analytics

- [ ] Dashboard shows correct counts
- [ ] Reports page loads
- [ ] Charts display
- [ ] Filters work
- [ ] Event table populated

## Security & Performance

### 21. Security Rules Testing

- [ ] Test unauthenticated access (should fail)
- [ ] Test authenticated read/write (should work)
- [ ] Test invalid file uploads (should fail)
- [ ] Review security rules in Firebase console

### 22. Performance Optimization

- [ ] Check Lighthouse score
- [ ] Verify lazy loading
- [ ] Check bundle size
- [ ] Verify code splitting

### 23. Monitoring Setup

- [ ] Enable Firebase Performance Monitoring
- [ ] Enable Cloud Functions logging
- [ ] Set up error tracking (optional: Sentry)
- [ ] Configure alerts for errors

## Production Checklist

### 24. Environment Variables

- [ ] Remove any hardcoded credentials
- [ ] Set production environment variables
- [ ] Verify .env is in .gitignore

### 25. Domain Setup (Optional)

- [ ] Add custom domain in Firebase Hosting
- [ ] Verify domain ownership
- [ ] SSL certificate issued
- [ ] DNS configured

### 26. Backup & Recovery

- [ ] Export Firestore data
- [ ] Document recovery procedure
- [ ] Test data import/export

### 27. Documentation

- [ ] Update README with production URLs
- [ ] Document any production-specific config
- [ ] Share access with team
- [ ] Document deployment process

## Common Issues & Solutions

### Functions Deployment Fails

```bash
# Check Node version
node --version  # Should be 18+

# Rebuild functions
cd functions
rm -rf node_modules
npm install
npm run build
cd ..
firebase deploy --only functions
```

### CORS Errors

- Functions already include CORS headers
- If issues persist, check browser console
- Verify function URLs are correct

### Video Upload Fails

- Check Storage rules deployed
- Verify file size < 100MB
- Check file type is video/*
- Verify Firebase Storage enabled

### Events Not Tracking

- Check Cloud Functions logs: `firebase functions:log`
- Verify Firestore rules allow writes
- Test tracking URLs directly in browser
- Check browser network tab

### Build Fails

```bash
# Clear cache
rm -rf node_modules
npm install
npm run build
```

## Monitoring & Maintenance

### 28. Regular Checks

- [ ] Monitor Cloud Functions usage
- [ ] Check Storage usage
- [ ] Review Firestore usage
- [ ] Monitor error logs
- [ ] Review security rules effectiveness

### 29. Cost Management

- [ ] Set up billing alerts
- [ ] Review Firebase usage dashboard
- [ ] Monitor bandwidth usage
- [ ] Implement data archiving if needed

### 30. Updates & Maintenance

- [ ] Keep dependencies updated
- [ ] Monitor Firebase updates
- [ ] Test updates in staging first
- [ ] Maintain changelog

## Success Criteria

- [ ] Users can register and login
- [ ] Users can create advertisers, campaigns, creatives
- [ ] Video upload works end-to-end
- [ ] VAST tags generate valid XML
- [ ] VAST tags work in video players
- [ ] Events are tracked correctly
- [ ] Reports show accurate data
- [ ] No critical errors in logs
- [ ] Performance is acceptable
- [ ] Security rules are properly configured

---

## Deployment Complete! 🎉

**Hosting URL:** `_______________________________________________________`

**Next Steps:**
1. Share URL with team
2. Create first advertiser and campaign
3. Test with real video players
4. Monitor performance and errors
5. Iterate based on feedback

**Support Resources:**
- Firebase Docs: https://firebase.google.com/docs
- VAST Spec: https://www.iab.com/guidelines/vast/
- Project README: See README.md
- Setup Guide: See SETUP_GUIDE.md
