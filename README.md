# OpenAPI Spec and CI/CD to Redocly

## Introduction

This repository demonstrates the use of an OpenAPI specification and the deployment of an API using GitHub Actions. Special thanks to Łukasz Michta for his "CI/CD for Technical Writers with OpenAPI and Redocly" tutorial, which served as the foundation for this project. I adapted and refined the API to align with my use case.

## Part 1: Build API docs from the OpenAPI spec with Redocly

### What is Redocly

Redocly is a platform designed to streamline and enhance the process of creating, managing, and publishing API documentation. Redocly helps generate interactive and visually appealing API documentation from OpenAPI specifications. It transforms raw API definitions into developer-friendly documentation.

**The OpenAPI YAML API**

> api-specs/api-v1.yaml

*Sample view of the Products API*

Screenshot of the [Products API on Redocly](docs/API-screen.png)

Screenshot of the [Products API on readme.io](https://github.com/janetjotw/create-cicd-automation/blob/main/api-specs/products-api.png).


## Part 2: Automated release notes

When a pull request changes the API spec, Claude drafts a release note and adds it to the PR for review. The PR isn't merged until a human reviews and approves it. The agent itself is Claude Code, running for a few minutes inside a GitHub Action whenever the workflow triggers.

### One-time setup

1. **Generate a Claude token.** In your terminal, run `claude setup-token`, sign in when the browser opens, and copy the token it prints.
**Note**: If you have a Claude PRO subscription, you can use that to generate a token instead of generating an API key.
2. **Add the token to GitHub.** In the repo, go to **Settings > Secrets and variables > Actions > New repository secret**. Name it `CLAUDE_CODE_OAUTH_TOKEN` and paste the token.
3. **Install the Claude GitHub app** on this repo: https://github.com/apps/claude. This is what lets the workflow commit the release note and comment on your PR.
4. **Add the automation files:** 
   - `.github/workflows/api-release-notes.yml`: runs Claude when a PR changes `api-specs/`
   - `.github/docs-agent/release-notes-instructions.md`: tells Claude how to write the notes
   - `docs/release-notes.md`: where the notes are saved

### Each time the API changes

1. **Change the spec on a new branch.** Edit the `api-specs/api-v1.yaml`; for example, add a field `SKU`.
2. **Open a pull request** into `main`.
3. **Claude drafts the release note.** Within a few minutes, it adds an entry to `docs/release-notes.md` on your PR and comments with anything to check.
4. **A human reviews the spec change and the note together.** Edit the note if needed.
5. **Merge the PR.** The release note goes live on `main` with the spec change.


 
