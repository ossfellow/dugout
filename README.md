# dugout

Dugout bootstraps a safe environment to test, validate, and showcase, your new, or updated, GitHub pipeline `workflows` and `actions`, so you can focus on your work instead of test environment setup.

## Digging in

Testing the underlying infrastructure of your `application environment` is often times difficult, and at times complex. Hard dependencies, and tight integrations, between some of these underlying infrastructure components can exacerbate these challenges and increase the blast radius of inevitable mistakes and misses. To minimize the chance of such misses and mistakes, dugout takes care of setting up a clean test environment for evaluating your new or modified GitHub workflows and actions, helping you focus on the outcomes of the pipeline changes you've made. Dugout provides the following services:

- Creates a copy of your repository.
- Pollinates it with the GitHub pipeline artifacts, which you identify (workflows, actions, templates, etc).
- Triggers your workflow to kick-start the tests.
- Cleans GitHub package repository and releases, before test execution.

To perform these services, it relies on a [custom checkout action](./dugout/.github/actions/workflow-change-validation-checkout/action.yml), and a [bootstrap workflow](./.github/workflows/bootstrap.yml), which receives, as input, the list of `to-be-copied-and-tested`, path-qualified, GitHub pipeline artifacts - which you have created in your `dugout fork` - and name of a `reusable workflow` that it will call after test environment is setup.

### Usage Recommendations

- Prefer scaling down your custom workflows, by cherry picking test samples.
- Clearly label the [custom] workflow execution outputs (images, releases, etc).
- Add reports, or extra logging, to custom workflows and actions.
- Seed your custom workflows and actions with context proper baseline test data.
- Attach a post-execution snapshot to your workflow update PR submission.

### What's in the Oven?

[wasmCloud](https://wasmcloud.com) - a [CNCF incubating project](https://www.cncf.io/projects/wasmcloud/) - is to [WebAssembly applications](https://github.com/WebAssembly/component-model?tab=readme-ov-file) what Kubernetes is to containers. The core platform is written in Rust language, and shares the same repo with platform extensions, which implement WASI standard and custom interfaces.

Like any other large project with many moving pieces, wasmCloud has a complex pipeline setup, which has grown organically over the lifetime of the project. This makes it difficult to introduce changes with strict requirements while isolating the scope.

The changes needed to support build attestations, and artifact SBOM generation, is one of such cases; to meet SLSA v1.0 Build Level 2 - if build isolation cannot be afforded - or SLSA v1.0 Build Level 3 - [if build could be isolated into reusable workflows](https://docs.github.com/en/actions/security-for-github-actions/using-artifact-attestations/using-artifact-attestations-and-reusable-workflows-to-achieve-slsa-v1-build-level-3) - require changing multiple jobs to keep attestations as close to build steps as possible, and container image transformations to properly link built artifacts and images to their stored SBOMs and attestation records.

This repo provides a test environment to test and validate the following new, or enhanced, features:

- Testing binary and image attestations, in `wasmcloud.yml` workflow.
- Testing binary and image attestations, in `provider.yml` workflow.
- Testing SBOM generation and attestation for binaries and images produced by aforementioned custom workflows and their actions.
- OCI image transformation and metadata enrichment.
- Publishing all generated attestation and SBOM artifacts as searchable OCI artifacts.
- Validation and demonstration reports to measure the quality of change outcomes.
