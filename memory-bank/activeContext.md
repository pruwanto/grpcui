# Active Context

## Current Focus
- 2026-03-26: Finalize the newly added Azure ACR publish pipelines and verify they match the repo's existing Docker/release behavior.
- 2026-03-26: Keep the Azure publish flow aligned with the existing CircleCI validation split instead of duplicating CI logic.

## Recent Changes
- 2026-03-26: Initialized `memory-bank/` with project brief, product context, system patterns, tech context, active context, and progress tracking.
- 2026-03-26: Added `azure-pipelines-dev.yml` and `azure-pipelines-release.yml` using the shared ACR variable pattern from the Rust ACR skill.
- 2026-03-26: Updated `Dockerfile` builder stage from Go 1.23 to Go 1.24.1 so container builds align with `go.mod`.

## Next Actions
- Decide whether Azure should stay publish-only or also absorb validation that currently lives in CircleCI.
- Confirm the target `dev` branch really exists in the remote and should remain the non-release publish branch.
- Confirm whether the image repository name should stay `grpcui` or include an organization-specific suffix/path.

## Blockers
- Azure variable group contents are assumed but not verified in the target Azure DevOps project.
- The chosen Azure hosted agent (`ubuntu-latest`) is a portability default and may need to be swapped if the environment requires a self-hosted pool.
