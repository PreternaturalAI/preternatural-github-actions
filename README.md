# Preternatural Build Action

This GitHub Action allows you to run the Preternatural CLI build command on your repositories with support for a specific Xcode version.

## Features

- Run Preternatural `build` command
- Specify Xcode version for your build
- Configure build settings such as derived data path and platform targets
- Version-specific caching of derived data

## Usage

To use this action in your workflow, add the following step:

```yaml
- name: Run Preternatural Build
  uses: PreternaturalAI/github-action/preternatural-build@main
  with:
    xcode-version: '16'
```

### Inputs

- `xcode-version`: (Required) Xcode version to use.
- `derived_data_path`: (Optional) The path to the derived data folder.
- `build_all_platforms`: (Optional) Set to 'true' to build for all supported platforms. Defaults to 'false'.

## Examples

### Basic Build Command

```yaml
- name: Run Preternatural Build
  uses: PreternaturalAI/github-action/preternatural-build@main
  with:
    xcode-version: '16'
```

### Build with Specific Configuration and All Platforms

```yaml
- name: Run Preternatural Build
  uses: PreternaturalAI/github-action/preternatural-build@main
  with:
    xcode-version: '16'
    build_all_platforms: 'true'
    derived_data_path: '~/Desktop/derived_data'
```

## Full Workflow Example

Here's an example of how to use this action in a workflow:

```yaml
name: Preternatural Build
on:
  push:
    branches:
      - '**'
jobs:
  build:
    runs-on: macos-latest
    steps:
    - name: Checkout repository
      uses: actions/checkout@v3
    
    - name: Run Preternatural Build
      uses: PreternaturalAI/github-action/preternatural-build@main
      with:
        xcode-version: '16'
```

This workflow does the following:

1. Triggers on push events for all branches.
2. Sets up the macOS environment.
3. Checks out the repository.
4. Runs the Preternatural build command with the following options:
   - Uses Xcode version 16
   - Builds for all platforms

## Notes

- The action automatically sets up the specified Xcode version before running the Preternatural command.
- Derived data is cached separately for each Xcode version to optimize build times.
- The action uses Homebrew to install the Preternatural CLI.
- For detailed information on Xcode version specification, refer to the [setup-xcode action documentation](https://github.com/marketplace/actions/setup-xcode-version).
