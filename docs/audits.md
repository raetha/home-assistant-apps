# Audits

Reviews of the apps and CI. Repeat whenever the owner asks for a full review; record each one below, newest first.

## Checklist for a full review

1. **Open pull requests**, Dependabot's especially (`gh api "repos/raetha/home-assistant-apps/pulls?state=open"`; sessions have the REST API, not GraphQL). Read each diff, check its CI passed (`gh api repos/raetha/home-assistant-apps/commits/<sha>/check-runs`), check a pinned Action's commit is the tag it claims (`git ls-remote https://github.com/<owner>/<action> refs/tags/<tag>`), and squash-merge good ones into `dev` (`gh api -X PUT repos/raetha/home-assistant-apps/pulls/<n>/merge -f merge_method=squash`), never into `main`.
2. **Security alerts:** sessions can't read them (403): ask the owner for their titles.
3. **The apps install:** each app's `version` is an image tag that exists (`https://ghcr.io/v2/raetha/ws2m-hub/tags/list` with an anonymous pull token), `image:` has no placeholder but `{arch}`, the lint workflow passes, and an install in the devcontainer starts.
4. **Options match the source project:** every option in `config.yaml` is read by ws2m's `hub/service.sh`, with the same name, and `DOCS.md` and `translations/en.yaml` describe the same set.
5. **Security of the design:** app permissions (`usb`, `services`, host access) are the minimum the hub needs; nothing secret in the docs.
6. **Workflows:** top-level `contents: read`, write permissions only on the jobs that need them, actions pinned to commits, no `${{ }}` inside `run:` scripts, dispatched input validated before use.
7. **Leftovers:** stale docs, options nobody reads.
8. **Record it** below.

## 2026-11-02: move to the shared repository workflow

Not a full review: what was found while moving to `dev`/`main`.

- **Neither app could install ws2m 4.x.** WS2M Hub's version `4.0.1` wasn't an image tag (4.0.1 was published as `v4.0.1`), and WS2M Hub (devel) had `image: "ghcr.io/raetha/ws2m-hub:{version}"`, which Supervisor can't format, with versions (`4.0.1-devel.<date>`) that were never image tags. Fixed: WS2M Hub is set to `v4.0.1` until the next release (ws2m now also tags releases `X.Y.Z`), and the devel app pulls `devel` until ws2m's first test build under the new workflow sets `<version>-dev.<build>`.
- The apps' `usb_dongle` option wasn't read by ws2m 4.x (`hub/service.sh` read `dongle`), so the hub started without a dongle. Fixed in ws2m (it reads either name); in the release after 4.0.1 and the next test build.
- The two sync workflows pushed straight to `main`, which the new `main` ruleset refuses; replaced by one `sync.yml` that merges a pull request. Lint actions pinned to commits; Dependabot targets `dev`; `FUNDING.yml` added.
