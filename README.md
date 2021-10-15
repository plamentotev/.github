# Codehaus Plexus shared GitHub Actions and configuration


# Usage github build action

Create GitHub workflow (in in `.github/workflows/maven.yml` ) in project with content:

```yaml
name: Verify

on: [push, pull_request]

jobs:
  build:
    name: Verify
    uses: codehaus-plexus/.github/.github/workflows/maven.yml@master (or tag)

```

Excludes from build matrix:

```yaml
...
    uses: codehaus-plexus/.github/.github/workflows/maven.yml@master (or tag)
    with:
      matrix-exclude: >
        [ 
          {"jdk": "8"},   # exclude jdk 8 from all builds
          {"os": "windows-latest"}, # exclude windows from all builds
          {"jdk": "8", "os": "windows-latest"} # exclude jkd 8 on windows
        ]
```

# Usage release-drafter

To have the github release filled with Pull request content

in `.github` create a file `release-drafter.yml` with such content
```
_extends: .github
tag-template: plexus-utils-$NEXT_MINOR_VERSION (your tag name format)
```

in `.github/workflows/release-drafter.yml`

```
name: Release Drafter
on:
  push:
    branches:
      - master
jobs:
  update_release_draft:
    runs-on: ubuntu-latest
    steps:
      - uses: release-drafter/release-drafter@v5.15.0
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

# Resources

- [Workflow syntax](https://docs.github.com/en/actions/learn-github-actions/workflow-syntax-for-github-actions)
- [Reusing workflows](https://docs.github.com/en/actions/learn-github-actions/reusing-workflows)

