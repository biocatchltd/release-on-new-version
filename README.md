# release-on-new-version
a github action to automatically create a new release when a new version is pushed. It checks the version in a `pyproject.toml` file, and if a tag of the version does not exist. It fetches the release notes from a changelog and creates a release for that version.

## Example
```yaml
name: Check for new tags
on:
    push:
        branches:
            - main

jobs:
    check-tag:
        permissions:
            contents: write

        name: check for new tags
        runs-on: ubuntu-latest

        steps:
            - name: release on new version
              uses: biocatchltd/release-on-new-version@latest
              with:
                token: ${{ secrets.MY_SECRET_TOKEN }}
```

## inputs
* `token`: a github token to use when releasing, defaults to the default `github.token`, note that [the default token does not trigger workflows](https://github.com/actions/create-release/issues/71)
* `pyproject-path`: the path to the TOML file to use to determine the version, default to `pyproject.toml`
* `changelog-path`: the path to the markdown file to use to determine the version, default to `CHANGELOG.md`

## outputs
* `is-released`: whether the step created a new release
* `found-version`: the version detected by the action
* `release-notes`: the markdown snippet of the release notes of the version, will be blank if no version was released
* `is-prerelease`: whether the version is detected as a pre-release (currently, all versions with a `-` in them are considered pre-releases)

## Permissions
This Action requires the following permissions on the GitHub integration token:

```yaml
permissions:
  contents: write
```