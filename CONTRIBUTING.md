# Contributing to Grat

We welcome contributions to Grat!

## Code of Conduct

By participating, you agree to follow our [Code of Conduct](./CODE_OF_CONDUCT.md).

## How to Contribute

1. Fork the repository and create your feature branch.
2. Contributions to the **error taxonomy database** are highly encouraged.
3. For code changes, ensure all tests pass (`cargo test`).
4. Document any new features or API changes.
5. Open a Pull Request with a clear description of your work.

## Development Setup

Refer to the [Requirements section in the README](../README.md#requirements) for environment setup.

### Install Git Hooks

Install `pre-commit` with pipx (install pipx with your OS package manager if needed), then install project dependencies and Rust lint components and enable the repository hooks:

```sh
pipx install pre-commit
pnpm install
rustup component add rustfmt clippy
pre-commit install
```

Hooks run Rust formatting and Clippy for staged Rust changes, and Prettier and the workspace ESLint scripts for staged TypeScript changes. To run them manually against all supported files:

```sh
pre-commit run --all-files
```

## License

Contributions are licensed under the project's MIT License.
