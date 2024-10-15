# Preternatural GitHub Actions

This repository contains GitHub Actions for use with [Preternatural CLI](https://github.com/PreternaturalAI/CLI-release).


## Preternatural Build Action

This action allows you to run the Preternatural CLI build command on your repositories with support for a specific Xcode version.

### Inputs

- `xcode-version`: (Required) Xcode version to use.
- `derived_data_path`: (Optional) The path to the derived data folder.
- `build_all_platforms`: (Optional) Set to `true` to build for all supported platforms. Defaults to `false`.
- `configuration`: (Optional) Build configuration (debug or release). Defaults to `debug`.

#### Example Usage

```yaml
name: Run Preternatural Build
uses: PreternaturalAI/github-action/preternatural-build@main
with:
  xcode-version: '16'
  build_all_platforms: 'false'
  derived_data_path: '~/Library/Developer/Xcode/DerivedData'
  configuration: 'release'
```

## Preternatural Archive & Notarize Action

This action allows you to archive and notarize a MacOS application using the Preternatural CLI.

### Inputs

- `xcode-version`: (Required) Xcode version to use.
- `notarization_username`: (Required) Your Apple ID email.
- `notarization_password`: (Required) App specific password to use for notarization. See [here](https://support.apple.com/en-us/102654) for steps on how to create one.
- `notarization_team_id`: (Optional) App Store Connect Team ID for notarization. If not provided, this will be automatically extracted from the Xcode project. The Team ID provided here should match the Team ID of the certificate.
- `build_certificate_base64`: (Required) Your Apple Developer ID Certificate encoded in base64. Follow the steps in [this tutorial](https://localazy.com/blog/how-to-automatically-sign-macos-apps-using-github-actions) to get the base64 string of your certificate.
- `p12_password`: (Required) Password for the `Developer ID Application` certificate.

#### Example Usage

```yaml
name: Archive and Notarize MacOS App
uses: PreternaturalAI/github-action/preternatural-archive@main
with:
  xcode-version: '16'
  notarization_username: ${{ secrets.NOTARIZATION_USERNAME }}
  notarization_password: ${{ secrets.NOTARIZATION_PASSWORD }}
  notarization_team_id: ${{ secrets.NOTARIZATION_TEAM_ID }}
  build_certificate_base64: ${{ secrets.BUILD_CERTIFICATE_BASE64 }}
  p12_password: ${{ secrets.P12_PASSWORD }}
```
