# Reusable GitHub Actions Workflows

This repository contains reusable GitHub Actions workflows for building, publishing Python packages, and deploying to staging. The workflows are designed to streamline your CI/CD processes and can be easily integrated into your own GitHub Actions workflows.

## Available Workflows

### 🚀 Build & Publish Python Package

This workflow builds and publishes a Python package to a specified repository. It supports semantic versioning and allows you to configure various parameters.

**File:** `.github/workflows/publish-python-package.yml`

#### Workflow Inputs

- **`source_branch`** (string, default: `main`): The branch of the source repository to be checked out.
- **`pypi_repository`** (string, default: `testpypi`): The name of the package to be built and deployed.
- **`version_update`** (string, required): The type of semantic version update (`patch`, `minor`, or `major`).

#### Workflow Triggers

- **`workflow_call`**: This trigger allows the workflow to be called by other workflows.
- **`workflow_dispatch`**: Allows the workflow to be manually triggered via the GitHub Actions interface.

#### Example Usage

Here's an example of how to use the `publish-python-package` workflow in another workflow file:

```yaml
name: 🔬 Deploy staging version [auto]

on:
  workflow_dispatch:
  push:
    branches:
      - develop
    paths-ignore:
      - "README*.md"
      - ".gitignore"
      - ".github/**"
      - ".vscode/**"
      - "Makefile"
      - "examples/**"
      - ".bumpversion.cfg"
      - "pyproject.toml"

concurrency:
  group: staging-${{ github.event.pull_request.number || github.ref }}
  cancel-in-progress: true

jobs:
  build-and-publish-package:
    permissions:
      contents: write
    uses: usepolvo/workflows/.github/workflows/publish-python-package.yml@develop
    with:
      version_update: patch
    secrets: inherit
```

### Setup and Configuration

1.	Add the Workflow to Your Repository
Copy the .github/workflows/publish-python-package.yml file into your repository’s .github/workflows/ directory.

2.	Configure Your Workflow
You can customize the workflow by setting the appropriate inputs and secrets as required. Ensure that the necessary secrets, such as TEST_PYPI_TOKEN, are added to your repository’s secrets.

3.	Trigger the Workflow
The workflow can be triggered either manually via GitHub Actions or automatically based on push events to specified branches.

## Contribution

If you have any suggestions or improvements, feel free to open an issue or submit a pull request. Contributions are welcome!

## License

This repository is licensed under the MIT License.