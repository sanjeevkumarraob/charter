# Codebase map: <repo>

Last reviewed: <YYYY-MM-DD> by <name>

Tag each section `observed` (read in code) or `confirmed` (a person on the owning team agreed);
note individual exceptions only. This map grows one change at a time: cover the areas you
touch, and refresh it when it is over 90 days old or a change touches an area it misses.

## Purpose  <observed | confirmed>
<What this system does, for whom, in two or three sentences.>

## Architecture  <observed | confirmed>
<Main modules or services, how a request flows through them, external systems.>

| Module / folder | What it does | Owner |
|-----------------|--------------|-------|

## Established patterns  <observed | confirmed>
Follow the exemplar. Do not introduce a second way of doing any of these without a
docs/sdlc/decisions.md entry. Delete rows that do not apply; add rows for this repo's own
units of change.

| To do this | Follow this exemplar | Rules |
|------------|---------------------|-------|
| Add an API endpoint | `<path>` | |
| Add a DB query / repository | `<path>` | |
| Publish or consume an event | `<path>` | |
| Add a background job | `<path>` | |
| Handle and log errors | `<path>` | |
| Read config / secrets | `<path>` | |
| Write a unit test | `<path>` | |
| Write an integration test | `<path>` | |
| Add a CLI command or flag | `<path>` | |
| Produce machine-readable output | `<path>` | |
| Add an exported library function | `<path>` | |
| Add a UI screen or component | `<path>` | |

## Conventions  <observed | confirmed>
- Naming:
- Folder layout:
- Dependencies we use (and ones we deliberately avoid):

## Commands  <observed | confirmed>
| Purpose | Command | Healthy output | Notes (flaky, slow, needs docker) |
|---------|---------|----------------|-----------------------------------|
| Build | | | |
| Unit tests | | | |
| Integration tests | | | |
| Lint | | | |
| Run locally | | | |

## Danger zones  <observed | confirmed>
| Path | Why | Rule |
|------|-----|------|
| | generated | never edit by hand |
| | frozen / legacy | changes need owner sign-off |
| | shared with other teams | contract changes need their review |
| | no tests | add characterization tests before changing |

## Contracts other systems depend on  <observed | confirmed>
<Public APIs, events, CLI output and exit codes, exported functions, DB tables read by
others, files or reports exported. Changing or removing any of these makes a change High-risk.>

## Deploy and roll back  <observed | confirmed>
<How it ships, how to undo a release, feature-flag system if any.>
