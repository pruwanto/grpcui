# Progress

## Done
- 2026-03-26: Inspected repository structure, build files, Dockerfile, release scripts, and key Go packages.
- 2026-03-26: Confirmed hidden repo automation files already exist for CircleCI, Dependabot, and GoReleaser.
- 2026-03-26: Identified the major runtime layers: CLI, root library package, standalone handler package, embedded assets, and test server fixtures.
- 2026-03-26: Initialized the memory-bank baseline for future sessions.
- 2026-03-26: Added Azure Pipelines YAML for dev and release ACR publishing with separate trigger rules and explicit Docker CLI push steps.
- 2026-03-26: Updated the Docker builder stage to Go 1.24.1 so the new publish pipelines can build against the module's declared toolchain.

## In Progress
- 2026-03-26: Reviewing the new Azure YAML against branch policy, image naming, and variable-group assumptions.

## Todo
- Confirm the Azure DevOps project has the `global-settings` variable group and the expected ACR variables.
- Register the two YAML files as separate Azure Pipelines and connect them to the intended branch/tag triggers.
- Decide whether Docker Hub release steps in `releasing/README.md` should eventually gain an ACR counterpart.

## Risks
- If release tags are introduced without branch validation, images could be published from unintended commits.
- The repo's documented release process still points at Docker Hub, so operator docs can drift from the new Azure ACR workflow unless they are updated.
- If the remote branch strategy differs from the current local `master`/assumed `dev` model, the release or dev triggers will need to be adjusted before activation.
