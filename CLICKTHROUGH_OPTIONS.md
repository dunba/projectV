# Click-Through URL Options

## 🤔 Current Behavior

Your VAST XML currently has:

```xml
<ClickThrough>
  <![CDATA[
    https://us-central1-projectv-1844f.cloudfunctions.net/click?tagId=XXX&creativeId=YYY&redirect=YOUR_URL
  ]]>
</ClickThrough>
```

This means:
1. User clicks ad
2. Goes to our `/click` function first
3. We log the click event
4. We redirect to your actual URL

**Pros:** ✅ You track every click in Firestore
**Cons:** ❌ Extra redirect step

---

## 📋 Two Options

### **Option 1: Track Clicks (Current - Recommended)**

**What happens:**
1. User clicks ad → Goes to our function
2. Function logs click to Firestore
3. Function redirects to your URL
4. You see clicks in Reports!

**ClickThrough URL:**
```
https://us-central1-projectv-1844f.cloudfunctions.net/click?tagId=X&creativeId=Y&redirect=https://your-site.com
```

**Tracking:** ✅ Yes, clicks appear in Reports

---

### **Option 2: Direct Link (No Tracking)**

**What happens:**
1. User clicks ad → Goes directly to your URL
2. No tracking
3. No click events in Reports

**ClickThrough URL:**
```
https://your-site.com
```

**Tracking:** ❌ No click data

---

## 🎯 Which Do You Want?

### **I want Option 1 (Track clicks - RECOMMENDED)**

**Current setup is correct!** The VAST XML should show:
```xml
<VideoClicks>
  <ClickThrough><![CDATA[https://us-central1-projectv-1844f.cloudfunctions.net/click?tagId=XXX&creativeId=YYY&redirect=https://your-site.com]]></ClickThrough>
  <ClickTracking><![CDATA[https://us-central1-projectv-1844f.cloudfunctions.net/track?event=click&...]]></ClickTracking>
</VideoClicks>
```

**How it works:**
1. User clicks video
2. Browser goes to our `/click` function
3. Function logs click event to Firestore
4. Function immediately redirects (302) to your actual URL
5. User lands on your site
6. You see click data in Reports page

**This is the industry standard!** Most ad platforms do this.

---

### **I want Option 2 (Direct link)**

If you want the ClickThrough to go **directly** to your URL (no tracking):

I can change the code to:
```xml
<ClickThrough><![CDATA[https://your-site.com]]></ClickThrough>
```

**But you'll lose click tracking!**

---

## 🔍 Debug: Check Your Current XML

Open your VAST URL in browser:
```
https://us-central1-projectv-1844f.cloudfunctions.net/vast?tagId=YOUR_TAG_ID
```

Look for the `<ClickThrough>` element. What does it show?

### **If it shows:**

**A) Our click function URL:**
```xml
<ClickThrough><![CDATA[https://us-central1-projectv-1844f.cloudfunctions.net/click?...&redirect=https://your-site.com]]></ClickThrough>
```
✅ **This is correct for tracking!** It will redirect to your URL after logging the click.

**B) Just your URL:**
```xml
<ClickThrough><![CDATA[https://your-site.com]]></ClickThrough>
```
❌ Click tracking won't work.

**C) Something else or empty:**
```xml
<ClickThrough><![CDATA[]]></ClickThrough>
```
❌ Click-through URL wasn't saved in creative.

---

## 🧪 Test Click Tracking

### **Test the redirect manually:**

Open this in your browser (replace with actual values):
```
https://us-central1-projectv-1844f.cloudfunctions.net/click?tagId=YOUR_TAG&creativeId=YOUR_CREATIVE&redirect=https://google.com
```

**Expected behavior:**
1. Browser instantly redirects to google.com
2. Click event is logged in Firestore
3. You see it in Reports page

---

## 🐛 Common Issues

### **"ClickThrough is empty"**

**Cause:** Click-through URL wasn't saved when creating creative

**Fix:**
1. Go to Creatives page
2. Check if your creative has a "Click-Through URL"
3. If not, you need to re-upload with the URL

### **"ClickThrough doesn't have my URL"**

**Cause:** URL might not be in Firestore

**Check:**
1. Open Firebase Console
2. Go to Firestore Database
3. Navigate to: `organizations/org-default/creatives/YOUR_CREATIVE_ID`
4. Look for `clickThroughUrl` field
5. Should contain your URL (e.g., "https://example.com")

### **"I see the click function URL but want direct"**

If you want to **skip click tracking** and go directly to your URL:

Let me know and I'll modify the code to use direct URLs instead.

---

## 💡 Recommendation

**Keep Option 1 (Current)** because:
- ✅ You track every click
- ✅ See click data in Reports
- ✅ Calculate click-through rate
- ✅ Monitor campaign performance
- ✅ Industry standard approach
- ✅ Redirect is instant (users won't notice)

The redirect happens in milliseconds, users won't notice!

---

## 🔧 If You Need Direct Links

Tell me and I'll modify `functions/src/vast.ts` to change:

```typescript
// Current (with tracking):
<ClickThrough><![CDATA[${clickBaseUrl}?tagId=${tagId}&creativeId=${creative.id}&redirect=${encodeURIComponent(creative.clickThroughUrl)}]]></ClickThrough>

// Changed to (direct):
<ClickThrough><![CDATA[${creative.clickThroughUrl}]]></ClickThrough>
```

---

## 📊 What Each Element Does

```xml
<VideoClicks>
  <!-- Where user goes when they click -->
  <ClickThrough>...</ClickThrough>
  
  <!-- Tracking pixel fired on click (for reporting) -->
  <ClickTracking>...</ClickTracking>
</VideoClicks>
```

Both can coexist! Video players call both.

---

## ❓ Questions to Answer

1. **What URL did you set as click-through when creating the creative?**
   - Example: https://example.com/landing-page

2. **What URL shows in your VAST XML ClickThrough element?**
   - Open VAST URL in browser and check

3. **Do you want click tracking or direct links?**
   - Track clicks (recommended) ✅
   - Direct links (lose tracking) ❌

Let me know and I'll help fix it! 🚀
