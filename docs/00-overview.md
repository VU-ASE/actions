# Overview

This repository contains **reusable GitHub Actions workflows** that are used by most of the services and libraries written in different languages across our organization. The actions standardize and streamline the **CI/CD** pipeline by allowing the common build, test and release logic to be a modular and shareable workflow.

## The Available Reusable Actions

### 1. build-and-upload.yaml

This workflow builds and packages the services for a release. The workflow runs on our [self-hosted runner](03-debixbuilder.md) and is triggered on `pull-request` and `release` events.

The workflow builds the service using `make build`, which is triggered for both events. However, if the event is `release` then we also perform the following steps after building the service:

- Update the `service.yaml` to include the newest version number from the release
- Then zip up the binary and the `service.yaml`
- Finally, upload the package as a GitHub Release Asset.

### 2. release.yaml

This workflow is used to automate the versioning and changelog generation process by using [Release Please](https://github.com/googleapis/release-please). It doesn't run on our [self-hosted runner](03-debixbuilder.md), instead, it uses GitHub's default `ubuntu-latest` runner.

It is triggered by a `workflow_call` from the repositories, which have a push to `main` as an event trigger themselves. The workflow also requires a `release_please_token` to be passed, which is stored as a Personal Access Token.

It is responsible for:

- Analyzing the commit messages to determine the version bump. It always looks at the highest priority commit (feat! → feat → fix) and updates the version by 1 increment, regardless of how many commits there have been.
- It automatically creates a **release pull request** which includes a version bump (e.g., from v0.3.1 to v0.3.2) and an updated `CHANGELOG.md`.

Some additional details include:

- The workflow sets `bump-minor-pre-major: true` which indicates that v0.1.0 → v0.2.0 indicates a breaking change, before the release of 1.0.0
- The release pull request title shows the following pattern: **chore(main): release 1.2.5**

### 3. test.yaml

The workflow is responsible for running the tests of the service/library (if implemented) in its own Devcontainer. It also runs using the GitHub's default `ubuntu-latest` runner.

It is triggered on every push and also requires a `gh_pat` to be passed, which is again stored as a Personal Access Token. The token is used to checkout the repository with submodule support.

The main steps are:

- Check out the repository with the submodule support
- Use the [Devcontainer Github Action](https://github.com/devcontainers/ci) to open the container of the associated service/library
- Inside the Devcontainer, run `make test` to run the unit tests (if implemented)