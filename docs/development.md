# Development

How this app repository is maintained. Users start at the [README](../README.md).

## Layout

| Path | What it is |
| --- | --- |
| `repository.yaml` | The repository's name and maintainer, as Home Assistant shows them. |
| `ws2m-hub/` | WS2M Hub: `config.yaml` (options, schema, image and version), `DOCS.md` (the app's Documentation tab), `README.md`, `CHANGELOG.md` (points to the ws2m releases), `translations/en.yaml`. |
| `ws2m-hub-devel/` | WS2M Hub (devel), the same for ws2m's test builds; its `CHANGELOG.md` points to ws2m's Unreleased section. |
| `.devcontainer.json`, `.vscode/` | Home Assistant's app devcontainer, with tasks to start Supervisor and install a local app. |

The apps don't build images: both run `ghcr.io/raetha/ws2m-hub`, built by raetha/wyzesense2mqtt. Supervisor pulls `<image>:<version>`, so an app's `version` is an image tag.

## Branches and publishing

Supervisor reads `main`, so `main` is what users get: merging into it publishes.

| Branch | What goes there | What a push does |
| --- | --- | --- |
| `main` | Squash-merged pull requests only: from `dev`, and the sync workflow's version updates. | Users' Supervisor sees the change on its next store refresh. Lint. |
| `dev` | Adopted changes to apps, docs and workflows, and Dependabot's merges. | Lint. |
| `try/<topic>` | Experiments that may be dropped. Branched from `dev`. | Nothing by itself; open a (draft) pull request into `dev` to run lint. |

Day to day:

1. Small, settled changes go straight onto `dev`; anything exploratory on `try/<topic>`, squash-merged into `dev` by pull request if adopted.
2. When the owner asks, open a pull request from `dev` into `main` describing what changes for users; the owner squash-merges it.
3. Changes users only get with a new image (anything about how the hub runs) belong in raetha/wyzesense2mqtt; options or schema changed here need the matching change there (`hub/service.sh`).

### Versions: the sync workflow

`.github/workflows/sync.yml` runs when raetha/wyzesense2mqtt dispatches after publishing images:

- `ws2m-release` with `X.Y.Z` sets `ws2m-hub/config.yaml`'s version.
- `ws2m-dev` with `X.Y.Z-dev.<build>` sets `ws2m-hub-devel/config.yaml`'s version.

It opens a pull request into `main` with the change and squash-merges it, then resets `dev` to `main` if `dev` had nothing else, or commits the same change onto `dev`. Repository dispatches run `main`'s copy of the workflow, so changes to it take effect once they're on `main`. **Actions → Sync app versions → Run workflow** sets any app's version by hand (to an existing image tag).

Repository settings (owner, one time):

- **Settings → Rules → Rulesets**: one for `main`, the default branch (no deletion, no force pushes, a pull request before merging, squash only). **Require code scanning results:** CodeQL. **No required status checks:** `sync.yml` opens its pull requests with the workflow token, and GitHub doesn't run workflows for those, so a required check (even `CI passed`) would never report and every app update would stay blocked. The `Lint` workflow still runs on every push, `main` included, and on the owner's pull requests (its `CI passed` job sums it up).
- **Settings → General → Pull Requests**: allow only squash merging, and **Automatically delete head branches** (the sync workflow's `sync/…` branches go too).
- **Settings → Branches → classic branch protection rule for `dev`**: nothing required, deletions not allowed, **force pushes allowed** (the sync workflow resets `dev` to `main`).
- **Settings → Actions → General → Workflow permissions**: **Allow GitHub Actions to create and approve pull requests**, so the sync workflow can open its pull requests.
- raetha/wyzesense2mqtt holds the `HA_APPS_PAT` secret that lets it dispatch here.

### What CI does

- `lint.yml`: Home Assistant's app linter on every app folder, on pushes to `main` and `dev`, every pull request, and daily (the schedule runs `main`'s copy).
- `sync.yml`: above.

## Testing an app locally

Open the repository in the devcontainer (`.devcontainer.json`), run the "Start Home Assistant" task, then "Install App" with the app's folder name. Home Assistant runs on port 7123.

## Audits

Full reviews follow the checklist in [docs/audits.md](audits.md) and are recorded there.
