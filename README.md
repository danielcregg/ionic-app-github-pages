# Ionic App GitHub Pages Deployment Example

Check out this Ionic App deployed to GitHub pages. [My Ionic App](https://danielcregg.github.io/ionic-app-github-pages/)
Please note it is also deployed as an installedable PWA.

## Quick Usage Steps

1. **Add Ionic Project**: Push your Ionic project (with `ionic.config.json`) to a GitHub repository.
2. **Add Workflow File**: Copy the provided `.github/workflows/deploy.yml` file into your repo.
3. **Enable Pages Once**: In **Settings > Pages**, select "GitHub Actions" as the source and save.
4. **Push Changes**: Commit and push your code to `main`. The workflow will automatically build and deploy your app.
5. **View Your Site**: Access your app at `https://<your-username>.github.io/<your-repo-name>/`.

---

## Overview

This repository’s workflow demonstrates how to build and deploy an Ionic Angular application to GitHub Pages using GitHub Actions. By using a dynamic approach, you can place your Ionic project anywhere in the repository (at the root or in a subdirectory), and the workflow will:

- Detect the Ionic project directory by searching for `ionic.config.json`.
- Dynamically set the `--base-href` based on your repository name.
- Automatically build, upload, and deploy the `www` folder to GitHub Pages.

## Prerequisites

1. **Public Repository**:  
   Easiest with public repos. For private repos, ensure you have a GitHub plan that supports private Pages.

2. **Ionic Project Structure**:  
   Your Ionic project should have a `ionic.config.json` file. Running `ionic build` locally should produce a `www` folder with your production-ready files.

3. **Node and Ionic CLI**:  
   The workflow installs Node.js 18 and the Ionic CLI in the GitHub Actions runner. You don’t need to set this up yourself.

## How the Workflow Works

1. **Detect Project Directory**:  
   The workflow finds `ionic.config.json` and sets that directory as the working directory. If none is found, it defaults to the repository’s root.

2. **Dynamic Base Href**:  
   The workflow extracts your repository name from `GITHUB_REPOSITORY` and uses it as the `--base-href`. For a repo named `ionic-app-github-pages`, the final site URL is `https://<username>.github.io/ionic-app-github-pages/`.

3. **Build and Deploy**:  
   After building, `actions/upload-pages-artifact` packages the `www` folder, and `actions/deploy-pages` deploys it to GitHub Pages.

4. **One-Time Pages Enablement**:  
   You must enable GitHub Pages to use GitHub Actions once manually: **Settings > Pages > Build and Deployment**, select **GitHub Actions**, and save.

## Detailed Steps for Your Own Repo

1. **Add Your Ionic Project to a Repo**:  
   Push your Ionic project to a new or existing GitHub repository. Make sure `ionic.config.json` is present at the root or a subdirectory.

2. **Add the Workflow File**:  
   Create `.github/workflows/deploy.yml` in your repository if it doesn’t exist. Copy the provided workflow into this file. Example structure:
   ```
   .
   ├─ .github/
   │  └─ workflows/
   │     └─ deploy.yml
   ├─ ionic.config.json
   ├─ package.json
   ├─ src/
   └─ ...
   ```

   If your project is in `my-ionic-app/`, for instance:
   ```
   .
   ├─ .github/
   │  └─ workflows/
   │     └─ deploy.yml
   ├─ my-ionic-app/
   │  ├─ ionic.config.json
   │  ├─ package.json
   │  ├─ src/
   │  └─ ...
   ```

   No edits to the workflow are necessary; it will automatically detect the project directory.

3. **Manually Enable GitHub Pages Once**:  
   Go to **Settings > Pages** in your repository and set "GitHub Actions" as the source and save. You only need to do this once.

4. **Push to Main**:  
   Commit and push your code to the `main` branch. The workflow will:
   - Locate the project directory.
   - Install dependencies and run `ionic build` with the correct `--base-href`.
   - Upload and deploy the `www` folder.

5. **Visit Your Site**:  
   Once the action completes, your site will be live at:
   ```
   https://<username>.github.io/<repo-name>/
   ```

## Adapting for Different Project Structures

- **Project at Root**:  
  If `ionic.config.json` is at the root, it just builds from `.`.
  
- **Project in a Subdirectory**:  
  If the file is in `my-ionic-app/`, the workflow will detect that and run build commands there.

## Troubleshooting

- **Blank Page or 404 Errors**:  
  Ensure the `--base-href` matches the repo name. The workflow does this automatically, but verify that the final URL corresponds to your repo’s name.

- **Build or Permissions Issues**:  
  Check your `ionic build` process locally. Also, ensure you did the one-time manual setup in **Settings > Pages**.

## Conclusion

With this workflow, you can:

- Easily drop a ready-to-go CI/CD pipeline into any Ionic project.
- Automatically adapt to different directory structures and repository names.
- Enjoy continuous deployment to GitHub Pages once the initial setup is complete.

