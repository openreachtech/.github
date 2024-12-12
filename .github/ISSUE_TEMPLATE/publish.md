---
name: '🚀 Publish'
about: Publish Issue
title: '🚀 Publish `vvvvvv`'
labels: ''
assignees: ''

---
## Overview

Publish version as `vvvvvv`.

## Tasks

- [ ] ⚙️ Update package version to `vvvvvv`
- [ ] Publish by procedure
- [ ] ⚙️ Merge to `main` as `vvvvvv`
- [ ] Push version tag `vvvvvv` on main

<br>
<br>

## Prepare by Publisher

### (1) Confirm version of environment

- [ ] Node.js version `nnnnnn`
- [ ] npm version `nnnn`

### (2) Confirm Exporting Contents

- [ ] To export new features correctly
- [ ] Confirm version in `package.json`
- [ ] Confirm version in `package-lock.json`

<br>
<br>

## Confirm to Publish

### (1) Pick up Target Commit Hash to Publish

- [ ] git fetch origin --prune
- [ ] git log --graph --oneline --decorate --all
- [ ] target commit as: `xxxxxxxx`

### (2) Publish with `--dry-run`

- [ ] Confirm on target commit hash

  ```
  git diff xxxxxxxx
  ```

- [ ] Confirm no differences

  ```sh
  git status
  ```

- [ ] Report publishing logs with `--dry-run` in the comments of this issue

  ```
  npm publish --dry-run
  ```

  Sample logs as follows:

  ```
  % npm publish --dry-run
  npm notice
  npm notice 📦  your-package-name
  npm notice === Tarball Contents ===
  npm notice 3.5kB index.js
  npm notice 1.1kB LICENSE
  npm notice 428B  README.md
  npm notice 879B  package.json
  npm notice === Tarball Details ===
  ...
  ...
  npm notice
  + @openreachtech/package-name@0.0.0
  ```

### (3) Confirm Logs with `--dry-run` by Other Member before Actual Publishing

- [ ] Confirm Node version `nnnnnn`

  ```sh
  node -v
  ```

- [ ] Confirm npm version `nnnn`

  ```sh
  npm -v
  ```

- [ ] Move to target commit hash

  ```sh
  git checkout xxxxxxxx
  ```

- [ ] Report publishing logs with `--dry-run` in the comments of this issue

  ```
  npm publish --dry-run
  ```

<br>
<br>

## (4) Publish Actually

- [ ] Confirm login user

  ```
  npm whoami --registry https://npm.pkg.github.com
  ```

- [ ] npm login to GitHub Packages before publishing if requires

  1. Get login password from your GitHub settings<br>Go to → https://github.com/settings/tokens
  2. Copy your `personal access token` created
  3. Login

      ```sh
      npm login --registry=https://npm.pkg.github.com/
      ```

      ```sh
      Username: your-github-account-in-lower-case-only
      Password:
      Email: your.mail.account@gmail.com
      Logged in as [your-name] on https://npm.pkg.github.com/.
      ```

- [ ] Last Confirm

  ```sh
  git status
  ```

  ```sh
  git diff xxxxxxxx
  ```

- [ ] Publish

  When publish as OSS, use options of `--access public`

  ```sh
  npm publish
  ```

## (5) Confirm Access Right of Published Package

- When public module, check by installing package
- When ORT private module, check it below in package page

  ```
  https://github.com/orgs/openreachtech/packages/npm/__package__name__/settings
  ```
