# pkgcruft-action

GitHub action that runs pkgcruft over an ebuild repo. Note that the action
currently doesn't support repo syncing which can be manually hacked around as
shown in the example below.

## Inputs

### `options` (optional) -- custom options for `pkgcruft scan`

Any options used with ``pkgcruft scan`` when running directly on the command
line.

### `repo` (optional) -- custom target repo path

By default, the current working directory is used for a repo path.

## Outputs

### `exit-status` -- action exit status

## Example workflows

Workflow with no inputs:

```yaml
name: pkgcruft

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    # currently the pkgcruft-action doesn't natively handle repo syncing
    # see https://github.com/pkgcraft/pkgcruft-action/issues/2
    - name: Checkout Gentoo repo and mangle /etc/portage/repos.conf
      shell: bash
      run: |
        mkdir -p "${HOME}/.cache/pkgcruft/repos/gentoo"
        curl -L https://github.com/gentoo-mirror/gentoo/archive/stable.tar.gz | tar -zxf - --strip-components=1 -C "${HOME}/.cache/pkgcruft/repos/gentoo"

        dir=$(mktemp -d)
        cat <<- EOF > "${dir}/repos.conf"
        [DEFAULT]
        main-repo = gentoo

        [gentoo]
        location = "${HOME}/.cache/pkgcruft/repos/gentoo"
        EOF

        sudo mv "${dir}" /etc/portage

    - name: Run pkgcruft
      uses: pkgcraft/pkgcruft-action@main
```
