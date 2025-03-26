## what-changed

This is a GitHub action that detects what changed in a series of pkgsrc
commits, and outputs variables to be used by subsequent actions.

### Usage

```yaml
jobs:
  what-changed:
    runs-on: ubuntu-latest
    outputs:
      bootstrap-hash: ${{ steps.what-changed.outputs.bootstrap-hash }}
    steps:
      - uses: actions/checkout@v4
      - name: Calculate changes
        id: what-changed
        uses: pkgsrc-ci/what-changed@v1
```

### Output variables

* `bootstrap-hash`: A unique hash of all the files that are potentially in
  the bootstrap path. This can then be used to determine if a new bootstrap
  kit needs to be produced, as well as creating a unique name for existing
  bootstrap kits to be cached and restored.

* `run-pkglint`: A boolean indicating whether any files that should be
  checked by pkglint were modified or not.

* `pkglint-files`: A list of files to be checked by pkglint.

* `run-pkgbuild`: A boolean indicating whether any package directories were
  modified or not.

* `pkgbuild-files`: A list of package files that were modified.  It is up to
  subsequent actions to decide how to handle these, e.g. create unique list
  of `PKGPATH` from them and build those packages.
