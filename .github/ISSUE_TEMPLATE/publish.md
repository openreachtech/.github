---
name: '🚀 Publish'
about: Publish Issue
title: '🚀 Publish `vvvvvv`'
labels: ''
assignees: ''

---
## Overview

* Publish version as `vvvvvv`.

* See [[Renchan] Publish to GitHub Packages - Kibela](https://openreachtech.kibe.la/notes/5722)

## Tasks

- [ ] ⚙️ Update package version to `vvvvvv`
- [ ] Publish by procedure
- [ ] ⚙️ Merge to `main` as `vvvvvv`
- [ ] Push version tag `vvvvvv` on main

## npm login to GitHub Packages before publishing

* Procedure login as follows:
  1. get login password from your GitHub settings<br>Go to → https://github.com/settings/tokens
  2. copy your `personal access token` created
  3. run in terminal
      ```
      % npm login --registry=https://npm.pkg.github.com/

      Username: your-github-account-in-lower-case-only
      Password:
      Email: your.mail.account@gmail.com
      Logged in as [your-name] on https://npm.pkg.github.com/.
      ```

## Procedure to Publish

### 1. Check Commit Hash to Publish

- [ ] git log --graph --oneline --decorate --all
- [ ] target commit as: `xxxxxxxx`

### 2. Confirm Work to Publish

- [ ] To export new features correctly.
- [ ] Confirm version in `package.json`
- [ ] Confirm version in `package-lock.json`

### 3. Publish

- [ ] Confirm login user

  ```
  npm whoami --registry https://npm.pkg.github.com
  ```

- [ ] Confirm publishing commit with `--dry-run`

  ```
  npm publish --dry-run
  ```

  ```
  % npm publish --dry-run
  npm notice
  npm notice 📦  your-package-name
  npm notice === Tarball Contents ===
  npm notice 3.5kB .eslintrc.yml
  npm notice 1.1kB LICENSE
  npm notice 428B  README.md
  npm notice 879B  package.json
  npm notice === Tarball Details ===
  ...
  ...
  ...
  npm notice
  + @openreachtech/eslint-config@0.0.0
  ```

- [ ] Check log of `--dry-run` by other member before actual publishing
  - target commit as: `xxxxxxxx`
  - node version: `nnnn`
  - npm version: `nnn`

- [ ] Publish

  ```console
  git status
  git diff xxxxxxxx
  ```

  ```console
  npm publish
  ```
