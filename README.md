# Preternatural Build Action

This GitHub Action allows you to run Preternatural CLI commands on your repositories.

## Features

- Run Preternatural `build` and `update` commands
- Specify Xcode version for your builds
- Configure build settings such as derived data path and platform targets
- Verify update process
- Choose between debug and release configurations

## Usage

To use this action in your workflow, add the following step:

```yaml
- name: Run Preternatural Command
  uses: PreternaturalAI/github-action@main
  with:
    command: build  # or 'update'
```

### Inputs

- `command`: The Preternatural command to run. Supported values are `build` and `update`.
- `derived_data_path`: (Optional) The path to the derived data folder.
- `build_all_platforms`: (Optional) Set to 'true' to build for all supported platforms.
- `verify`: (Optional) Set to 'true' to verify the update process (for update command).
- `configuration`: (Optional) Build configuration. Can be 'debug' or 'release'. Defaults to 'release'.
- `xcode-version`: (Optional) Specify the Xcode version to use. Defaults to 'latest-stable'.

## Examples

### Basic Build Command

```yaml
- name: Run Preternatural Build
  uses: PreternaturalAI/github-action@main
  with:
    command: build
```

### Update Command with Verification

```yaml
- name: Run Preternatural Update
  uses: PreternaturalAI/github-action@main
  with:
    command: update
    verify: 'true'
```

### Build with Specific Xcode Version and Configuration

```yaml
- name: Run Preternatural Build
  uses: PreternaturalAI/github-action@main
  with:
    command: build
    xcode-version: '14.3.1'
    configuration: 'debug'
    build_all_platforms: 'true'
```

## Full Workflow Example

Here's a comprehensive example of how to use this action in a workflow, including caching and error handling:

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

    - name: Cache derived data
      uses: actions/cache@v3
      with:
        path: ~/Library/Developer/Xcode/DerivedData
        key: ${{ runner.os }}-derived-data
        restore-keys: |
          ${{ runner.os }}-derived-data

    - name: Run Preternatural Build
      id: build
      continue-on-error: true
      uses: PreternaturalAI/github-action@main
      with:
        command: build
        derived_data_path: ~/Library/Developer/Xcode/DerivedData
        xcode-version: 'latest-stable'
        build_all_platforms: 'true'

    - name: Clear derived data and retry on failure
      if: steps.build.outcome == 'failure'
      run: |
        rm -rf ~/Library/Developer/Xcode/DerivedData
        echo "Cleared derived data. Retrying build..."

    - name: Retry Preternatural Build
      if: steps.build.outcome == 'failure'
      uses: PreternaturalAI/github-action@main
      with:
        command: build
        derived_data_path: ~/Library/Developer/Xcode/DerivedData
        xcode-version: 'latest-stable'
        build_all_platforms: 'true'
```

This workflow does the following:

1. Triggers on push events for all branches.
2. Sets up the macOS environment.
3. Checks out the repository.
4. Sets up caching for the derived data to speed up subsequent builds.
5. Runs the Preternatural build command with specific options:
   - Uses the latest stable Xcode version
   - Builds for all platforms
   - Specifies a derived data path
6. If the build fails, it clears the derived data and retries the build.

This example showcases error handling and optimization techniques to ensure robust builds in your CI/CD pipeline.

## Notes

- The action automatically sets up the specified Xcode version before running the Preternatural command.
- For detailed information on Xcode version specification, refer to the [setup-xcode action documentation](https://github.com/marketplace/actions/setup-xcode-version).
