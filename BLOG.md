---
author: Richard Flosi
title: "Getting Started with Netlify: Creating your first Netlify Site"
description: Create a git workflow CI/CD pipeline with Netilfy that updates when you push to git.
excerpt: TODO - This is what shows up on the blog landing page under your title. Can be your first paragraph, if it describes the post. The goal is to encourage readers to click through.
categories:
  - Front-End
  - Serverless
  - Git-Workflow
  - CI/CD
  - Web
social_media_img: netlify-and-github.png
---

Want to create a deployment workflow that is as easy as `git push`?
To follow along with this post please sign up for free [GitHub](https://github.com/signup) and [Netilfy](https://app.netlify.com/signup) accounts or have your credentials ready.
For detailed instructions on installing `git` see [GitHub's Set up Git documentation](https://docs.github.com/en/get-started/quickstart/set-up-git).

# Create a GitHub repository for your project

To get started you need to [create a new repository](https://github.com/new) in GitHub.
For detailed instructions see [GitHub's Create a repo documentation](https://docs.github.com/en/get-started/quickstart/create-a-repo).
Start by naming and creating an empty repository.
The repository for this blog post can be found at [https://github.com/BNR-Developer-Sandbox/BNR-blog-netlify-101](https://github.com/BNR-Developer-Sandbox/BNR-blog-netlify-101).
Use `git clone` to clone your new repository to your local computer in your terminal.
For detailed instructions see [GitHub's Cloning a repository documentation](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository-from-github/cloning-a-repository).

# Netlify CLI

In order to login to Netlify and link our GitHub repository to a new Netlify Site, you'll need to install the `netlify-cli`.
The [Netlify CLI](https://docs.netlify.com/cli/get-started/) let's you run a local development server, run local builds, and deploy your site.
Next you'll configure a `package.json` file to install `netlify-cli` and define `scripts` to run commands.

## Create a package.json File

Create a new file in your empty repository called `package.json` and save the following content to it:

```
{
  "devDependencies": {
    "netlify-cli": "6.8.9"
  },
  "scripts": {
    "netlify-login": "netlify login",
    "netlify-logout": "netlify logout",
    "netlify-link": "netlify link",
    "netlify-status": "netlify status",
    "start": "netlify dev"
  }
}
```

The version used above is the current version as of this writing.
Please update to the [latest version](https://www.npmjs.com/package/netlify-cli) of `netlify-cli`.

In `package.json` you defined your development dependency on `netlify-cli` and configured a set of scripts to run `netlify` commands.
Next you'll configure a `netlify.toml` file to define your `build` and `dev` settings.

## Create a netlify.toml File

Create a new file in the root of your repository called `netlify.toml` and save the following content to it:

```
[build]
publish = "dist/"
command = "mkdir -p dist/ && cp -R src/* dist/"

[dev]
publish = "src/"
```

The `[build]` section tells Netlify how to build your project.
In this case your build step is simply creating a `dist/` directory and copying files from the `src/` to `dist/` as defined in the `command` setting.
The value used in `command` can be replaced by the build step for the framework you are using.
The `publish` setting defines where Netlify will serve static content from.
Set `publish` to the output directory for your build `command`.

The `[dev]` section tells Netlify how to run your project for local development.
Just like in the `[build]` section, the `publish` setting defines where Netlify will serve static content from.
This section also has a `command` setting can be set to run the local development server for your framework.
In this case you are only serving up static files and Netlify will handle that for you by default.

## Create src/ Directory

Since we configured Netlify to serve development content from a `src/` directory you can create that directory with the `mkdir src/` command in your terminal or through your code editor.
Next create a new HTML in `src/index.html` with and save the following content to it:

```
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Getting Started with Netlify: Creating your first Netlify Site</title>
</head>

<body>
  <h1>Getting Started with Netlify: Creating your first Netlify Site</h1>
  <p>Hello, world!</p>
</body>

</html>
```

In this step you've created a simple `Hello, world!` webpage that you can deploy to Netlify.

## Create a .gitignore File

Before installing your dependencies you should create a `.gitignore` file in the root directory and save the following content to it:

```
node_modules
dist
```

This will prevent your `node_modules` and `dist` directories from being included in your `git commit` later.

## Git Commit and Git Push

You now have all the files you need to deploy to Netlify.
Take a moment to commit our changes and push them up to GitHub with the following commands in your terminal:

```
git commit -m "My first Netlify Site"
git push origin master
```

## Install your dependencies

Now you are ready to run `npm install` to install `netlify-cli`.
This step will create the `node_modules` directory for your dependencies.
Once `npm install` completes it will output a summary of the installation.

## Start your local development server

Start `netlify dev` by running your `npm start` command you defined in `package.json`.
Netlify Dev will detect that no framework was defined and serve static content from your `publish` directory which is `src/` in this case.
Netlify Dev will print out the local development url, open your web browser with that url, and continuing running in your terminal.
You should now see the rendered version of your `index.html` file in your browser with your "Hello, world!" message.

At this point you have a local development environment running, but have not yet connected your repository to Netlify.
Kill the local development server by holding the "control" key and pressing the "c" key, also referred to as `^C`.

## Netlify Login

Next login to Netlify from your terminal with `npm run netlify-login` which will run `netlify login` as defined in your `package.json` file.
This will open a browser window on Netlify where you can complete the login process and authorize the connection between Netlify CLI and Netlify.

## Create the Netlify Site

From your [Netlify Team Overview](https://app.netlify.com/) screen click [New site from Git](https://app.netlify.com/start) and following the instructions.
First you'll be asked to "Connect to your Git provider".
Click the "GitHub" button.
This will open an new window to authorize the connection between Netlify and GitHub.
Once authorized, Netlify will ask you to "Pick a repository".
Find and select the repository you created for this project.
Now you will be asked to configure the "Site settings, and deploy!".

## Netlify Link

Now it's time to link your GitHub repository to a new Netlify Site.
Run `npm run netlify-link` in your terminal and follow the prompts to create the new site.
`netlify link` will connect the current folder to a site on Netlify.
When prompted, select `Use current git remote origin` which will be the default selection and press the "Enter" key.

# Typical Workflow

In most cases your git workflow would involve creating a Pull Request to be reviewed by a teammate.
You may also have feature or staging branches.
In other words, in most cases you don't want to push directly to production with every change.
This is where [Netlify Deploy Previews](https://www.netlify.com/products/deploy-previews/) come in.
Most likely you'll also need an API layer which can be implemented with [Netilfy Functions](https://www.netlify.com/products/functions/).
And your API layer might need to use API keys which can be kept secret in [Build environment variables](https://docs.netlify.com/configure-builds/environment-variables/).
