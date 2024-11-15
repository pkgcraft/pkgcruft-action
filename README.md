# pkgcruft-action

GitHub action that runs pkgcruft over an ebuild repo.

## Inputs

### `repo` (optional) -- custom target repo path

By default, the current working directory is used for a repo path.

## Example workflow

Note that the action requires using x86-64 Linux runners.

```yaml
name: pkgcruft

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      pull-requests: write
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
    - name: Run pkgcruft
      uses: pkgcraft/pkgcruft-action@main
```
