# Terminal Commands Reference

Complete list of all terminal commands needed to set up and deploy the VAST Platform.

## Initial Setup (Already Done)

These commands have already been run:

```bash
# Create React app
npx create-react-app vast-platform

# Navigate to project
cd vast-platform

# Install dependencies
npm install firebase react-router-dom recharts

# Install Tailwind CSS
npm install -D tailwindcss postcss autoprefixer

# Create folder structure (already done)
mkdir -p src/components/{Layout,Advertisers,Campaigns,Creatives,VastTags,Reports}
mkdir -p src/pages src/services src/hooks src/utils functions/src
```

## What You Need to Do

### 1. Configure Firebase

```bash
# Edit this file with your Firebase config:
nano src/services/firebase.js

# Or use your preferred editor:
# code src/services/firebase.js
# vim src/services/firebase.js
```

### 2. Install Firebase CLI

```bash
# Install globally
npm install -g firebase-tools

# Verify installation
firebase --version

# Login to Firebase
firebase login
```

### 3. Initialize Firebase Project

```bash
# Navigate to project root
cd /Users/Dunba.Tehinse/Documents/Project\ V/vast-platform

# Initialize Firebase
firebase init

# During init, select:
# - Firestore
# - Functions  
# - Hosting
# - Storage

# Follow prompts:
# - Use existing project
# - Firestore rules: firestore.rules
# - Firestore indexes: firestore.indexes.json
# - Functions language: TypeScript
# - Use ESLint: Yes
# - Install dependencies: Yes
# - Public directory: build
# - Configure as SPA: Yes
# - Storage rules: storage.rules

# Link to your Firebase project
firebase use --add
# Select your project from the list
```

### 4. Install Function Dependencies

```bash
# Navigate to functions directory
cd functions

# Install dependencies
npm install

# Build TypeScript
npm run build

# Go back to root
cd ..
```

### 5. Deploy Security Rules

```bash
# Deploy Firestore rules
firebase deploy --only firestore:rules

# Deploy Firestore indexes
firebase deploy --only firestore:indexes

# Deploy Storage rules
firebase deploy --only storage:rules
```

### 6. Deploy Cloud Functions

```bash
# Build and deploy functions
firebase deploy --only functions

# Note the URLs output - you'll need these
# They look like:
# https://us-central1-your-project-id.cloudfunctions.net/vast
# https://us-central1-your-project-id.cloudfunctions.net/track
# https://us-central1-your-project-id.cloudfunctions.net/click
```

### 7. Update Functions URL in Code

```bash
# Create .env file
cat > .env << 'EOF'
REACT_APP_FUNCTIONS_URL=https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net
EOF

# Or edit src/services/vastTags.js manually
nano src/services/vastTags.js
```

### 8. Run Locally

```bash
# Start development server
npm start

# In another terminal, run Firebase emulators (optional)
firebase emulators:start

# Or run specific emulators
firebase emulators:start --only firestore,storage
```

### 9. Build for Production

```bash
# Build React app
npm run build

# The build folder is ready to deploy
```

### 10. Deploy to Firebase Hosting

```bash
# Deploy hosting only
firebase deploy --only hosting

# Or deploy everything at once
firebase deploy

# Or deploy specific services
firebase deploy --only functions,hosting
```

## Common Development Commands

### React Development

```bash
# Start dev server
npm start

# Build production bundle
npm run build

# Run tests (if configured)
npm test

# Eject from Create React App (not recommended)
# npm run eject
```

### Firebase Functions

```bash
# Navigate to functions directory
cd functions

# Install dependencies
npm install

# Build TypeScript
npm run build

# Serve functions locally
npm run serve

# View function logs
firebase functions:log

# View logs for specific function
firebase functions:log --only vast

# Go back to root
cd ..
```

### Firebase Operations

```bash
# Check current project
firebase use

# Switch to different project
firebase use project-id

# List all projects
firebase projects:list

# Deploy everything
firebase deploy

# Deploy specific service
firebase deploy --only functions
firebase deploy --only hosting
firebase deploy --only firestore:rules
firebase deploy --only storage:rules

# Open project in browser
firebase open

# View Firestore data
firebase open firestore

# View Firebase console
firebase open console
```

