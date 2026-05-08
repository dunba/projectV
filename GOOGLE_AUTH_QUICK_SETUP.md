# 🚀 Google Auth - 2 Minute Setup

## What You Need to Do

### 1. Enable Google Sign-In (1 minute)

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Select your project
3. Click **Authentication** → **Sign-in method**
4. Click **Google**
5. Toggle **Enable** to ON
6. Enter your support email
7. Click **Save**

### 2. Test It (1 minute)

```bash
cd vast-platform
npm start
```

Visit `http://localhost:3000` and click "Sign in with Google"!

## That's It! 🎉

No API keys, no configuration files, no complex setup.

---

## What Changed from Email/Password

**Before:**
- User enters email and password
- Needs to remember password
- Need password reset flow
- Need email verification

**After (Google):**
- User clicks "Sign in with Google"
- One click - done!
- No password to manage
- More secure
- Profile photo included

---

## How It Looks

### Login Page
```
┌─────────────────────────────────┐
│      VAST Platform              │
│  Sign in with your Google       │
│         account                 │
│                                 │
│  ┌────────────────────────┐    │
│  │  [G] Sign in with      │    │
│  │      Google            │    │
│  └────────────────────────┘    │
│                                 │
└─────────────────────────────────┘
```

### After Login
```
┌──────────────────────────────────────┐
│  VAST Platform    👤 John Doe  Logout│
└──────────────────────────────────────┘
     ↑ User photo and name shown
```

---

## User Flow

1. User clicks "Sign in with Google"
2. Google pop-up appears
3. User selects account
4. **First time:** User document created in Firestore
5. **Returning:** Last login updated
6. Redirected to dashboard

---

## Production Deployment

No changes needed! After deploying:

```bash
firebase deploy
```

Your `.web.app` domain is automatically authorized.

If using custom domain:
1. Firebase Console → Authentication → Settings
2. Add domain to "Authorized domains"

---

## Troubleshooting

**Pop-up blocked?**
→ Allow pop-ups for this site in browser settings

**Domain not authorized?**
→ Add your domain in Firebase Console → Authentication → Settings

**Still stuck?**
→ See [GOOGLE_AUTH_SETUP.md](./GOOGLE_AUTH_SETUP.md) for detailed guide

---

## Files Changed

- `src/services/auth.js` - Added `loginWithGoogle()`
- `src/pages/Login.jsx` - Google Sign-In button
- `src/components/Layout/Navbar.jsx` - Show profile photo

Everything else works exactly the same!

---

## Want Email/Password Back?

See [GOOGLE_AUTH_SETUP.md](./GOOGLE_AUTH_SETUP.md) for how to enable both.

But Google-only is recommended:
- ✅ Simpler
- ✅ More secure
- ✅ Better UX
- ✅ Less code

---

**Ready?** Enable Google in Firebase Console and run `npm start`!
