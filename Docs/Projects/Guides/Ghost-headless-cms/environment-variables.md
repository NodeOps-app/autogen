🚧 This page is WIP 🚧


# Environment variables guide extension

This extension shows how to properly secure your Ghost headless CMS by using environment variables instead of hardcoded API keys. This is the **production-ready approach**.

> ⚠️ **Security Importance**: Hardcoded API keys in frontend code are visible to anyone who views your website's source code. This creates serious security vulnerabilities.

You will:

- Replace hardcoded API keys with environment variables
- Configure secure API key handling in NodeOps platform
- Implement production-ready security practices
- Understand different security levels for API access

## Why use environment variables?

Malicious users could spam your Ghost site or delete content. This is because hardcoded API keys are exposed to anyone who can view your website's source code. In the Ghost be/fe project, we hard coded:

- **Content API Key**: Allows reading all content
- **Admin API Key**: Allows creating, editing, and deleting content

Environment variables keep sensitive data on the server side, never exposing it to the browser.

## Method 1: Client-side environment variables for local testing

### Step 1: Update Frontend Code

Replace the hardcoded values in your `pages/index.js`:

```javascript
import { useState, useEffect } from 'react';

// ✅ Secure: Using environment variables
const GHOST_API_URL = process.env.NEXT_PUBLIC_GHOST_API_URL;
const GHOST_API_KEY = process.env.NEXT_PUBLIC_GHOST_API_KEY;

async function fetchGhostData(endpoint) {
  try {
    const response = await fetch(`${GHOST_API_URL}/${endpoint}?key=${GHOST_API_KEY}`);
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    return await response.json();
  } catch (error) {
    console.error(`Error fetching ${endpoint}:`, error);
    return { errors: [{ message: `Failed to fetch ${endpoint}` }] };
  }
}

// ... rest of component remains the same
```

### Step 2: Create a local environment file

Create `.env.local` in your `fe/` directory for local development:

```
NEXT_PUBLIC_GHOST_API_URL=https://your-backend-url.com/ghost/api/content
NEXT_PUBLIC_GHOST_API_KEY=your-content-api-key-here
```

### Step 3: Configure Platform Environment Variables

In your NodeOps frontend project, add these environment variables:

| Variable Name | Value |
|---------------|-------|
| `NEXT_PUBLIC_GHOST_API_URL` | `https://your-backend-url.com/ghost/api/content` |
| `NEXT_PUBLIC_GHOST_API_KEY` | `your-content-api-key-here` |

<details>
  <summary>How to add environment variables in NodeOps</summary>

1. Go to your frontend project in NodeOps
2. Click **View Details**
3. Scroll to **Environment Variables** section
4. Click **Add Environment Variable**
5. Enter the key-value pairs from the table above
6. Click **Save** and redeploy your project

</details>

## Method 2: Server-Side API Routes (Most Secure)

For maximum security, use Next.js API routes to proxy requests:

### Step 1: Create API Route

Create `pages/api/ghost.js`:

```javascript
// Server-side API route - API key never exposed to browser
export default async function handler(req, res) {
  const { endpoint } = req.query;
  
  const GHOST_API_URL = process.env.GHOST_API_URL;
  const GHOST_API_KEY = process.env.GHOST_API_KEY;
  
  try {
    const response = await fetch(`${GHOST_API_URL}/${endpoint}?key=${GHOST_API_KEY}`);
    const data = await response.json();
    
    res.status(200).json(data);
  } catch (error) {
    res.status(500).json({ error: 'Failed to fetch data' });
  }
}
```

### Step 2: Update Frontend Code

Update your `pages/index.js` to use the API route:

```javascript
// ✅ Most secure: API key stays on server
async function fetchGhostData(endpoint) {
  try {
    const response = await fetch(`/api/ghost?endpoint=${endpoint}`);
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    return await response.json();
  } catch (error) {
    console.error(`Error fetching ${endpoint}:`, error);
    return { errors: [{ message: `Failed to fetch ${endpoint}` }] };
  }
}
```

### Step 3: Configure Server-Side Environment Variables

In your NodeOps frontend project, add these environment variables:

| Variable Name | Value |
|---------------|-------|
| `GHOST_API_URL` | `https://your-backend-url.com/ghost/api/content` |
| `GHOST_API_KEY` | `your-content-api-key-here` |

> Note: No `NEXT_PUBLIC_` prefix needed for server-side variables.

## Security Levels Comparison

| Method | Security Level | Use Case | API Key Exposure |
|--------|---------------|----------|------------------|
| Hardcoded Keys | ❌ Dangerous | Learning/Demo only | Visible in browser |
| `NEXT_PUBLIC_*` | ⚠️ Moderate | Public APIs only | Visible in browser |
| Server-Side API Routes | ✅ Secure | Production recommended | Never exposed |

## Migration Steps

1. **Backup** your current implementation
2. **Choose** your preferred method (Method 1 or 2)
3. **Create** environment variables in NodeOps platform
4. **Update** frontend code to use environment variables
5. **Test** thoroughly in staging environment
6. **Deploy** to production

<details>
  <summary>Step-by-step migration</summary>

**For Method 1 (Client-side):**

1. Update `pages/index.js` to use `process.env.NEXT_PUBLIC_GHOST_API_URL`
2. Add environment variables in NodeOps platform
3. Test locally with `.env.local` file
4. Deploy and verify

**For Method 2 (Server-side):**

1. Create `pages/api/ghost.js` API route
2. Update `pages/index.js` to call `/api/ghost`
3. Add server-side environment variables in NodeOps platform
4. Test locally and deploy

</details>

## Best Practices

1. **Never commit** `.env` files to version control
2. **Use Content API Key** for read-only operations
3. **Use Admin API Key** only in server-side code
4. **Rotate API keys** regularly
5. **Monitor API usage** for suspicious activity
6. **Use HTTPS** for all API communications

## Troubleshooting

<details>
  <summary>Common issues and solutions</summary>

**Environment variables not loading:**

- Ensure `NEXT_PUBLIC_` prefix for client-side variables
- Check variable names match exactly (case-sensitive)
- Verify variables are set in NodeOps platform
- Redeploy after adding environment variables

**API calls failing:**

- Check variable names and values
- Verify the Ghost backend is accessible
- Test the API endpoint directly with curl
- Check browser console for errors

**Build errors:**

- Ensure all required variables are set
- Check for typos in variable names
- Verify Next.js can access the variables

</details>

## Verification

Add this temporarily to verify environment variables are working:

```javascript
// Add this temporarily to verify environment variables
console.log('API URL:', process.env.NEXT_PUBLIC_GHOST_API_URL);
console.log('API Key exists:', !!process.env.NEXT_PUBLIC_GHOST_API_KEY);
```

