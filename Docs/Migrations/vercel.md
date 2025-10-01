# Vercel migration steps

## Step 1. Audit and prepare your Vercel project

1. Record any that apply:

- Environment variables
- Build commands
- Run commands
- Custom (public) Dockerfile

per your Vercel dashboard.

> For example, note your framework’s build steps (`npm run build` or similar).

 <!-- Feature request output directory (dist, out, etc.) -->

## Step 2. Login to AutoGen and connect the resources

1. Create an account using email/Gmail sign in.

<details>
  <summary>Sign up walkthrough</summary>

- [Click for walkthrough](https://app.guidemaker.com/guide/06ae9806-c6cb-46ad-b2f9-d3d632fa1585)

- [Click for video](https://github.com/NodeOps-app/beta-deploy/issues/4#issue-3429991454)

</details>

2. [Connect your GitHub repository/s](../GitHub-Integration/github-support.md#set-up-github-integration) or [link the Dockerfile](../Docker-Integration/docker-support.md#deploy-a-docker-image) through the platform’s integration steps. 

3. Verify build settings (such as commands, environment variables).

<!-- feature request secret management eg GitHub Secrets -->

## Step 3. Configure and deploy a test project

Deploy a test instance and verify functionality, performance, and build logs.

## Step 4. Cutover planning and DNS updates

1. Gradually migrate non-critical projects for testing.

2. Update DNS to point to the [permanent endpoint](../Projects/functions.md#share-project-copies-deployed-projects-public-endpoint
) provided in the project.

> Coming soon, support for custom domains.

3. Monitor for regressions and reach out to support if needed.

<details>
  <summary>Support walkthrough</summary>

- [Click for walkthrough](https://app.guidemaker.com/guide/0b3580de-36a4-4e13-9b3f-b78533d20708)

- [Click for video](https://github.com/NodeOps-app/beta-deploy/issues/4#issuecomment-3311172755)

</details>

