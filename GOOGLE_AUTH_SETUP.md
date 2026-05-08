# Google Authentication Setup Guide

Your VAST Platform now uses **Google Sign-In** instead of email/password authentication.

## ✅ What Changed

- ✅ Login page now shows "Sign in with Google" button
- ✅ Beautiful Google button with official logo
- ✅ User profile photo displayed in navbar
- ✅ Display name shown instead of just email
- ✅ Automatic user creation on first login
- ✅ Simpler, more secure authentication

## 🚀 Setup Instructions

### Step 1: Enable Google Sign-In in Firebase Console

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Select your project
3. Click **Authentication** in the left sidebar
4. Click **Sign-in method** tab
5. Click **Google** in the providers list
6. Toggle **Enable** to ON
7. Enter your **Project support email** (required)
8. Click **Save**

**That's it!** No API keys or client IDs needed for basic setup.

### Step 2: Test Locally

```bash
cd vast-platform
npm start
```

1. Visit `http://localhost:3000`
2. Click "Sign in with Google"
3. Select your Google account
4. You'll be redirected to the dashboard!

### Step 3: Configure for Production (After Deployment)

Once you deploy to Firebase Hosting, you need to add your domain:

1. Go to Firebase Console > Authentication > Settings
2. Scroll to **Authorized domains**
3. Your Firebase domain is already authorized (e.g., `your-app.web.app`)
4. If using custom domain, click **Add domain** and enter it

## 🎨 What You Get

### User Experience
- One-click sign-in with Google
- No password to remember
- Profile photo in navbar
- Display name shown
- Automatic account creation

### Security Benefits
- No password storage
- Google handles security
- 2FA if user has it enabled
- OAuth 2.0 standard
- Automatic token refresh

### Developer Benefits
- Less code to maintain
- No password reset flow needed
- No email verification needed
- Built-in session management
- Profile data included (name, photo, email)

## 📊 User Data Stored in Firestore

When a user signs in, this document is created/updated:

```
organizations/org-default/users/{uid}
{
  email: "user@gmail.com",
  displayName: "John Doe",
  photoURL: "https://lh3.googleusercontent.com/...",
  role: "admin",
  createdAt: timestamp,
  lastLogin: timestamp
}
```

## 🔧 Customization Options

### Change Button Style

Edit `src/pages/Login.jsx`:

```javascript
// Current: White button with Google logo
className="bg-white hover:bg-gray-50 text-gray-700"

// Blue button (Google brand color):
className="bg-blue-600 hover:bg-blue-700 text-white"

// Or custom colors:
className="bg-purple-600 hover:bg-purple-700 text-white"
```

### Force Account Selection

Already enabled! Users always see account picker, even if logged in:

```javascript
provider.setCustomParameters({
  prompt: 'select_account'
});
```

### Add More OAuth Providers

Want to support Microsoft, GitHub, etc.? Just add them:

```javascript
// In auth.js
import { GithubAuthProvider } from 'firebase/auth';

export const loginWithGithub = async () => {
  const provider = new GithubAuthProvider();
  return await signInWithPopup(auth, provider);
};
```

Then add button in Login.jsx.

### Restrict to Specific Domain

Want to only allow users from your company domain?

```javascript
// In auth.js - after sign-in:
if (!user.email.endsWith('@yourcompany.com')) {
  await auth.signOut();
  throw new Error('Only @yourcompany.com emails allowed');
}
```

## 🔒 Security Rules

Your Firestore rules still work - they just check `request.auth != null`:

```javascript
// firestore.rules
match /organizations/{orgId}/{document=**} {
  allow read, write: if request.auth != null;
}
```

This works for Google, email/password, or any auth provider.

## 🐛 Troubleshooting

### Pop-up Blocked

**Error:** "Pop-up blocked by browser"

**Solution:** 
- Click the pop-up blocker icon in browser address bar
- Select "Always allow pop-ups from this site"
- Try again

