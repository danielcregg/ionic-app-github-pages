# Ionic App GitHub Pages Deployment Example

Check out this Ionic App deployed to GitHub Pages: [My Ionic App](https://<your-username>.github.io/<your-repo-name>/)

Please note it is also deployed as an installable Progressive Web App (PWA).

## Quick Usage Steps

1. **Add Ionic Project**: Push your Ionic project (with `ionic.config.json`) to a GitHub repository.

2. **Add Workflow File**: Copy the provided `.github/workflows/deploy.yml` file into your repository.

3. **Enable GitHub Pages**:
   - Go to **Settings > Pages** in your repository.
   - Under **Build and deployment**, select **GitHub Actions** as the source and save.

4. **Push Changes**: Commit and push your code to the `main` branch. The workflow will automatically build and deploy your app as a PWA to GitHub Pages.

5. **View Your Site**: Access your app at `https://<your-username>.github.io/<your-repo-name>/`.

---

## What's New

The workflow has been updated to fix issues with PWA functionality. The following changes have been made:

- **Service Worker Registration Fix**: Adjusted `index.html` and `ngsw-config.json` to correctly register and configure the service worker when the app is served from a subdirectory.
- **Relative Paths in Service Worker Config**: Updated `ngsw-config.json` to use relative paths, ensuring assets are correctly cached.
- **Build Configuration Updates**: Modified `angular.json` and the build command to include `baseHref` and `deployUrl`, allowing the app to work properly from the GitHub Pages subdirectory.

## Detailed Steps for Your Own Repo

1. **Add Your Ionic Project to a Repo**:

   Push your Ionic project to a new or existing GitHub repository. Ensure `ionic.config.json` is present at the root or within a subdirectory.

2. **Add the Workflow File**:

   Create a `.github/workflows/deploy.yml` file in your repository if it doesn’t exist. Copy the updated workflow into this file. Example structure:

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

   **Note**: Ensure the workflow file reflects the changes made to fix the PWA issues.

3. **Update `angular.json`**:

   Ensure that your `angular.json` includes the following in the production build configuration:

   ```json
   // ...existing configurations...
   "configurations": {
     "production": {
       // ...existing code...
       "baseHref": "/<your-repo-name>/",
       "deployUrl": "/<your-repo-name>/",
       "serviceWorker": true,
       "ngswConfigPath": "ngsw-config.json"
       // ...existing code...
     }
   }
   // ...existing configurations...
   ```

4. **Update `ngsw-config.json`**:

   Ensure paths are relative in `ngsw-config.json`:

   ```json
   // ...existing code...
   "index": "index.html",
   "assetGroups": [
     {
       // ...existing code...
       "resources": {
         "files": [
           "favicon.ico",
           "index.html",
           "manifest.webmanifest",
           "/*.css",
           "/*.js",
           "!ngsw-worker.js"
         ]
         // ...existing code...
       }
     },
     // ...existing asset groups...
   ]
   // ...existing code...
   ```

5. **Update `src/index.html`**:

   Modify `index.html` to correctly reference the manifest and register the service worker:

   ```html
   <!-- ...existing code... -->
   <link rel="manifest" href="manifest.webmanifest" crossorigin="use-credentials" />
   <!-- ...existing code... -->
   <script>
     if ('serviceWorker' in navigator) {
       window.addEventListener('load', function() {
         navigator.serviceWorker.register('ngsw-worker.js')
           .then(function(registration) {
             console.log('Service Worker registered:', registration.scope);
           })
           .catch(function(error) {
             console.error('Service Worker registration failed:', error);
           });
       });
     }
   </script>
   <!-- ...existing code... -->
   ```

6. **Ensure Production Environment is Correctly Set**:

   In `src/environments/environment.prod.ts`, ensure that `production` is set to `true`:

   ```typescript
   // filepath: /src/environments/environment.prod.ts
   export const environment = {
     production: true
   };
   ```

7. **Modify the Build Command in the Workflow**:

   Ensure that the build command in your workflow includes `--deploy-url`:

   ```yaml
   # ...existing workflow steps...
   - name: Build the Ionic PWA
     run: |
       cd $APP_PATH
       # Ensure production environment
       mkdir -p src/environments
       echo "export const environment = { production: true };" > src/environments/environment.prod.ts
       # Build the app with PWA support
       ionic build --configuration=production -- --base-href="/${{ env.REPO_NAME }}/" --deploy-url="/${{ env.REPO_NAME }}/" --service-worker=true
       # Post-build fix: Create 404.html
       cd www
       cp index.html 404.html
   # ...remaining workflow steps...
   ```

8. **Push to Main**:

   Commit and push all changes to the `main` branch. The workflow will:

   - Install dependencies.
   - Build the app with PWA support.
   - Deploy the `www` folder to GitHub Pages.

9. **Visit Your Site**:

   Once the action completes, your PWA will be live at:

   ```
   https://<your-username>.github.io/<your-repo-name>/
   ```

## Troubleshooting

- **Service Worker Not Registering**:

  Ensure that the paths in `ngsw-config.json` are relative and that the service worker registration script in `index.html` uses the correct path.

- **PWA Assets Not Loading**:

  Check that `baseHref` and `deployUrl` are correctly set in `angular.json` and in the build command within your workflow.

- **404 Errors on Reload**:

  Make sure to include a `404.html` file in your `www` directory. The workflow handles this by copying `index.html` to `404.html` after the build.

- **Manifest or Service Worker Issues**:

  Verify that `manifest.webmanifest` is correctly referenced in `index.html` and that the service worker is properly registered. Check the console for any errors related to the manifest or service worker.

## Conclusion

With these updates, your Ionic app should now function correctly as a PWA when deployed to GitHub Pages. The service worker will cache assets appropriately, and users can install your app to their devices.

---

**Note**: Replace `<your-username>` and `<your-repo-name>` with your actual GitHub username and repository name.

---

By following these steps, you can utilize the updated workflow to deploy your Ionic PWA to GitHub Pages successfully.

