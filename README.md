# pkgcruft-action

GitHub action that runs [pkgcruft] over an ebuild repo and supports showing run
differences between commits including optional PR comment support.

## Inputs

### `exit` (optional) -- report sets that trigger failures

By default, critical and error level reports trigger failures.

### `repo` (optional) -- custom target repo path

By default, the current working directory is used for a repo path.

### `pr-comments` (optional) -- true or false

By default, comments are added to pull requests showing the scan difference
from the base commit.

## Example workflow

Note that for the action's diff support to work, the full history of the target
repo needs to be fetched using the checkout action's `fetch-depth: 0` option.
Shallow git repos will only show diff output if the target commit results exist
in the cache.

Also, the action requires using x86-64 Linux runners.

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
      uses: actions/checkout@v6
      with:
        fetch-depth: 0
    - name: Run pkgcruft
      uses: pkgcraft/pkgcruft-action@main
```

[pkgcruft]: <https://github.com/pkgcraft/pkgcraft/tree/main/crates/pkgcruft>
