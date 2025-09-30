# Vercel migration steps

## Step 1. Audit and prepare your Vercel project

1. Record all:

- Environment variables
- Build commands
- Run commands
- Custom (public) Dockefile

per your Vercel dashboard.

> For example, note your framework’s build steps (`npm run build` or similar).

 <!-- Feature request output directory (dist, out, etc.). Notion -->

## Step 2. Login to AutoGen and connect the resources

1. Create an account using email.

<!-- link -->

2. Connect your GitHub repository or link the Dockerfile through the platform’s integration steps.

<!-- link -->

3. Verify build settings (such as commands, environment variables).

<!-- feature request eg GitHub Secrets -->

## Step 3. Configure and deploy a test project

Deploy a test instance and verify functionality, performance, and build logs.

## Step 4. Cutover planning and DNS updates

1. Gradually migrate non-critical projects for testing.

2. Update DNS to point to the permanent endpoint provided in the project.

<!-- link -->

> Coming soon, support for custom domains.

3. Monitor for regressions and reach out to support if needed.

<!-- link -->

