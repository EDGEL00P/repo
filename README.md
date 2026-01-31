# repo

This repository provides GitHub Actions workflow configurations for setting up GitHub Copilot CLI across all your repositories.

## GitHub Copilot CLI Setup

This repository includes workflows to set up and use the [GitHub Copilot CLI](https://github.com/marketplace/actions/setup-github-copilot-cli) in your GitHub Actions pipelines.

### Available Workflows

#### 1. Direct Workflow (`copilot-cli.yml`)
A standalone workflow that demonstrates how to set up and use GitHub Copilot CLI in your repository.

**Triggers:**
- Push to `main` or `master` branches
- Pull requests to `main` or `master` branches
- Manual trigger via `workflow_dispatch`

#### 2. Reusable Workflow (`reusable-copilot-setup.yml`)
A reusable workflow that can be called from any repository to set up GitHub Copilot CLI.

**Usage in other repositories:**

Create a workflow file in your repository (e.g., `.github/workflows/use-copilot.yml`):

```yaml
name: Use Copilot CLI

on: [push, pull_request]

jobs:
  copilot-tasks:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup GitHub Copilot CLI
        uses: mvkaran/setup-copilot-cli@v1
        with:
          version: 'latest'
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Use Copilot CLI
        run: copilot -i "your prompt here"
```

Alternatively, you can reference the reusable workflow for just the setup step,
but note that each job needs to install Copilot CLI separately:

```yaml
name: Use Copilot CLI

on: [push]

jobs:
  verify-copilot:
    uses: EDGEL00P/repo/.github/workflows/reusable-copilot-setup.yml@main
    secrets:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    with:
      version: 'latest'
```

### Quick Start for Any Repository

To add GitHub Copilot CLI to your repository's workflow:

1. **Option 1: Add to existing workflow**
   ```yaml
   - name: Setup GitHub Copilot CLI
     uses: mvkaran/setup-copilot-cli@v1
     with:
       version: 'latest'
       token: ${{ secrets.GITHUB_TOKEN }}
   
   - name: Use Copilot CLI
     run: copilot -i "your prompt"
   ```

2. **Option 2: Use the reusable workflow**
   ```yaml
   jobs:
     copilot-setup:
       uses: EDGEL00P/repo/.github/workflows/reusable-copilot-setup.yml@main
       secrets:
         GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
   ```

### Features

- ✅ Supports all major platforms: Linux, macOS, and Windows
- ✅ Supports x64 and arm64 architectures
- ✅ Allows specifying a specific version or use the latest
- ✅ Automatically detects platform/architecture
- ✅ Simplifies token setup by exporting GH_TOKEN
- ✅ Ready to use across all your repositories

### Examples

**Ask Copilot for help:**
```yaml
- name: Get coding help
  run: copilot -i "how to create a Python virtual environment"
```

**Use with piped input:**
```yaml
- name: Analyze code
  run: |
    echo "Explain this code" | copilot -i "$(cat README.md)"
```

### Version Options

- `latest` - Install the latest stable version (default)
- `prerelease` - Install the latest prerelease version
- `v0.0.369` - Install a specific version

## License

This repository provides workflow configurations for GitHub Copilot CLI setup.
