# Guide to use the AI agent

> Currently, AutoGen does not allow you to start with an empty repository. It requires a deployable app with a provided Dockerfile (or it assigns an LLM to create one).

Follow along with this tutorial to get started with a boilerplate Next.js app and get coding assistance from the AI agent.

You will:

- Create/fork a boilerplate app in GitHub
- Deploy that app with AutoGen
- Launch a sandboxed instance of the app: getting access to an AI coding assistant

![ai agent guide](../../Static/Gifs/guide-ai-agent.gif)

## Prerequisites

- GitHub account
- [AutoGen](https://autogen.nodeops.network/) account

## Step 1: Create a Next.js app

Either fork this repository: [github.com/NodeOps-app/boilerplate-next.js](https://github.com/NodeOps-app/boilerplate-next.js).

Or recreate the 3-page repo in your own GitHub.

<details>
  <summary>3-page repo details</summary>

  ```your-nextjs-project/
  ├── Dockerfile   
  ├── package.json
  ├── pages/
  │   └── index.js
  ```

  <details>
    <summary>pages/index.js contents</summary>

  ```
  export default function Home() {
    return <h1>Hello World</h1>
  }
  ```

  </details>

  <details>
    <summary>package.json contents</summary>

  ```
  {
    "name": "minimal-next-app",
    "scripts": {
      "dev": "next dev",
      "build": "next build",
      "start": "next start"
    },
    "dependencies": {
      "next": "latest",
      "react": "latest",
      "react-dom": "latest"
    }
  }
  ```

  </details>

  <details>
    <summary>Dockerfile contents</summary>

  ```FROM node:18-alpine
  WORKDIR /app
  COPY package.json package-lock.json* ./
  RUN npm install
  COPY . .
  RUN npm run build
  EXPOSE 3000
  CMD ["npm", "start"]
  ```

  </details>

</details>

## Step 2: Deploy your Next.js app

1. Logged into [AutoGen](https://autogen.nodeops.network/), click **+ New Project**.
2. Click **Browse Repositories** and browse to the GitHub repo that contains your app.
> If required, follow the [GitHub Get Started](../GitHub-Integration/github-support.md#set-up-github-integration), then return here.
<br></br>
> If required, follow the [GitHub reconfiguration guide](../GitHub-Integration/github-support.md#amend-github-integration) to alter which repositories are available from AutoGen, then return here.

3. Configure the deployment, this guide uses:

- Branch: main
- Framework: Next.js
- Port: 3000
- Name: {assign unique name}
- My project has a Dockerfile: **toggle on**

click **Deploy**.

> You may verify the build by clicking **Visit Project** to see the **Hello World** page.

## Step 3: Launch sandbox and AI agent

> If you left the flow, return by navigating to **Projects** > Select the relevant Project > Click **View Details**. 

1. Click **Create Sandbox**.

> You may need to reload the page before the next step.

2. Click **Edit in Sandbox**. 

> A vanilla VS code instance will open.

3. Click the Kangaroo, the final icon on the LHS menu bar.

> You are now ready to build out your app with the AI assistant.

## What next?

- Learn how to [configure Git](./sandbox-support.md#configure-git) in your VS code instance to push code changes back to GitHub
- Consider creating your own boilerplate app or leveraging existing repos for other frameworks as a starting point to work with the AI assistant