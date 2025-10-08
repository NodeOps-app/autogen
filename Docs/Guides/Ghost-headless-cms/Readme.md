# Ghost Headless CMS

Use this guide to walkthrough setting up a backend/frontend for the popular blogging platform, Ghost. 
<!-- add link to Ghost site or Ghost docs --> 

## Prerequisites

- GitHub account
- AutoGen account
- Basic dev skills

## Overview

This guide demonstrates deploying Ghost as a headless CMS with a separate Next.js frontend using the NodeOps platform. This creates a scalable BE/FE architecture where the backend serves content via API and the frontend consumes it.

## ⚠️ Security notice

**This guide uses hardcoded API keys for simplicity. Never do this in  production deployments. See the [environment variables extension](./environment-variables.md) guide to understand how to properly handle API keys.**

## Architecture

```
┌─────────────────┐    HTTP API    ┌─────────────────┐
│   Ghost CMS     │◄──────────────►│   Next.js FE    │
│   (Backend)     │                │   (Frontend)    │
│   Platform      │                │   Platform      │
└─────────────────┘                └─────────────────┘
```

## Step 1: Set up backend 

The first step is to deploy your Ghost CMS backend. This will host the blogs and the API server.

<details>
  <summary>Next.js project structure</summary>

```
fe/
├── pages/
│   ├── index.js      # Main page component
│   └── _app.js       # App configuration
├── styles/
│   └── globals.css   # Global styles
├── package.json      # Dependencies
└── .gitignore        # Excludes node_modules
```

</details>

### 1.1 Platform Configuration

1. Logged into the [AutoGen app](https://autogen.nodeops.network/), click **+ New Project**. 

2. Select Docker and complete the following:

**Image**: `ghost:5-alpine`  
**Port**: `2368`

**Environment variables**:
  - NODE_ENV=production
  - database__client=sqlite3
  - database__connection__filename=/var/lib/ghost/content/data/ghost.db
  - database__useNullAsDefault=true

## 1.2 Collect API keys and URLs from Ghost

1. From the deployment's card, click **View details** > **Visit Project**.
2. Access admin panel by appending `/ghost` to your project's URL: `https://your-backend-url.com/ghost/`
3. Complete the Ghost setup wizard.
> There is no requirement to use the same email as you use for AutoGen.
4. Navigate Settings > Integrations > Create custom integration and name it.
5. Make a note of the following:
- The **Content API Key** (we'll use this in frontend).
- URL for the backend: `https://your-backend-url.com`
<!-- - URL for the Admin Panel: `https://your-backend-url.com/ghost/` -->
- URL for the Content API: `https://your-backend-url.com/ghost/api/content/`

## Step 2: Set up frontend

Next, we will leverage a basic Next.js app to consume the backend data via API and deploy that as a separate project.

### 2.1 Platform configuration

1. Fork this GitHub to your own profile {todo add url to boilerplate}

2. Logged into the [AutoGen app](https://autogen.nodeops.network/), click **+ New Project**. 

3. Select the GitHub from step 2.1.1 
<!-- grab the details from other docs pages -->  and complete the following:

**Repository**: {Select the repo from the dropdown}
**Branch**: {Select the branch, default to main if you forked and didn't edit}  
**Framework**: Next.js  
**Port**: `3000`
**Build Command**: `npm run build`  
**Start Command**: `npm start`  


### 2.2 Configure your frontend implementation

1. Edit the index.js file to hardcode your URL and API key.

**pages/index.js**:
```javascript
import { useState, useEffect } from 'react';

// ⚠️ Hardcoded API key - see environment-variables.md for production
const GHOST_API_URL = 'https://your-backend-url.com/ghost/api/content';
const GHOST_API_KEY = 'your-content-api-key-here';

  ...

```

## Test your deployments

1. **Backend**: Create a test post in Ghost admin panel
2. **Frontend**: Visit your deployed frontend URL
3. **Verification**: Confirm the post appears on the frontend
4. **Real-time**: Add another post and refresh the frontend

## Troubleshoot your deployments

1. Test your backend API with curl:
   ```bash
   curl "https://your-backend-url.com/ghost/api/content/posts?key=your-content-api-key"
   ```

2. Test your frontend locally:
   ```bash
   cd fe/
   npm install
   npm run dev
   ```
   Visit `http://localhost:3000` to see your frontend connecting to the backend API.

## Benefits

- **Scalability**: BE/FE can scale independently
- **Flexibility**: Use any frontend technology
- **Performance**: Frontend optimized for delivery
- **Development**: Teams can work separately
- **Deployment**: Different platforms for BE/FE

## Next Steps

- **Production Security**: [Environment Variables Extension](./environment-variables.md)

