# CLAUDE.md

## Project
Lotus Sutra Lite – a stripped-down version of the broader sutra_matrix app. Focus exclusively on the Lotus Sutra; ignore all other sutras for now.

## Git Workflow
- ALWAYS commit directly to main. Never create a branch unless explicitly told to.
- After every numbered task item, stage all changes and push directly to main.
- Bump the patch version (vX.Y → vX.Y+1) in the HTML meta/comment header with each commit.
- Commit message format: `vX.Y – [brief description]`
- Never open a PR. Never use `git checkout -b`.
