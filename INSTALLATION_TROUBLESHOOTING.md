# 🔧 Installation Troubleshooting Guide

## Problem: App Redirects to Shopify App Store Instead of Installing

If clicking "Install" from the Shopify Partner Dashboard redirects to the Shopify App Store instead of installing the app directly, this guide will help you fix it.

## 🎯 Root Causes & Solutions

### 1. **Partner Dashboard Configuration Issues**

#### ❌ Common Mistakes:
- App URL not set correctly
- Callback URLs missing or incorrect
- App not set to "Development" mode
- Wrong scopes configured

#### ✅ Correct Configuration:

**In Shopify Partner Dashboard:**
```
App Setup > App URL: https://your-app-domain.com/
App Setup > Allowed redirection URLs: 
  - https://your-app-domain.com/auth/shopify/callback
  - https://your-app-domain.com/auth/callback

App Setup > App proxy URL: (leave empty for now)
App Setup > Embedded app: Yes
```

**Important:** Make sure your app is in **Development** mode, not **Published** mode.

### 2. **Environment Variables Not Set**

#### Check Required Variables:
```bash
# Required for installation
SHOPIFY_API_KEY=your_api_key_here
SHOPIFY_API_SECRET_KEY=your_secret_key_here
HOST=https://your-app-domain.com

# Required for Facebook integration
FACEBOOK_APP_ID=your_facebook_app_id
FACEBOOK_APP_SECRET=your_facebook_app_secret

# Required for database
DATABASE_URL=your_database_connection_string
SESSION_SECRET=your_random_session_secret
```

### 3. **App URL Configuration**

#### Check shopify.app.toml:
```toml
name = "AI Facebook Ads Pro"
client_id = "{{ SHOPIFY_API_KEY }}"
application_url = "{{ HOST }}/"  # Note the trailing slash
embedded = true

[auth]
redirect_urls = [
  "{{ HOST }}/auth/shopify/callback",
  "{{ HOST }}/auth/callback"
]
```

### 4. **SSL/HTTPS Issues**

#### Requirements:
- ✅ App must be served over HTTPS in production
- ✅ SSL certificate must be valid
- ✅ No mixed content warnings
- ✅ All redirects must use HTTPS

#### Local Development:
For local development, use ngrok or similar:
```bash
# Install ngrok
npm install -g ngrok

# Start your app
npm start

# In another terminal, expose it
ngrok http 3000

# Use the HTTPS URL from ngrok as your HOST
```

### 5. **App Store Listing Issues**

#### If App is Published:
If your app is already published on the App Store, Shopify might redirect to the store listing instead of allowing direct installation.

**Solution:**
1. Go to Partner Dashboard
2. Navigate to your app
3. Go to "Distribution" section
4. Change from "Public" to "Development" mode
5. Save changes

### 6. **Browser/Cache Issues**

#### Clear Everything:
1. Clear browser cache and cookies
2. Try in incognito/private mode
3. Try different browser
4. Clear Shopify admin cache

## 🔍 Debugging Steps

### Step 1: Test Debug Endpoint
Visit: `https://your-app-domain.com/debug`

This will show:
- Environment configuration
- Current settings
- Proper URLs to use

### Step 2: Test Direct Installation
Try installing manually:
`https://your-app-domain.com/?shop=your-test-store.myshopify.com`

### Step 3: Check Logs
Monitor your app logs when clicking "Install" from Partner Dashboard:
```bash
# If using Vercel
vercel logs

# If using Heroku
heroku logs --tail

# If running locally
npm start
```

### Step 4: Verify OAuth Flow
1. Click "Install" from Partner Dashboard
2. Check if you see logs in your app
3. Verify the OAuth redirect happens
4. Check for any error messages

## 🚀 Quick Fix Checklist

### Partner Dashboard Settings:
- [ ] App URL: `https://your-domain.com/`
- [ ] Callback URL: `https://your-domain.com/auth/shopify/callback`
- [ ] App is in Development mode
- [ ] Embedded app is enabled
- [ ] Correct scopes are set

### Environment Variables:
- [ ] `SHOPIFY_API_KEY` is set
- [ ] `SHOPIFY_API_SECRET_KEY` is set
- [ ] `HOST` is set to your app's URL
- [ ] All other required variables are set

### App Configuration:
- [ ] `shopify.app.toml` has correct URLs
- [ ] App is served over HTTPS
- [ ] SSL certificate is valid
- [ ] No console errors in browser

### Testing:
- [ ] `/debug` endpoint works
- [ ] Direct installation URL works
- [ ] OAuth flow completes successfully
- [ ] App loads in Shopify admin

## 🔧 Common Solutions

### Solution 1: Reset Partner Dashboard URLs
1. Go to Partner Dashboard
2. Navigate to your app
3. Go to "App setup"
4. Update App URL to: `https://your-domain.com/`
5. Update Callback URLs to: `https://your-domain.com/auth/shopify/callback`
6. Save and test

### Solution 2: Force Development Mode
1. In Partner Dashboard
2. Go to "Distribution"
3. If published, change to "Development"
4. Save changes
5. Test installation

### Solution 3: Clear All Caches
1. Clear browser cache
2. Clear Shopify admin cache
3. Restart your app
4. Test in incognito mode

### Solution 4: Verify Environment
1. Check all environment variables
2. Restart your app after changes
3. Test the `/debug` endpoint
4. Verify HTTPS is working

## 📞 Still Having Issues?

### Check These:

1. **Network Issues:**
   - Is your app accessible from the internet?
   - Are there any firewall restrictions?
   - Is the domain resolving correctly?

2. **Shopify API Issues:**
   - Are your API credentials correct?
   - Is your app approved for the scopes you're requesting?
   - Are there any API rate limits being hit?

3. **Code Issues:**
   - Are there any JavaScript errors in the console?
   - Are there any server errors in the logs?
   - Is the OAuth flow implemented correctly?

### Debug Commands:
```bash
# Test if your app is accessible
curl -I https://your-domain.com/

# Test the debug endpoint
curl https://your-domain.com/debug

# Test direct installation
curl "https://your-domain.com/?shop=test.myshopify.com"
```

## ✅ Success Indicators

When everything is working correctly:

1. **Partner Dashboard:** Clicking "Install" redirects to your app
2. **OAuth Flow:** Shopify authorization page appears
3. **Installation:** After authorization, app loads in Shopify admin
4. **Logs:** You see installation requests in your app logs
5. **Database:** User record is created in your database

## 🎉 Final Notes

- Always test in a development store first
- Keep your Partner Dashboard in development mode during testing
- Monitor your app logs during installation attempts
- Use the `/debug` endpoint to verify configuration
- Test the complete flow from Partner Dashboard to app loading

The most common issue is incorrect Partner Dashboard configuration. Double-check your App URL and Callback URLs first!