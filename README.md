# Preternatural GitHub Actions

This repository contains GitHub Actions for use with [Preternatural CLI](https://github.com/PreternaturalAI/CLI-release).

# Actions

## `preternatural-build-action`
Runs the Preternatural CLI build command on your repositories.

```yaml
- uses: PreternaturalAI/preternatural-build-action@v1
```

## `preternatural-archive-action`
Archives and notarizes macOS applications using the Preternatural CLI.

```yaml
- uses: PreternaturalAI/preternatural-archive-action@v1
  with:
    notarization_username: ${{ secrets.NOTARIZATION_USERNAME }}
    notarization_password: ${{ secrets.NOTARIZATION_PASSWORD }}
    build_certificate_base64: ${{ secrets.BUILD_CERTIFICATE_BASE64 }}
    p12_password: ${{ secrets.P12_PASSWORD }}
```

# Documentation

See [USING.md](./USING.md) for detailed documentation, examples, and troubleshooting.
