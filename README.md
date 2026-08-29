# @odigos/opentelemetry-node

This is a branch for the legacy `nodejs-community-14` distro, which uses
OpenTelemetry SDK version 1.
It supports Node.js >= 14, which some Odigos users are still using.

Since OpenTelemetry stopped supporting Node 14 and stopped maintaining the SDK
v1 line, this branch is not expected to be updated regularly — it is left as an
option for existing users during a migration period.

## Publishing a new version

The release workflow lives on `main` (so it is available in the Actions UI) and
always builds from this `nodejs-community-14` branch.

1. Merge any needed changes into `nodejs-community-14`.
2. In GitHub Actions on this repo, run **Publish Legacy 14**
   (`.github/workflows/publish-legacy-14.yml` on `main`). It takes no inputs —
   just trigger `workflow_dispatch`.
3. The workflow will:
   - Find the latest git tag matching `nodejs-community-14/v0.0.*`
   - Bump the patch (for example `nodejs-community-14/v0.0.16` → `nodejs-community-14/v0.0.17`)
   - Create and push that git tag
   - Build and push multi-arch (`linux/amd64`, `linux/arm64`) images tagged with
     the bare version and `latest` to:
     - `public.ecr.aws/odigos/agents/nodejs-community-14`
     - Depot registry `agents/nodejs-community-14`
4. After this community image is published, release the enterprise legacy 14
   agent next (Publish Legacy 14 in `ebpf-nodejs-instrumentation`), which
   consumes this community base.

Git tags are namespaced with the `nodejs-community-14/` prefix so they do not
collide with mainline `v0.0.x` tags. Docker image tags use the bare version only
(for example `v0.0.17`).
