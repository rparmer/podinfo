# Podinfo Example CI/CD project
This repo provides a basic example of using GitHub Actions with Flux for CI/CD.  The layout and pipeline flow is designed to mimic a potential real world deployment pattern.  It uses a single repo with a branch strategy to separate application code from environment deployment.

## Repo layout
The `main` branch is designed to be the home of all deployment work.  It contains the Dockerfile needed to make app changes as well as an app directory which holds the base config necessary to deploy the app to a cluster.

The `release` branch contains all the env specific configs necessary to deploy the app.  This includes specific image versions, which namespace to use, etc.  This is the branch and directories that Flux will be configured to use.

## Pipelines
In the main branch there is a single workflow defined.  This workflow will automatically build new Docker images and tag them based on the branch, tag, or pull request (depending on what triggered the action).  This image will only be pushed for branch or tag events.  PR events are only used to test the build process and make sure it succeeds correctly.  All pushes to main or newly created tags are automatically built and deployed to the dev environment.

In the release branch that is another set of workflows that handle the semi-automated release of the app to the staging and prod environments.  These workflows detect changes to the previous environment and automatically create branches, commit changes, and create PR's for the next target environment.  For example, changes to the dev environment will trigger a PR to be opened for the stagging environment and changes to the stagging environment with open a PR for the prod environment.

> Using a single repo for app and release config was used for demo simplicity.  The patterns used here could easily be migrated to a multi-repo or multi-directory setup.