### Viewing Logs

```bash
# View all function logs
firebase functions:log

# View recent logs
firebase functions:log --limit 50

# View logs for specific function
firebase functions:log --only vast

# Follow logs in real-time (doesn't work on all systems)
# firebase functions:log --tail
```

### Troubleshooting Commands

```bash
# Clear npm cache
npm cache clean --force

# Remove node_modules and reinstall
rm -rf node_modules package-lock.json
npm install

# Clear React build cache
rm -rf build

# Check Node version (should be 18+)
node --version

# Check npm version
npm --version

# Check Firebase CLI version
firebase --version

# Update Firebase CLI
npm install -g firebase-tools@latest

# Verify Firebase login
firebase login:list

# Re-login to Firebase
firebase logout
firebase login
```

### Git Commands

```bash
# Initialize git (if not already)
git init

# Add all files
git add .

# Commit changes
git commit -m "Initial commit: VAST Platform MVP"

# Add remote (replace with your repo)
git remote add origin https://github.com/yourusername/vast-platform.git

# Push to remote
git push -u origin main

# View status
git status

# View git log
git log --oneline
```

## Testing Commands

### Test VAST Endpoints Locally

```bash
# Start the app
npm start

# In another terminal, test with curl (after deploying functions):

# Test VAST XML endpoint
curl "https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net/vast?tagId=YOUR_TAG_ID"

# Test tracking endpoint
curl "https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net/track?event=impression&tagId=TAG_ID&creativeId=CREATIVE_ID"

# Test click endpoint (will redirect)
curl -L "https://us-central1-YOUR_PROJECT_ID.cloudfunctions.net/click?tagId=TAG_ID&creativeId=CREATIVE_ID&redirect=https://example.com"
```

### Firebase Emulators

```bash
# Start all emulators
firebase emulators:start

# Start specific emulators
firebase emulators:start --only firestore,storage,functions

# Start with UI
firebase emulators:start --import=./emulator-data --export-on-exit

# Run in background
firebase emulators:start &

# Stop emulators
# Press Ctrl+C or:
pkill -f "firebase emulators"
```

## Maintenance Commands

### Update Dependencies

```bash
# Check for outdated packages
npm outdated

# Update all packages (careful!)
npm update

# Update specific package
npm update firebase

# Update to latest major versions
# npx npm-check-updates -u
# npm install

# Update Firebase CLI
npm install -g firebase-tools@latest
```

### Backup & Export

```bash
# Export Firestore data
firebase firestore:export gs://YOUR_BUCKET/backups

# Import Firestore data  
firebase firestore:import gs://YOUR_BUCKET/backups/BACKUP_ID

# Download Storage files
# Use Firebase Console or gsutil
```

### Clean Up

```bash
# Remove build artifacts
rm -rf build

# Remove functions build
rm -rf functions/lib

# Remove all node_modules
rm -rf node_modules functions/node_modules

# Remove Firebase debug logs
rm -f firebase-debug.log
rm -f firebase-debug.*.log

# Complete clean reinstall
rm -rf node_modules functions/node_modules package-lock.json functions/package-lock.json
npm install
cd functions && npm install && cd ..
```

## Quick Reference

### Start Development
```bash
cd vast-platform
npm start
```

### Deploy Everything
```bash
npm run build
firebase deploy
```

### View Logs
```bash
firebase functions:log
```

### Update Code and Redeploy
```bash
# Make changes to code
git add .
git commit -m "Your changes"
npm run build
firebase deploy
```

---

## Command Cheat Sheet

| Task | Command |
|------|---------|
| Start dev server | `npm start` |
| Build for production | `npm run build` |
| Deploy all | `firebase deploy` |
| Deploy functions only | `firebase deploy --only functions` |
| Deploy hosting only | `firebase deploy --only hosting` |
| View logs | `firebase functions:log` |
| Open Firebase console | `firebase open` |
| Check project | `firebase use` |
| Start emulators | `firebase emulators:start` |
| Install dependencies | `npm install` |
| Build functions | `cd functions && npm run build && cd ..` |

---

**Tip:** Save this file for reference. You'll use these commands throughout development and deployment.
