# zkBrief — Tinfoil enclave configuration

Measured configuration of the zkBrief enclave running on [Tinfoil Containers](https://docs.tinfoil.sh/containers/overview).

Each release measures `tinfoil-config.yml` and publishes the measurement to the Sigstore transparency log. zkBrief clients verify the enclave's attestation against a release of this repository before sending it a sealed reader profile.

## Contents

- `tinfoil-config.yml` — image pinned by digest, resources, environment, secret names, egress allowlist and exposed paths.
- `.github/workflows/` — release workflows.

Secret values are never stored here, only their names. The image is built from a separate source repository.

## Releasing

1. Set `image:` to the digest of the new build.
2. Run the **Tinfoil Release** workflow with a new version tag (e.g. `v0.1.1`). Tags are never reused.
3. Deploy it: `tinfoil container update zkbrief --tag <version>`.
