# Install Poetry Action

[![Tests](https://github.com/aisightgmbh/actions-install-poetry/actions/workflows/on_push.yaml/badge.svg)](https://github.com/aisightgmbh/actions-install-poetry/actions/workflows/on_push.yaml)

This action can be used to install Poetry and Python.
It also enables Poetry to pull packages from AWS CodeArtifact.
The latter only works when running on a self-hosted runner that has the right permissions set for the AWS CLI.
The action assumes you have configured a source in your `pyproject.toml` named `artifactory` that points to your AWS CodeArtifact artifactory.
It will then create the credentials as the environment variables `POETRY_HTTP_BASIC_ARTIFACTORY_USERNAME` and `POETRY_HTTP_BASIC_ARTIFACTORY_PASSWORD` which will be picked up by Poetry automatically.

## Example

```toml
# pyproject.toml

...

[[tool.poetry.source]]
name = "artifactory"
url = "<ArtifactoryUrl>"
secondary = true  # if you still want to use pipy

...
```

```yaml
# workflow.yaml

...

jobs:
  example-on-default-runner:
  runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v3
    - uses: aisightgmbh/actions-install-poetry@v1
      with:
        poetry-version: "1.1.14"
        python-version: "3.9"
    - run: poetry --version

  example-on-custom-runner:
    runs-on: <YourCustomRunner>
    steps:
      - uses: actions/checkout@v3
      - uses: aisightgmbh/actions-install-poetry@v1
        with:
          poetry-version: "1.1.14"
          python-version: "3.9"
          artifactory-owner: ${{ secrets.owner }}
          artifactory-domain: ${{ secrets.domain }}
      - run: poetry --version
```
