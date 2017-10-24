---
date: 2017-07-20T00:11:02+01:00
title: Documentation
type: index
weight: 90
---

As part of the Open Developer Platform, the Foundation provides a simple, self-managed solution for project documentation, leveraging:

1. Hugo - a static website engine
2. Github Pages - a platform to host and serve static HTML
3. Travis CI - to capture source code changes, generate static HTML and push it back to Github

## Getting Started
1. [Install Hugo](https://gohugo.io/#action)
2. [Configure Hugo](https://www.youtube.com/watch?v=w7Ft2ymGmfc) (70 seconds video tutorial)
3. Choose a [Hugo template](https://gohugo.io/templates/overview/) that's best for your needs

To test that everything is functioning correctly, you can run your website locally using the [`server` command](https://gohugo.io/commands/hugo_server/).

## Configuration

The next step is to enable Github Pages; by default, we choose to publish GitHub Pages site from a [/docs folder on master branch](https://help.github.com/articles/configuring-a-publishing-source-for-github-pages/#publishing-your-github-pages-site-from-a-docs-folder-on-your-master-branch), although there are other [options available](https://gohugo.io/hosting-and-deployment/hosting-on-github/).

## Go Live
Make sure that `config.toml` - located at the root of your Hugo project - defines the following variables:
```
baseurl = "https://symphonyoss.github.io/<github_project_name>"
publishDir = "docs"
```

The `hugo` command will generate static HTML content into the `publishDir` folder, as part of the GitHub project identified by `baseurl`; when changes are pushed to Github, HTML content will become publicly available on `https://symphonyoss.github.io/<github_project_name>`.

## Automated build
In order to automatically validate and publish changes to the website, Travis CI can be easily configured to run the `hugo` command upon each commit and push changes to the `docs/` folder.

Below is a `.travis.yml` commented with the different items needed (taken from the Symphony [Integrations-Docs project](https://github.com/symphonyoss/Integrations-Docs/blob/master/.travis.yml)).

```
# Define an encrypted version of GIT_TOKEN
# check https://docs.travis-ci.com/user/encryption-keys or
# https://docs.travis-ci.com/user/environment-variables
- secure: "..."
 
 
# Define where to push the folder to
- GIT_ORG: <Github user or organisation>
- GIT_REPO: <Github project name>
- GIT_USER_NAME: <name used as git commit author>
- GIT_USER_EMAIL: <email used as git commit author>
- FILES_TO_PUSH: docs
 
 
# Install hugo CLI tool
before_install: "wget https://github.com/spf13/hugo/releases/download/v0.24.1/hugo_0.24.1_Linux-64bit.deb ; sudo dpkg -i hugo*.deb ; hugo version"
install: true
 
 
# Run hugo CLI tool, updates the docs/ folder
script: hugo
 
# Pushes the changes - if any - to git,
# using the git repo and author coordinates defined above
after_success: "curl -L https://raw.githubusercontent.com/symphonyoss/contrib-toolbox/master/scripts/push-to-git.sh | bash"
```

Check [Hugo releases](https://github.com/gohugoio/hugo/releases) to update the URL in the `before_install` command with the latest version.