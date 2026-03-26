# Product Context

## Users
- Developers, testers, and operators who need to inspect or invoke gRPC methods without writing custom clients.
- Go teams that want to embed a gRPC UI into internal tools or debug/admin surfaces.

## Problems To Solve
- gRPC APIs are harder to inspect than REST APIs when schemas are only available via reflection or protobuf descriptors.
- Constructing valid protobuf requests, metadata, and TLS settings manually is tedious and error-prone.
- Teams need a quick way to browse schemas and issue requests during debugging, verification, and support workflows.

## UX Expectations
- The CLI should start an HTTP server and expose a usable browser UI with minimal setup.
- The UI should let users switch between structured form input and raw JSON input.
- Responses should surface headers, body, trailers, and gRPC errors in a way that supports debugging.
- Embedded/standalone usage should stay customizable through handler options, templates, CSS, JS, examples, and metadata defaults.

## Acceptance Signals
- A user can point `grpcui` at a server and successfully browse services/methods.
- A user can invoke unary and streaming RPCs, including with TLS or custom metadata.
- A Go integrator can wire the library packages into an existing HTTP server without forking the UI assets.
- A maintainer can publish dev and release container images to ACR with predictable tags and without manual Docker repository path edits.
