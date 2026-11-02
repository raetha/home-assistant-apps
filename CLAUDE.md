# Notes for Claude sessions

Raetha's Home Assistant app repository. Its apps run images built elsewhere: **WS2M Hub** (`ws2m-hub/`, releases) and **WS2M Hub (devel)** (`ws2m-hub-devel/`, test builds), both from raetha/wyzesense2mqtt. The canonical guide is `docs/development.md`. This file is the short version of what a new session needs.

## How we work

- Sessions push to this repository: work on `dev` or a `try/<topic>` branch and open pull requests. No zip packages.
- **Branches** (`docs/development.md` → "Branches and publishing"): never push to `main`. Supervisor installs apps from `main`, so merging into `main` is what publishes a change. Work goes on `dev`; experiments on `try/<topic>` off `dev`. Repository changes reach `main` in a pull request from `dev`, opened only when the owner asks, which he squash-merges.
- **App versions are set by `.github/workflows/sync.yml`**, not by hand: ws2m dispatches each release and test build, and the workflow merges a pull request into `main` (and brings `dev` along). Don't edit `version:` in an app's `config.yaml` unless asked; a manual run of the workflow is the way to set one by hand.
- App behaviour (options, startup) lives in the source project: `hub/service.sh` in raetha/wyzesense2mqtt reads `/data/options.json`. Option or schema changes here need the matching change there.
- Keep an app's `README.md`, `DOCS.md`, `translations/en.yaml` and `config.yaml` consistent in the same change.

## Validate

- The `Lint` workflow runs Home Assistant's app linter (`frenck/action-addon-linter`) on every app, on every push and pull request.
- `actionlint .github/workflows/*.yml` after changing a workflow.

## Hard-won facts

- Supervisor pulls `<image>:<version>` from `config.yaml`: the version must be an existing image tag (ws2m releases `X.Y.Z`; test builds `X.Y.Z-dev.<build>`; 4.0.1 only exists as `v4.0.1`). `image:` may hold `{arch}` only; any other placeholder breaks the app.
- Supervisor offers an update whenever the version string changes (it doesn't compare them).
- `repository_dispatch` runs `main`'s copy of a workflow.
- The `main` ruleset requires pull requests, so workflows can't push to `main`; `sync.yml` opens and merges one, which needs "Allow GitHub Actions to create and approve pull requests" on.
- Sessions can't read Actions settings, secrets or security alerts, or rename branches, through the proxy: ask the owner.
