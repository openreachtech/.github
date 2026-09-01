---
name: '🚀 Publish (npmjs.com)'
about: Publish to npmjs.com Issue
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

Lint and test are not run again here — the tip of `release/vvvvvv` has already passed them in CI.

- [ ] Confirm the Publisher's and Confirmor's `shasum` / `integrity` match

- [ ] Confirm login user

  ```sh
  npm whoami
  ```

  * If not logged in, log in to `npmjs.com` before publishing

    ```sh
    npm login
    ```

    Authentication completes in your browser. npm writes the credential to `~/.npmrc` for you.

    > ⚠️ **The session expires 2 hours after login.**
    > The `_authToken` line stays in `~/.npmrc`, but the registry stops accepting it.
    > `npm whoami` is the only reliable check — run `npm login` right before publishing.

    > ⚠️ **Never pass a token as a command-line argument.**
    > `npm config set ... <token>` records the token in your shell history in plain text.

- [ ] Last Confirm

  ```sh
  git status &&
  git diff "$(git rev-parse origin/release/vvvvvv)"
  ```

- [ ] Publish

  The command below publishes as OSS. Drop `--access public` when the package is private.

  > ⚠️ **Approving in your browser is required, even when `npm whoami` already shows you.**
  > `npm publish` opens your browser and waits for you to approve the publish there.
  > Run it where you can reach a browser, and do not walk away from the terminal.

  ```sh
  git stash push -u &&
  git checkout "$(git rev-parse origin/release/vvvvvv)" &&
  npm publish --access public
  ```

# Confirm Published on npmjs.com

- [ ] Confirm the published version is listed on the page below
  - [ORT published packages in https://npmjs.com](https://www.npmjs.com/settings/openreachtech/packages)

<br>
<br>

# Merge `release/vvvvvv` to `main`

- [ ] Release `vvvvvv` > Main
