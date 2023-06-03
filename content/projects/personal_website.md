---
date    : 2023-06-03
title   : Personal Website With Hugo and Github Pages
toc     : true

---

## Introduction

The objective of this project is to showcase the architecture of this website, which was crafted with the tools of Hugo and GitHub Pages. Hugo serves as a swift, open-source static site generator while GitHub Pages provides a platform for hosting and publishing static sites directly from a GitHub repository. This integration streamlines the management of the website and guarantees a secure environment for its online availability.

## Building the Website

### Installing Hugo

Hugo can be conveniently installed across a range of platforms utilizing the platform-specific package manager. For instance, homebrew is the preferred choice for macOS.
```
z@Pseudonym ~ % brew install hugo
```

Verifying the successful installation of Hugo is a straightforward task. The following command should return the version of Hugo and confirm that it is indeed in the `extended` state:
```
z@Pseudonym ~ % hugo version
>> hugo v0.105.0+extended darwin/arm64 BuildDate=unknown
```

### Creating a New Website Environment
With a single command, Hugo can be instructed to establish a fresh website environment in the existing directory (home directory in this example):
```
z@Pseudonym ~ % hugo new site myWebsite
```
This operation generates a new directory encompassing all necessary files and subdirectories pertinent to the Hugo website:
```
z@Pseudonym myWebsite % tree
.
├── archetypes
│   └── default.md
├── config.toml
├── content
├── data
├── layouts
├── public
├── static
└── themes
```

### Adding a Theme
Following the creation of the website's environment, the next step is to select a theme, available through the Hugo themes gallery. For this particular website, the theme 'hello-friend-ng' is chosen, lauded for its minimalistic design and incorporated dark and light modes. To set this theme, the author's GitHub repository must be cloned into the themes folder within the new Hugo directory.
```
z@Pseudonym myWebsite % git clone https://github.com/rhazdon/hugo-theme-hello-friend-ng.git themes/hello-friend-ng
```

Having stored the theme as `hello-friend-ng` in the themes folder, the next step is to define this as the theme of choice within the `config.toml` file.
```
z@Pseudonym myWebsite % echo theme = \"hello-friend-ng\" >> config.toml
```

To ensure the process has been executed correctly thus far, the website can be previewed in the browser after running it locally through Hugo. By default, the website can be accessed on port 1313, or `localhost:1313`.
```
z@Pseudonym myWebsite % hugo server
```

### Adding Content
Adding blog posts can be achieved either by manually incorporating a markdown file into the contents directory subfolder or by allowing Hugo to create one based on the pre-defined archetype template.
To create a new Markdown file named `creatingWebsite.md` in the designated `projects` subdirectory within the `content` folder, the following command needs to be run:
```
z@Pseudonym myWebsite % hugo new projects/creatingWebsite.md
```

### Customizing the Design
Access to the full source code of the Hugo template in the `themes` folder enables straightforward customization. By adjusting the SCSS and HTML files, the website's appearance can be altered or specific elements can be added or removed, according to the specific requirements.

## Setting up the GitHub Repository

### Creating a Repository
To utilize GitHub Pages for hosting a website, it's required to establish a repository adopting the naming scheme `username.github.io`. For instance, the repository dedicated to this project is dubbed `zPseudonym.github.io`.
Once the repository has been successfully set up, it's crucial to connect the local Hugo directory for the website with the remote GitHub repository. This can be accomplished through the implementation of the basic commands that are typically employed after the creation of a new GitHub repository.
```
z@Pseudonym myWebsite % git init  
z@Pseudonym myWebsite % git add .  
z@Pseudonym myWebsite % git commit -m "first commit"  
z@Pseudonym myWebsite % git branch -M main  
z@Pseudonym myWebsite % git remote add origin https://github.com/zPseudonym/zPseudonym.github.io
z@Pseudonym myWebsite % git push -u origin main
```

### Creating a Workflow in GitHub Actions
To streamline and automate the building and deployment process of the website, one can leverage GitHub Actions, GitHub's CI/CD tool. This helps to manage the recurring task by incorporating a specific workflow within a file housed in the `.github` directory.
```yaml
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ["main"]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow one concurrent deployment
concurrency:
  group: "pages"
  cancel-in-progress: true

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.102.3
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_Linux-64bit.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: recursive
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v2
      - name: Build with Hugo
        env:
          # For maximum backward compatibility with Hugo modules
          HUGO_ENVIRONMENT: production
          HUGO_ENV: production
        run: |
          hugo \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v1

```
Each time a modification is committed to the GitHub repository, the GitHub Actions workflow automatically undertakes the responsibility of building and updating the website, thereby ensuring a seamless and efficient process.