🚧 This page is WIP 🚧

# Ghost Headless CMS

Use this guide to walkthrough setting up a backend/frontend for the popular blogging platform, [Ghost](https://ghost.org/). 

## Prerequisites

- GitHub account
- AutoGen account
- Basic dev skills

## Overview

This guide demonstrates deploying Ghost as a headless CMS with a separate Next.js frontend using the NodeOps platform. This creates a scalable BE/FE architecture where the backend serves content via API which the frontend consumes.

> There is no need to run Ghost with separate BE/FE. [Follow the guide to understand how to run Ghost as a single container](../../../Docker-Integration/Guides/Readme.md), or remain here for a demo of a BE/FE app on AutoGen.

## ⚠️ Security notice

This guide uses hardcoded API keys for simplicity. Never do this in production deployments. See the [environment variables extension](./environment-variables.md) guide to understand how to properly handle API keys.

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

### 1.1 Platform Configuration

1. Logged into the [AutoGen app](https://autogen.nodeops.network/), click **+ New Project**. 

2. Click on **Deploy Docker Container**.
3. Populate:

    **Image**: `ghost:5-alpine`  
    **Port**: `2368`

    **Environment variables**:
      - NODE_ENV=production
      - database_client=sqlite3
      - database__onnection__filename=/var/lib/ghost/content/data/ghost.db
      - database_useNullAsDefault=true

## 1.2 Collect API keys and URLs from Ghost

1. From the deployment's card, click **View details** > **Visit Project**.
2. Access admin panel by appending `/ghost` to your project's URL: `https://your-backend-url.com/ghost/`
3. Complete the Ghost setup wizard.
> There is no requirement to use the same email as you use for AutoGen.
4. Navigate Settings > Integrations > Create custom integration and name it.
5. Make a note of the following:
- The **Content API Key**
- URL for the backend: `https://your-backend-url.com`
<!-- - URL for the Admin Panel: `https://your-backend-url.com/ghost/` -->
- URL for the Content API: `https://your-backend-url.com/ghost/api/content/`

## Step 2: Set up frontend

Next, we will leverage a basic Next.js app to consume the backend data via API and deploy that as a separate project.

### 2.1 Platform configuration

1. Fork this GitHub to your own profile {todo add url to boilerplate}

> Alternatively, recreate the app based on the [data here](#nextjs-app)

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
- **Deployment**: Different platforms for BE/FE and different frontends can be developed for different use cases such as mobile vs. desktop.

## Next Steps

- **Production Security**: [Environment Variables Extension](./environment-variables.md)

## Next.js App

<details>
  <summary>Show me the Next.js files</summary>

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
<details>
  <summary>Next.js pages/index.js</summary>

```javascript
import { useState, useEffect } from 'react';

// ⚠️ Hardcoded API key - see environment-variables.md for production
const GHOST_API_URL = 'https://your-backend-url.com/ghost/api/content';
const GHOST_API_KEY = 'your-content-api-key-here';

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

export default function Home() {
  const [posts, setPosts] = useState([]);
  const [site, setSite] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    async function loadGhostContent() {
      try {
        const postsData = await fetchGhostData('posts');
        const siteData = await fetchGhostData('settings');

        if (postsData.errors || siteData.errors) {
          throw new Error(postsData.errors?.[0]?.message || siteData.errors?.[0]?.message);
        }

        setPosts(postsData.posts);
        setSite(siteData.settings);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    }
    loadGhostContent();
  }, []);

  if (loading) return <div className="loading">Loading Ghost content...</div>;
  if (error) return <div className="error">Error: {error}</div>;
  if (!site) return <div className="error">Site data not found.</div>;

  return (
    <div className="container">
      <header className="site-header">
        <h1>{site.title}</h1>
        <p>{site.description}</p>
      </header>

      <main className="posts-list">
        {posts.length > 0 ? (
          posts.map(post => (
            <article key={post.id} className="post-card">
              <h2>{post.title}</h2>
              <div dangerouslySetInnerHTML={{ __html: post.html }} />
            </article>
          ))
        ) : (
          <p>No posts found.</p>
        )}
      </main>

      <footer className="site-footer">
        <p>&copy; {new Date().getFullYear()} {site.title}</p>
      </footer>
    </div>
  );
}
```

</details>

<details>
  <summary>Next.js pages/_app.js</summary>

```javascript
import '../styles/globals.css';

function MyApp({ Component, pageProps }) {
  return <Component {...pageProps} />;
}

export default MyApp;
```

</details>

<details>
  <summary>Next.js styles/globals.css</summary>

```css
/* Global Styles */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  line-height: 1.6;
  color: #333;
  background-color: #f8f9fa;
}

.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.site-header {
  text-align: center;
  margin-bottom: 40px;
  padding: 40px 0;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.site-header h1 {
  font-size: 2.5rem;
  color: #2c3e50;
  margin-bottom: 10px;
}

.site-header p {
  font-size: 1.1rem;
  color: #7f8c8d;
}

.posts-list {
  margin-bottom: 40px;
}

.post-card {
  background: white;
  margin-bottom: 30px;
  padding: 30px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  transition: transform 0.2s ease;
}

.post-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0,0,0,0.15);
}

.post-card h2 {
  font-size: 1.8rem;
  color: #2c3e50;
  margin-bottom: 15px;
}

.post-card p {
  color: #555;
  line-height: 1.7;
}

.site-footer {
  text-align: center;
  padding: 20px;
  color: #7f8c8d;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.loading, .error {
  text-align: center;
  padding: 40px;
  font-size: 1.2rem;
}

.loading {
  color: #3498db;
}

.error {
  color: #e74c3c;
}
```

</details>

<details>
  <summary>Next.js package.json</summary>

```json
{
  "name": "ghost-frontend",
  "version": "1.0.0",
  "description": "Next.js frontend for Ghost BE/FE separation",
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  },
  "dependencies": {
    "next": "^14.0.0",
    "react": "^18.0.0",
    "react-dom": "^18.0.0"
  },
  "engines": {
    "node": ">=18.0.0"
  }
}
```

</details>

<details>
  <summary>Next.js .gitignore</summary>

```
# Dependencies
node_modules/
npm-debug.log*

# Next.js
.next/
out/

# Environment variables
.env*

# IDE
.vscode/
.idea/
```

</details>

</details>