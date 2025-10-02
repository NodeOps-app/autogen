# GitHub integration

The tool integrates with GitHub to allow you to leverage existing repositories.

> Please ensure you respect the licensing of third parties.

This page covers: 

- [Supported frameworks](#supported-frameworks) 
- [Connecting to GitHub](#set-up-github-integrtation)
- [Amending which repositories are connectred](#amend-github-integrtation)

## Supported frameworks

- Go
- Next.js
- Python
- SvelteKit
- Vite

You may also select **Other/Unknown** as long as you provide the **Dockerfile** in the repository.

## Browse the list of walkthroughs

### Set up GitHub integration

You can connect your GitHub and provide access to all or selected repositories.

<details>
  <summary>Show me</summary>

1. Logged into the app, click on **New Project**.
2. Click on **Import Github Repository**.
3. Decide between **All repositories** and **Only select repositories**.
4. If selecting, choose the relevant repos (you can always configure the access from GitHub again later).
5. Click on **Install**.
6. Verify according to your GitHub verification method.

> You may need to re-login if this issue was not fixed yet!

![](../../Static/Gifs/autogen-all-repos-connect.gif)

</details>

### Deploy a GitHub-based project

You may deploy a project from a linked respository.

<details>
  <summary>Show me</summary>

1. Logged into the app, click on **New Project**.
2. Choose the relevant repo.
3. Click on **Install**.

> You may need to re-login if this issue was not fixed yet!

![](../../Static/Gifs/autogen-all-repos-connect.gif)

</details>

3. Work with AI in sandbox

<!-- todo -->

### Amend GitHub integration

From your GitHub account, you may revoke the app's access to your account or modify which repositories it can access.

<details>
  <summary>Show me</summary>

1. Logged into your GitHub > Your profile pic > Settings → [Integrations] Applications.
2. Select the tab **Installed GitHub Apps** > NodeOps Build0 > click Configure
3. You may:
- Suspend your installation: click **Suspend**
- Uninstall the app: click **Uninstall**
- Curate the repository access list

![Uninstall app](./Static/Gifs/disconnect-autogen-app.gif)

</details>


