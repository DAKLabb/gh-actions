<br/>
<div align="center">
  <a href="https://daklabb.dev">
    <picture>
      <img alt="DAKLabb logo" src="https://daklabb.s3.us-east-1.amazonaws.com/DAKLabb-logo.png" height="200" align="center">
    </picture>
  </a>
</div>
<br/>

# Overview

GitHub Actions are both powerful and broadly accessible. With minimal effort and little/no cost, you can build out a robust CI/CD pipeline that is versioned along with your code and smoothly integrated with your release process. That said, the [GitHub provided documentation](https://docs.github.com/en/actions) leaves something to be desired. The goal of this project is to provide helpful docs/examples to allow those new to GH actions (or those like myself who get rusty between projects) to hit the ground running and build out effective workflows quickly. While my goal is to make these docs as complete as possible, feel free to reach out to me vai email (david@daklabb.dev) if you have a question not answered here or would like to engage me to help you build out your CI/CD pipelines.

# Creating a workflow
To add a GH workflow to your repo, simplly create a `.gihub` directory at the root of your poject, and within this, create a `workflows` directory. You can then create yaml files in this directory to define your repo's workflows. For the smiple "hello world" style workflow discussed below, I have created [ci.yaml](/.github/workflows/ci.yaml) in this repo.

## Triggers
There are [lots of ways](https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/triggering-a-workflow) to trigger GH actions, but the 5 I use most often are:
- PR Sync: run the CI when a PR is opened/updated against a specific branch
- Brnach Push: run when code is pushed to a particular branch
- Tag Publish: run when a tag is created in the repo
- Cron: run on a schedule
- Manual: run whenever manually triggered

Triggers live in the [on](https://github.com/DAKLabb/gh-actions/blob/main/.github/workflows/ci.yaml?plain=1#L3) stanza.

### PR Sync
One of my key goals in adding workflows to a project is to maintain code quality. I may want to enfoce style conventions with a linter, or perhaps run unit tests, or even just make sure my code builds. Generally, the goal will be that the `main` branch is stable/correct, and I will want to make sure that any PRs run checks before allowing code to be merged in. Note: Adding [branch protection rules] allows us to actually enforce this.

While there are a number of `pull_request` triggers, I tend to focus on those targeting specfiic `branches`. In [this]((https://github.com/DAKLabb/gh-actions/blob/main/.github/workflows/ci.yaml?plain=1#L4-L8)) example, I trigger the CI on any PR targeting any of my key branches (`main`, `staging`, `dev`).

```yaml
on:
  pull_request:
    branches:
      - main
      - staging
      - dev
```

### Branch Push
In addition to having CI that protects prod/staging/dev branchs by running workflows on PRs, you may also want to have CI that runs on pushes to particular branches. For example, if you have environments that you deploy your various builds to (main branch -> production, staging branch -> staging, dev branch -> develop) you may want to trigger CI to update those deployments when you push to a particular branch.

In [this]((https://github.com/DAKLabb/gh-actions/blob/main/.github/workflows/ci.yaml?plain=1#L9-L13)) example, I trigger the CI on pushes to any of my key branches (`main`, `staging`, `dev`).

```yaml
on:
  push:
    branches:
      - main
      - staging
      - dev
```

### Tag Publish
As with pushes to branches, you can also trigger a workflow when a tag is pushed. Lets say I want to publish my python SDK every time I create a semantically versioned tag in my repo. I could do something like [this](https://github.com/DAKLabb/gh-actions/blob/main/.github/workflows/ci.yaml?plain=1#L14-L15) and push to pypi every time a new tag matchign my naming convention is pushed to my repo.

```yaml
on:
  push:
    tags:
      - 'v[0-9]+.[0-9]+.[0-9]+'
```

### Cron
When managing client code, I like to write regression tests that run against the production system it is designed to interact with. This is a great use-case for cron-based CI jobs. [This](https://github.com/DAKLabb/gh-actions/blob/main/.github/workflows/ci.yaml?plain=1#L16-L17) example will run my CI job ever day at 6am GMT. The benefit of jobs like this is that they can detect issue before work hours to alert you to issues before your users find them.

```yaml
on:
  schedule:
    - cron: "0 6 * * *"
```

### Manual
If you are reading this, you may have tried developing GH actoins before and come to the conclusion that they are a pain to test. This is a fact, but one method that can make life a bit easier is to add a [manual trigger](https://github.com/DAKLabb/gh-actions/blob/main/.github/workflows/ci.yaml?plain=1#L18) to your workflow. This allows you to trigger it ad-hoc for testing. It can also be useful for ad-hoc integration tests or other useful workflows discussed in more details in a [later]()s secion.

```yaml
on:
  workflow_dispatch:
```

## Environment & Secrets
Within the workflow/actions you can create env variables using `env`. This can be done at the [workflow level](https://github.com/DAKLabb/gh-actions/blob/main/.github/workflows/ci.yaml?plain=1#L20-L22), at the [job level](https://github.com/DAKLabb/gh-actions/blob/main/.github/workflows/ci.yaml?plain=1#L28-L29), or at the [step level](https://github.com/DAKLabb/gh-actions/blob/main/.github/workflows/ci.yaml?plain=1#L42-L43). These can be hardcoded values, but they can also be templated from the action context (more on this [later]()) or from repo secrets (see screenshot below).

![Action secrets and variable](imgs/action_secrets_and_variables.png)


## Jobs/Steps
### Conditional Execution


## Useful info
### Workflow context
### Retrying on failure
#### Workflows
#### Jobs
#### Steps

# Example Workflows/Actions
## Code Quality
### Lint
### Compile
### Unit Test
### Integration Tests

## Continuous deployment/gitOps
### Dev/Test/Prod

## DevEx/QOL
### Dependabot Auto-approve/auto-merge
### Periodic updates (think sayari fern spec)
### Slack integration


## Sample Implementation
### DAKLabb Website
### Fern?

# Related GH settings
## Branch protection rules
### Checks
### Convos
### ...

## Dependabot

# Sharing actions/workflows
## Inputs
## Tagging
## Testing



# Frequenty Asked Questions

