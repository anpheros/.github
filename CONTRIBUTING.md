# Contributing

Thank you for helping improve the Anpheros SDKs and examples.

## Before you start

- **Bugs:** open an issue in [anpheros-sdk](https://github.com/anpheros/anpheros-sdk/issues) with the SDK, its version,
  your runtime (Node, browser, Deno, Bun, Dart or Flutter version), what you did, what you expected and what happened.
- **Ideas and new features:** open an issue first, so we can agree on the approach before you write code.
- **Security issues:** follow the [security policy](SECURITY.md) instead of opening an issue.

## Working on the code

```bash
# TypeScript SDK
cd ts && npm ci && npm test

# Dart / Flutter SDK
cd dart/anpheros_sdk && dart pub get && dart analyze && dart test
```

- Keep the two SDKs in step: a new endpoint or option belongs in both, with the same shape.
- Add or update tests for what you change.
- Test only against the **sandbox** with synthetic patients. Never commit keys, tokens or real patient data, and do not
  paste them into issues or pull requests.

## Pull requests

Pull requests are welcome. The public repository is published from the Anpheros codebase, so maintainers apply
accepted changes there and publish them with the next release; your contribution is credited in the change history.

By contributing, you agree that your contribution is licensed under the Apache License 2.0, like the SDKs.

## Behaviour

Please follow our [code of conduct](CODE_OF_CONDUCT.md).
