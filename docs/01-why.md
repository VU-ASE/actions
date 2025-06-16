# Why we need CI/CD

With the help of CI/CD (Continuous Integration and Continuous Development) we can **automate** the software development lifecycle and also **reduce human error**. All in all, it ensures that all changes are automatically built, tested, and packaged giving us the benefits of shipping reliable code faster.

Some of the benefits due to using CI/CD:

- **Automatic testing**
- **Consistent and automated release process**
- **Faster feedback**
- **Saving time**

## What It Means to Build and Release Software

Building and releasing software is the process of turning source code into a packaged and versioned product that is ready for distribution or deployment.

We follow a structured approach to releasing software by using **conventional commits** and the [Release Please](https://github.com/googleapis/release-please) GitHub Action.

### Conventional Commits

All commit messages should follow the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) standard, which looks like:

```
feat: add dependent service
fix: correct service yaml value
chore: update dependencies
```

Using this format allows us to **automatically determine the next version number**.

### Using Release Please

The following are the steps of how Release Please automates the release process:

- Scan commit history for conventional commits
- Creates a **release pull request** with a new version number and updated `CHANGELOG.md`
