# System Patterns

## Architecture Snapshot
- `cmd/grpcui` is the CLI/runtime orchestration layer: parses flags, resolves descriptors, dials the target gRPC server, and serves the UI.
- The root `grpcui` package provides reusable primitives: method/file discovery, HTML web form generation, schema metadata endpoints, and RPC invoke handlers.
- The `standalone` package composes the root package with bundled HTML/CSS/JS/image assets and adds CSRF protection plus handler customization hooks.
- `internal/resources/webform` and `internal/resources/standalone` embed static UI assets that are served by the library/standalone layers.
- `testing/cmd/testsvr` provides a sample gRPC server and generated protobuf assets used by tests.

## Boundaries
- Public API lives in the root `grpcui` package and `standalone`; `internal/*` is implementation-only.
- The CLI depends on the library packages, not the other way around.
- Containerization and release scripts are repository operations layered on top of the Go build; they are not part of the runtime architecture.
- CI/CD responsibilities are split across checked-in automation: CircleCI handles validation, Azure Pipelines handles ACR publication, and GoReleaser plus release scripts still handle GitHub release packaging.

## Shared Patterns
- Descriptor discovery flows through `grpcurl` and `protoreflect`, using reflection, source protos, or protoset files.
- HTTP handlers return JSON payloads for metadata and invocation results, which the browser UI consumes.
- Static assets are embedded in Go and served from in-memory resources rather than requiring an external asset pipeline.
- Release/version stamping for container builds is file-based: Docker expects a repository-root `VERSION` file during image build.
- Current validation strategy is matrix-style across Go versions in CircleCI, with the newest Go version also running the heavier `make ci` target.
- Azure container publishing is split by lifecycle: `azure-pipelines-dev.yml` handles post-merge `dev` pushes, while `azure-pipelines-release.yml` handles `v*` tags after validating the tagged commit is contained in `master`.
- ACR repository assembly follows the Rust ACR pattern: keep `ACR_LOGIN_SERVER` host-only, apply `ACR_PREFIX_DEV` or `ACR_PREFIX_PROD` separately, sanitize slashes, and fail fast on malformed `//` paths.

## Decisions
- 2026-03-26: Keep memory-bank notes factual and repo-local. Rationale: this repo already has a clear code/release structure and needs durable context for future CI/CD work. Impact: memory-bank files should track architecture, build constraints, and pending workflow decisions.
- 2026-03-26: Treat `master` as the current release branch candidate unless the user confirms otherwise. Rationale: the checked-out branch is `master` and the existing docs/scripts still reference `master`. Impact: any release-tag workflow should validate tags against `master` until branch policy is explicitly changed.
- 2026-03-26: Record Docker build prerequisites explicitly. Rationale: the checked-in `Dockerfile` requires a generated `VERSION` file and the builder image must stay aligned with the module's Go version. Impact: any CI/CD workflow or Docker build plan must create `VERSION`, and the current Dockerfile already uses Go 1.24.1 for container builds.
- 2026-03-26: Superseded decision. Earlier planning assumed GitHub Actions would own ACR publication. Rationale: the implementation direction changed to Azure Pipelines for container publication. Impact: keep CircleCI for validation and treat Azure YAML as the ACR publish surface.
- 2026-03-26: Implement Azure publishing as two dedicated pipeline YAMLs instead of one mixed trigger file. Rationale: dev and release flows have different trigger, tag, and branch-validation requirements. Impact: Azure runs stay easier to reason about and `latest` remains reserved for release images.
- 2026-03-26: Keep Azure publishing focused on image build/push and leave general validation in CircleCI for now. Rationale: the repo already has a working validation matrix in `.circleci/config.yml`, while the new Azure pipelines are specifically for ACR publication. Impact: Azure YAML stays smaller and avoids duplicating the existing CI surface.
