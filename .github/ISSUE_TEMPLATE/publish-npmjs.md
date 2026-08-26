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
- [ ] ⚙️ Update version of dependency packages to latest
- [ ] 🛡️ Resolve everything `npm audit` reports, or record what is left and why

  `--omit=dev` is the half that decides urgency, since it narrows the report to what a consumer is actually exposed to.

  ```sh
  npm audit
  npm audit --omit=dev
  ```

- [ ] 🧵 `npm run lint` and 🧪 `npm test` pass on the release branch tip
- [ ] ⚙️ Update package version to `vvvvvv`
- [ ] 🎯 Confirm `license` agrees with npmjs, and that the scope resolves there

  An open-source license belongs with npmjs, and `UNLICENSED` with GitHub Packages. Either one paired with the other registry means the target was never settled.

  ```sh
  npm config get @openreachtech:registry &&
  npm pkg get name version license publishConfig
  ```

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

- [ ] Confirm target commit hash

  ```sh
  git log --graph --oneline --decorate --all --exclude=refs/stash
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

## (4) Request the verify `--dry-run` checksum

- [ ] Request the verify `--dry-run` checksum

<br>
<br>

# Verify by Confirmor

## (1) Run the Command

Run the same block the publisher ran, so the two reports are comparable.

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

    > ⚠️ **Logging in does not clear 2FA for publish.**
    > `npm publish` requires its own one-time password. See the `--otp` option below.

    > ⚠️ **Never pass a token as a command-line argument.**
    > `npm config set ... <token>` records the token in your shell history in plain text.

- [ ] Last Confirm

  ```sh
  git status &&
  git diff "$(git rev-parse origin/release/vvvvvv)"
  ```

- [ ] Publish

  The command below publishes as OSS. Drop `--access public` when the package is private.

  Replace `xxxxxx` with the 6-digit code from your authenticator app. The code is
  valid for 30 seconds, so read it immediately before running the command.

  ```sh
  git stash push -u &&
  git checkout "$(git rev-parse origin/release/vvvvvv)" &&
  npm publish --access public --otp=xxxxxx
  ```

# Confirm Access Right of Published Package

- When public module, check by installing package
- When ORT private module, check it below in package page
- Confirm check of `Inherit access from source repository (recommended)`

[ORT published packages in https://npmjs.com](https://www.npmjs.com/settings/openreachtech/packages)

<br>
<br>

# Merge `release/vvvvvv` to `main`

- [ ] Release `vvvvvv` > Main
