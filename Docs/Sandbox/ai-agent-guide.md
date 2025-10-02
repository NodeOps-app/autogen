# Guide to use the AI agent

> Currently, AutoGen does not allow you to start with an empty repository. It anticipates deploying an app with a provided Dockerfile or by creating one.

Follow along with this tutorial to get started with a Next.js app and get coding assistant from the AI agent.

You will:

- Create a boilerplate app in a GitHub repository
- Deploy that app with AutoGen
- Launch a sandboxed instance of the app which provides an AI assistant

![ai agent guide](../../Static/Gifs/guide-ai-agent.gif)

## Step 1: Create a Next.js app

In GitHub, create a 3 page app:

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
  <summary>Dockerfile</summary>

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

## Step 2: Deploy your Next.js app

1. Logged into AutoGen, browse for the GitHub repo that contains your app.
> If required, follow the [GitHub Get Started](../GitHub-Integration/github-support.md#set-up-github-integration), then return here.
> If required, follow the [GitHub reconfiguration guide](../GitHub-Integration/github-support.md#amend-github-integration) to alter which repositories are available from AutoGen, then return here.
2. Configure the deployment, this guide uses:
- Branch: main
- Framework: Next.js
- Port: 3000
- Name: {assign unique name}
- My project has a Dockerfile: toggle on
click **Deploy**.

> You may verify the build by clicking **Visit Project** to see the **Hello World** page.

## Step 3: Launch sandbox and AI agent

1. Click **Create Sandbox**.

> You may need to reload the page before the next step.

2. Click **Edit in Sandbox**. 

> A vanilla VS code instance will open.

3. Click the final icon on the LHS menu bar, a kangaroo.

> You are now ready to build out your app with the AI assistant.

