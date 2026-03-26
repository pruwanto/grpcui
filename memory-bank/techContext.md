# Tech Context

## Stack
- Go module `github.com/fullstorydev/grpcui`
- Go language version `1.24.0` with `toolchain go1.24.1`
- Key libraries: `github.com/fullstorydev/grpcurl`, `github.com/jhump/protoreflect`, `google.golang.org/grpc`, `google.golang.org/protobuf`
- Frontend assets are embedded static files, including jQuery and jQuery UI for the standalone web UI
- Docker multi-stage build with a `scratch` runtime image
- Make-based local CI/release helpers plus `goreleaser` for GitHub release artifacts
- Existing CI config in `.circleci/config.yml`; existing dependency automation in `.github/dependabot.yml`
- Azure Pipelines definitions for ACR publishing in `azure-pipelines-dev.yml` and `azure-pipelines-release.yml`

## Setup
- Standard build/test entrypoint is Go tooling from the repo root.
- `make ci` runs dependency sync, formatting, code generation verification, vet/static analysis, and tests.
- `make docker` writes `VERSION`, builds the image, and removes `VERSION`.
- `make generate` installs protoc plugins and uses `download_protoc.sh` to fetch the pinned protoc version when needed.
- CircleCI currently runs `make test` on Go 1.22 and 1.23, and `make ci` on Go 1.24.
- Azure dev publishing expects branch pushes to `dev`, writes `VERSION=dev-<12charsha>`, and pushes `dev-<12charsha>` plus `dev-latest`.
- Azure release publishing expects `v*` tags, verifies the tagged commit is in `origin/master`, writes `VERSION=<tag>`, and pushes `<tag>` plus `latest`.

## Constraints
- The checked-in `Dockerfile` copies `VERSION`; builds will fail unless the workflow creates that file first.
- The Docker builder image is now pinned to `golang:1.24.1-alpine` so container builds match the module's Go toolchain requirement.
- Hidden repo config still includes CircleCI validation and GoReleaser release packaging alongside the newly added Azure Pipelines YAML.
- Existing release documentation still targets GitHub releases, Docker Hub image publication, and Homebrew updates; ACR publish automation now exists in Azure YAML but the operator docs have not been updated yet.
- `make ci` and `make test` both include dependency-related steps that can mutate module files, so they may need adjustment before being used as strict GitHub publish gates.
- Azure publishing relies on the shared variable group `global-settings` with `ACR_LOGIN_SERVER`, `ACR_USERNAME`, `ACR_PASSWORD`, `ACR_PREFIX_DEV`, and `ACR_PREFIX_PROD`.

## Key Commands
- `go test ./...`
- `make ci`
- `make install`
- `make docker`
- `make generate`
- `GITHUB_TOKEN=... ./releasing/do-release.sh vX.Y.Z`
