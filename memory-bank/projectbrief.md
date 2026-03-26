# Project Brief

## Goal
- Provide a browser-based UI for exploring and invoking gRPC APIs from a local CLI or an embedded Go HTTP server.
- Ship both a CLI entrypoint and reusable Go packages for teams that want to embed the UI into their own tooling.

## Non-Goals
- Not a replacement for interactive bidirectional streaming tooling; request streams are composed up front and rendered after execution.
- Not a full deployment platform; container/release automation is ancillary to the core product.

## Success Criteria
- `grpcui` can connect to gRPC servers using reflection, proto sources, or protoset files.
- Users can browse services/methods, submit requests, and inspect headers, trailers, and response payloads in the browser.
- The repository remains buildable as a Go module, testable with standard Go tooling, and releasable as a container image.
- Maintainers can publish environment-specific container images to Azure Container Registry without changing the image repository path logic per environment.

## Scope Notes
- The root `grpcui` package exposes web form generation and RPC metadata/invocation handlers.
- The `standalone` package wraps the lower-level pieces into a self-contained UI handler with bundled assets.
- The `cmd/grpcui` binary is the end-user CLI that hosts the UI and connects to remote gRPC targets.
