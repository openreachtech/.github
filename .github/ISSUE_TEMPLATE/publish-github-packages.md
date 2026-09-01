---
name: '🚀 Publish (GitHub Packages)'
about: Publish to GitHub Packages Issue
title: '🚀 Publish `vvvvvv`'
labels: ''
assignees: ''

---
# Overview

Publish version `vvvvvv`.

# Confirmations before Publish

Do the final check before publishing. If there is any item below that you cannot check off, make it a sub-task and merge it into the `release/vvvvvv` branch.

- [ ] ⚒️ Export new features correctly in version `vvvvvv`
- [ ] ⚙️ Bump version of dependency packages to latest
- [ ] 📄 Resolve license inconsistencies
- [ ] ⚙️ Bump package version to `vvvvvv`

<br>
<br>

# Procedure for Publisher

## (1) Confirm package version the same

- [ ] The same version in `package.json` and `package-lock.json`

  ```sh
  grep -m2 '"version"' package.json package-lock.json
  ```

## (2) Get `--dry-run` checksum for Verifying

- [ ] Get the `--dry-run` checksum

  `npm ci` belongs in the chain because `prepack` may build files that ship. Without the dev dependencies, `npm pack` fails with `code 127`.

  ```sh
  git stash push -u &&
  git fetch origin --prune &&
  echo "
  📦️ Node: $(node -v)
  📦️ npm : $(npm -v)
  " &&
  git -c advice.detachedHead=false checkout "$(git rev-parse origin/release/vvvvvv)" &&
  npm ci &&
  npm pack --dry-run 2>&1 | sed -n '/Tarball Details/,$p; /^npm error/p'
  ```

## (3) Report publishing logs with `--dry-run` in the comments of this issue

- [ ] Paste the result via comment in this issue

Example:

```
📦️ Node: v22.22.3
📦️ npm : 11.14.1

HEAD is now at 7b6d182 Merge pull request #105 from openreachtech/release/1.2.0
npm notice Tarball Details
npm notice name: @openreachtech/todo-fulfill-here
npm notice version: 0.0.0
npm notice filename: openreachtech-todo-fulfill-here-0.0.0.tgz
npm notice package size: 630 B
npm notice unpacked size: 1.2 kB
npm notice shasum: f9ab38f3bdfcaff5559832e00000000000000000
npm notice integrity: sha512-ILxF820000000[...]OIK0000000000==
npm notice total files: 2
npm notice
openreachtech-todo-fulfill-here-0.0.0.tgz
```

## (4) Confirm target commit hash

- [ ] Confirm target commit hash

  ```sh
  git log --graph --oneline --decorate --all --exclude=refs/stash
  ```

## (5) Request the verify `--dry-run` checksum on Slack

- [ ] Request the verify `--dry-run` checksum on Slack

<br>
<br>

# Verify by Confirmor

## (1) Run the Command

Run the same block the Publisher ran. If the Node or npm version differs from the Publisher's report, align it before comparing — a mismatch caused by tooling says nothing about the content.

```sh
git stash push -u &&
git fetch origin --prune &&
echo "
📦️ Node: $(node -v)
📦️ npm : $(npm -v)
" &&
git -c advice.detachedHead=false checkout "$(git rev-parse origin/release/vvvvvv)" &&
npm ci &&
npm pack --dry-run 2>&1 | sed -n '/Tarball Details/,$p; /^npm error/p'
```

## (2) Report confirming logs with `--dry-run` in the comments of this issue

Paste the Results via comment of this issue

Example:

```
📦️ Node: v22.22.3
📦️ npm : 11.14.1

HEAD is now at 7b6d182 Merge pull request #105 from openreachtech/release/1.2.0
npm notice Tarball Details
npm notice name: @openreachtech/todo-fulfill-here
npm notice version: 0.0.0
npm notice filename: openreachtech-todo-fulfill-here-0.0.0.tgz
npm notice package size: 630 B
npm notice unpacked size: 1.2 kB
npm notice shasum: f9ab38f3bdfcaff5559832e00000000000000000
npm notice integrity: sha512-ILxF820000000[...]OIK0000000000==
npm notice total files: 2
npm notice
openreachtech-todo-fulfill-here-0.0.0.tgz
```

<br>
<br>

# Publish

- [ ] Confirm the Publisher's and Confirmor's `shasum` / `integrity` match

- [ ] Confirm login user

  ```sh
  npm whoami --registry https://npm.pkg.github.com
  ```

  * If not logged in, log in to GitHub Packages before publishing

    1. Create a fine-grained personal access token<br>Go to → https://github.com/settings/personal-access-tokens
       - Repository access: only the repositories you publish
       - Repository permissions: **`Packages: Read and write`**
       - Do **not** grant `Contents: Write` or use a classic token with `repo` scope
    2. Log in. Paste the token at the `Password:` prompt — it is not echoed and not recorded in your shell history.
       ```sh
       npm login --scope=@openreachtech --auth-type=legacy --registry=https://npm.pkg.github.com
       ```

       > ⚠️ **Never pass a token as a command-line argument.**
       > `npm config set ... <token>` records the token in your shell history in plain text.

- [ ] Last Confirm

  ```sh
  git status &&
  git diff "$(git rev-parse origin/release/vvvvvv)"
  ```

- [ ] Publish

  When publishing as OSS, add the `--access public` option to `npm publish`

  ```sh
  git stash push -u &&
  git checkout "$(git rev-parse origin/release/vvvvvv)" &&
  npm publish # --access public ## if the package is public
  ```

# Confirm Access Right of Published Package

- When public module, check by installing package
- When ORT private module, check it below in package page
- Confirm check of `Inherit access from source repository (recommended)`

[ORT package settings in GitHub](https://github.com/orgs/openreachtech/packages/npm/__package__name__/settings)

<br>
<br>

# Merge `release/vvvvvv` to `main`

- [ ] Release `vvvvvv` > Main