### Pop-up Closed by User

**Error:** "Sign-in cancelled"

**Solution:** User closed the pop-up. They just need to try again.

### Auth Domain Not Authorized

**Error:** "This domain is not authorized for OAuth operations"

**Solution:**
1. Go to Firebase Console > Authentication > Settings
2. Add your domain to **Authorized domains**
3. Wait a few minutes for changes to propagate

### Google Account Picker Doesn't Show

**Cause:** User is already logged into one Google account

**Solution:** Already handled! We force account picker with:
```javascript
provider.setCustomParameters({ prompt: 'select_account' });
```

### User Photo Not Showing

**Cause:** User doesn't have a Google profile photo

**Solution:** Code already handles this - displays name only if no photo

## 📱 Testing Different Scenarios

### Test with Multiple Google Accounts

1. Sign in with first account
2. Logout
3. Sign in again - you'll see account picker
4. Select different account
5. Both users will have separate profiles

### Test First-Time vs Returning User

**First Time:**
- User signs in
- User document created in Firestore
- Redirected to dashboard

**Returning User:**
- User signs in
- Last login timestamp updated
- Profile info refreshed (name, photo)
- Redirected to dashboard

## 🚀 Production Deployment

Works exactly the same after deployment:

```bash
npm run build
firebase deploy
```

Just make sure your production domain is in Firebase **Authorized domains**.

## 📖 Firebase Auth Docs

For more advanced features:
- [Google Sign-In Documentation](https://firebase.google.com/docs/auth/web/google-signin)
- [Manage Users](https://firebase.google.com/docs/auth/web/manage-users)
- [Auth State Persistence](https://firebase.google.com/docs/auth/web/auth-state-persistence)

## 💡 Pro Tips

### 1. User Profile in Dashboard

Want to show user info in the dashboard?

```javascript
// In Dashboard.jsx
import { useAuth } from '../hooks/useAuth';

const Dashboard = () => {
  const { user } = useAuth();
  
  return (
    <div>
      <h1>Welcome, {user?.displayName}!</h1>
      {user?.photoURL && <img src={user.photoURL} alt="Profile" />}
    </div>
  );
};
```

### 2. Sign Out from Google Too

Current logout only signs out from Firebase. To also sign out from Google:

```javascript
// In auth.js
export const logoutUser = async () => {
  await signOut(auth);
  // Optional: Redirect to Google logout
  // window.location.href = 'https://accounts.google.com/Logout';
};
```

### 3. Handle Errors Gracefully

Already implemented! Errors are shown with friendly messages:
- "Sign-in cancelled. Please try again."
- "Pop-up blocked. Please allow pop-ups."

### 4. Silent Sign-In

Want to check if user is already signed in without showing UI?

```javascript
// Already works! Firebase persists auth state
// User stays logged in across page refreshes
```

## ✅ Checklist

Before going live:

- [ ] Google Sign-In enabled in Firebase Console
- [ ] Support email added in Firebase
- [ ] Tested login flow locally
- [ ] Profile photo displays correctly
- [ ] Logout works
- [ ] User document created in Firestore
- [ ] Production domain added to authorized domains
- [ ] Pop-up blocker warning shown to users
- [ ] Terms of service added (optional)

## 🎉 You're Done!

Your VAST Platform now has Google authentication!

**Benefits:**
- ✅ Easier for users (one-click login)
- ✅ More secure (Google handles security)
- ✅ Less code to maintain
- ✅ Better UX (profile photos, display names)
- ✅ No password resets needed

**Test it now:**
```bash
npm start
```

Then click "Sign in with Google"!

---

## Still Want Email/Password?

If you want BOTH Google and email/password:

1. Keep `loginWithGoogle()` in `auth.js`
2. Add back the email/password functions
3. Add both buttons to `Login.jsx`
4. Enable both in Firebase Console

But for most apps, Google-only is simpler and more secure!
