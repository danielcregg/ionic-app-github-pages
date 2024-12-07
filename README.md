# Ionic Angular PWA GitHub Pages Deployment

Deploy any Ionic Angular app as a Progressive Web App (PWA) to GitHub Pages with just one file. Works with both standalone and module-based apps.

Live demo: [Ionic PWA Demo](https://danielcregg.github.io/ionic-app-github-pages-pwa/)

## 🚀 One-Step Setup

1. **Add the workflow file to your repo**:
   ```bash
   curl -o .github/workflows/deploy.yml https://raw.githubusercontent.com/danielcregg/ionic-app-github-pages-pwa/main/.github/workflows/deploy.yml
   ```

2. **Enable GitHub Pages**:
   - Go to repository **Settings** → **Pages**
   - Select **GitHub Actions** as source

That's it! Push your changes and your app will deploy to `https://<username>.github.io/<repo-name>/`

## ✨ Features

- 🔄 **Zero Config**: Works automatically with any Ionic Angular project
- 📱 **Full PWA Support**: Service worker, offline mode, installable
- 🎯 **Smart Detection**: Finds your project whether in root or subdirectory
- 🏃 **Fast Builds**: Implements intelligent caching for quick deployments
- 🔧 **Auto-configuration**: Sets up all PWA requirements automatically
- 🖼️ **Icon Generation**: Creates all required PWA icons automatically
- 📑 **Deep Linking**: Handles routing for both tabbed and non-tabbed apps
- 🔒 **Cache Control**: Implements build caching for:
  - npm packages
  - Angular build cache
  - Ionic build artifacts
  - Platform and plugin files

## 📁 Supported Project Structures

Works with any of these layouts:

