# Using GitHub Copilot CLI Across All Repositories

This guide shows you how to use the GitHub Copilot CLI workflows from this repository in any of your other repositories.

## Quick Reference

### For Repository Owners/Admins

You have three options to enable GitHub Copilot CLI in your repositories:

#### Option 1: Direct Action Usage (Recommended)

Add to any existing workflow or create `.github/workflows/copilot.yml`:

```yaml
name: Copilot CLI

on: [push, pull_request]

jobs:
  use-copilot:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup GitHub Copilot CLI
        uses: mvkaran/setup-copilot-cli@v1
        with:
          version: 'latest'
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Use Copilot
        run: copilot -i "your question here"
```

#### Option 2: Use the Reusable Workflow

Use the reusable workflow for verification and testing:

```yaml
name: Verify Copilot CLI

on: [push, pull_request]

jobs:
  verify-copilot:
    uses: EDGEL00P/repo/.github/workflows/reusable-copilot-setup.yml@main
    secrets:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

Note: The reusable workflow is best for verification. For using Copilot CLI in
your own jobs, use Option 1 (Direct Action Usage).

```yaml
steps:
  - name: Setup GitHub Copilot CLI
    uses: mvkaran/setup-copilot-cli@v1
    with:
      version: 'latest'
      token: ${{ secrets.GITHUB_TOKEN }}
  
  - name: Use Copilot
    run: copilot -i "your question here"
```

#### Option 3: Copy the Workflow Files

Copy any of the workflow files from this repository into your `.github/workflows/` directory:
- `copilot-cli.yml` - Basic setup and usage
- `example-code-review.yml` - PR code review example
- `reusable-copilot-setup.yml` - Reusable workflow template

## Available Workflows in This Repository

### 1. copilot-cli.yml
**Purpose:** Basic GitHub Copilot CLI setup and verification

**Triggers:**
- Push to main/master
- Pull requests to main/master
- Manual dispatch

**What it does:**
- Installs the latest Copilot CLI
- Verifies installation
- Shows basic usage examples

### 2. reusable-copilot-setup.yml
**Purpose:** Reusable workflow that can be called from other repositories

**Inputs:**
- `version` (optional): CLI version to install (default: 'latest')

**Secrets:**
- `GITHUB_TOKEN` (required): GitHub authentication token

**Outputs:**
- `copilot-version`: Installed version
- `copilot-path`: Installation path

### 3. example-code-review.yml
**Purpose:** Example of using Copilot CLI for PR code reviews

**Triggers:**
- Pull requests to main/master

**What it does:**
- Sets up Copilot CLI
- Gets list of changed files
- Provides AI-powered code review insights

## Common Use Cases

### Use Case 1: Get Help with Commands
```yaml
- name: Get git help
  run: copilot -i "how to undo the last commit in git"
```

### Use Case 2: Code Analysis
```yaml
- name: Analyze Python code
  run: |
    copilot -i "review this Python file for potential issues" < myfile.py
```

### Use Case 3: Generate Scripts
```yaml
- name: Generate deployment script
  run: |
    copilot -i "create a bash script to deploy a Node.js app" > deploy.sh
```

### Use Case 4: CI/CD Optimization
```yaml
- name: Get CI/CD advice
  run: |
    copilot -i "how to optimize GitHub Actions workflows for faster builds"
```

## Version Options

- `latest` - Most recent stable release (recommended)
- `prerelease` - Latest prerelease version (cutting edge)
- `v0.0.369` - Specific version number

## Troubleshooting

### Issue: "copilot: command not found"
**Solution:** Ensure the Setup step runs before trying to use Copilot CLI

### Issue: Authentication errors
**Solution:** Make sure you're passing the GITHUB_TOKEN:
```yaml
with:
  token: ${{ secrets.GITHUB_TOKEN }}
```
or set it as environment variable:
```yaml
env:
  GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### Issue: Workflow fails on specific OS
**Solution:** The action supports Linux, macOS, and Windows. Check the runner OS:
```yaml
runs-on: ubuntu-latest  # or macos-latest, windows-latest
```

## Platform Support

| Platform | x64 | arm64 |
|----------|-----|-------|
| Linux    | ✅  | ✅    |
| macOS    | ✅  | ✅    |
| Windows  | ✅  | ✅    |

## Security Considerations

- The `GITHUB_TOKEN` is automatically provided by GitHub Actions
- Token has read/write permissions for the repository
- Copilot CLI only uses the token for authentication with GitHub services
- No sensitive data is sent to third-party services

## Additional Resources

- [GitHub Copilot CLI Documentation](https://github.com/github/copilot-cli)
- [Setup Action on Marketplace](https://github.com/marketplace/actions/setup-github-copilot-cli)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)

## Contributing

If you have improvements or examples to add, please submit a pull request to this repository!
