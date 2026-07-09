---
name: '🚀 Publish (npmjs.com)'
about: Publish to npmjs.com Issue
title: '🚀 Publish `vvvvvv`'
labels: ''
assignees: ''

---
# Overview

Publish version as `vvvvvv`.

## Tasks

- [ ] ⚙️ Update version of dependency packages to latest
- [ ] ⚙️ Update package version to `vvvvvv`
- [ ] Publish by procedure
- [ ] ⚙️ Merge to `main` as `vvvvvv`

<br>
<br>

# Prepare by Publisher

## (1) Confirm version of environment

- [ ] Node.js version `nnnnnn`
- [ ] npm version `nnnn`

## (2) Confirm Exporting Contents

- [ ] To export new features correctly
- [ ] Confirm version in `package.json`
- [ ] Confirm version in `package-lock.json`

<br>
<br>

# Confirm to Publish

## (1) Pick up Target Commit Hash to Publish

- [ ] git fetch origin --prune
- [ ] git log --graph --oneline --decorate --all
- [ ] target commit as: `xxxxxxxx`

## (2) Publish with `--dry-run`

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
  npm pack --dry-run
  ```

  Sample logs as follows:

  ```
  % npm pack --dry-run
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

## (3) Confirm Logs with `--dry-run` by Other Member before Actual Publishing

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
  npm pack --dry-run
  ```

<br>
<br>

# Publish

## (4) Publish Actually

- [ ] Confirm login user

  ```
  npm whoami
  ```

  * npm login to npmjs.com before publishing if requires

    1. Get access token from your `npmjs.com` account<br>Go to → https://www.npmjs.com/settings/[username]/tokens
    2. Set your `personal access token`
       ```sh
       npm config set //registry.npmjs.org/:_authToken [your access token]
       ```

    3. Login

       ```sh
       npm login
       ```

       ```sh
       % npm login
       npm notice Log in on https://registry.npmjs.org/
       Login at:
       https://www.npmjs.com/login?next=/login/cli/...
       Press ENTER to open in the browser...
       Logged in on https://registry.npmjs.org/.
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
- Confirm check of `Inherit access from source repository (recommended)`

  ```
  https://www.npmjs.com/settings/openreachtech/packages
  ```
