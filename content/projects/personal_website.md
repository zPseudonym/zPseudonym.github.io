---
date    : 2022-12-31
title   : Personal Website With Hugo and Github Pages
toc     : true
tech    : ["Hugo", 
            "HTML", 
            "CSS", 
            "Markdown",
            "Git", 
            "GitHub", 
            "GitHub Pages", 
            "GitHub Actions"]
---

## Introduction

This project is about how this website was built using [Hugo](https://gohugo.io/) and [GitHub Pages](https://pages.github.com/). Hugo is a fast open-source static site generator and GitHub Pages allows hosting and publishing of static sites from a GitHub repository. The combination of both allows fast and easy management of the website while having a secure environment to make the website available from.

## Building the Website

### Installing Hugo

Hugo can be easily installed on different platforms using their respective package manager; e.g. homebrew for macOS:
```
z@Pseudonym ~ % brew install hugo
```

The installation of Hugo can be verified with the following command, which should output its version as well as stating that it is `extended`:
```
z@Pseudonym ~ % hugo version
>> hugo v0.105.0+extended darwin/arm64 BuildDate=unknown
```

### Creating a New Website Environment
The following command tells Hugo to create a new environment for a website in the current directory (home directory in this example):
```
z@Pseudonym ~ % hugo new site myWebsite
```
The newly created directory contains all relevant files and subdirectories for the Hugo website:
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
After creating the environment for the new website, a theme should be set, which can be found in the Hugo [themes gallery](https://themes.gohugo.io/). For this website, the theme [hello-friend-ng](https://themes.gohugo.io/themes/hugo-theme-hello-friend-ng/), which offers a simple design as well as an integrated dark and light mode, was selected. The author's GitHub repository needs to be cloned into the `themes` folder of the new Hugo directory.
```
z@Pseudonym myWebsite % git clone https://github.com/rhazdon/hugo-theme-hello-friend-ng.git themes/hello-friend-ng
```

Now that the theme is saved as `hello-friend-ng` in the themes folder, it needs to be set as the theme to use in the `config.toml` file.
```
z@Pseudonym myWebsite % echo theme = \"hello-friend-ng\" >> config.toml
```

To confirm all steps have been correctly done so far, the website can be viewed in the browser after Hugo runs it locally with the following command. By default, the website is accessible on port 1313; i.e. `localhost:1313`.
```
z@Pseudonym myWebsite % hugo server
```

### Adding Content
Blog posts can be added either by manually adding a markdown file to the subfolder in the contents directory or by letting Hugo create one based on the defined archetype template.  
For creating a new Markdown file called `creatingWebsite.md` in the specified subdirectory `projects` inside of the `content` folder, the following command needs to be run:
```
z@Pseudonym myWebsite % hugo new projects/creatingWebsite.md
```

### Customizing the Design
As the entire source code of the Hugo template is available in the `themes` folder, changes to it can easily be made by adjusting the SCSS and HTML files, e.g. for editing the appearance or for adding/removing certain elements of the website.

## Setting up the GitHub Repository

### Creating a Repository
For GitHub to host a website with GitHub pages, a repository with the naming convention `username.github.io` needs to be created. In this case, the [repository](https://github.com/zPseudonym/zPseudonym.github.io) for this project is called `zPseudonym.github.io`.  
With the repository created, the local Hugo directory for the website needs to be linked to the remote repository on GitHub by using the basic commands after freshly creating a repository.
```
z@Pseudonym myWebsite % git init  
z@Pseudonym myWebsite % git add .  
z@Pseudonym myWebsite % git commit -m "first commit"  
z@Pseudonym myWebsite % git branch -M main  
z@Pseudonym myWebsite % git remote add origin https://github.com/zPseudonym/zPseudonym.github.io
z@Pseudonym myWebsite % git push -u origin main
```

### Creating a Workflow in GitHub Actions
For automating the process of building and deploying the website, GitHub's CI/CD tool [GitHub Actions](https://github.com/features/actions) can be used to take care of this repetitive task by adding the following [workflow](https://github.com/marketplace/actions/hugo-setup) in a file inside the `.github` directory.
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
Every time a new change is pushed to the GitHub repository, the GitHub Actions workflow is triggered and it automatically builds and publishes the website.

## What I Have Learned From This Project

During this project, I learned what steps it takes to create and host a personal website on the internet, allowing me to share content to the public. It also strengthened my skills in version control using git as well as repository management via GitHub. Furthermore, by tweaking the pre-selected design template, I used HTML and CSS to customize the website to my liking. By using GitHub Actions, I also had the chance to dive in the DevOps world by setting up a workflow to automate the building and deployment processes of the website.