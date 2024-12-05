# Ionic App GitHub Pages Deployment Example

This repository provides a fully functional example of using GitHub Actions to build and deploy an Ionic Angular application to GitHub Pages. By following the steps outlined here, you can easily adapt the included workflow for any of your own Ionic projects.

## Overview

**What This Repository Demonstrates:**

- Automatically detecting the Ionic project directory by searching for `ionic.config.json`.
- Installing dependencies and building the Ionic app using a dynamically determined base URL (based on the repository name).
- Uploading the production build (`www` folder) as an artifact.
- Deploying directly to GitHub Pages from the `main` branch using GitHub Actions.

**Key Benefits:**

- You do not need to hardcode your repository name in the workflow.
- Works even if your Ionic project is in a subdirectory.
- Once GitHub Pages is manually enabled (one-time step), all future deployments are automatic on pushes to `main`.

## Prerequisites

1. **Public Repository**:  
   GitHub Pages works easiest on public repositories. For private repositories, ensure your plan supports private Pages.
   
2. **Ionic Project Structure**:  
   - Your Ionic project should contain a `ionic.config.json` file.  
   - Running `ionic build` in the project directory should produce a `www` folder with `index.html` and other build artifacts.

3. **Node and Ionic CLI**:  
   The workflow uses Node.js 18 and installs the Ionic CLI globally in the build environment. No additional global configuration is needed on your part.

## How This Works

1. **Detecting the Project Directory**:  
   The workflow searches your repository for `ionic.config.json`. If it’s found in a subdirectory (e.g., `my-ionic-app/`), the workflow uses that directory as the working directory for installing dependencies and building. If not found, it assumes the root directory is the project directory.

2. **Dynamic Base Href for Routing**:  
   The workflow extracts your repository name at runtime and sets the Angular/Ionic build’s `--base-href` accordingly. For a repository named `my-ionic-app`, it will use `/my-ionic-app/`. This ensures that when served at `https://<username>.github.io/<repo-name>/`, all scripts and assets resolve correctly.

3. **Building and Deploying**:  
   After installing dependencies and building, the workflow uses:
   - `actions/upload-pages-artifact` to package the `www` folder
   - `actions/deploy-pages` to deploy the packaged site to GitHub Pages

4. **GitHub Pages Setup**:  
   You will need to enable GitHub Pages to use GitHub Actions as the build source **once** from your repository’s **Settings > Pages**. After that initial setup, every push to `main` will trigger a rebuild and redeployment automatically.

## Step-by-Step Instructions to Use This Workflow in Your Own Repo

1. **Add Your Ionic Project to a Repo**:  
   If you haven’t already, create a GitHub repository and add your Ionic project code. Ensure the `ionic.config.json` file is present at the root or in a subdirectory.

2. **Copy the Workflow File**:  
   In your repository, create the directory `.github/workflows` if it doesn’t exist. Add the provided `deploy.yml` (or rename it as desired) to `.github/workflows/deploy.yml`.

   **Example layout:**
