# ihat-identity-assertion-contracts

Verify a short-lived device identity assertion against an explicitly pinned issuer and audience.

## What you can do

- Validate pairwise subject, device and revocation context.
- Enforce bounded validity and privileged-consumer requirements.

## Current scope

The consumer supplies trusted time and keys. An assertion is not a credential export or permission grant.

Package distribution is not activated by this documentation. Use the checked-in source and the declared dependency versions; published availability must be verified separately.

## Getting started

Install Rust 1.97 or newer and make the declared dependencies available. Use the configured private registry when a dependency is not distributed publicly. Run from this repository:

```sh
cargo test --locked
```

## Documentation and source

[Usage guide](docs/getting-started.md)

[Schemas](schema) · [Implementation and public interfaces](src) · [Verification cases](tests) · [Contributing](CONTRIBUTING.md) · [Security reporting](SECURITY.md) · [License](LICENSE) · [Attribution notices](NOTICE)
