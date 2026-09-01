# {{NAME}} — operating contract

Operating contract for AI work in this repo; the global `~/AGENTS.md` still applies. {{NAME}} is TODO(one sentence: what this is). Live at {{DOMAIN}}.

Work here follows the CONVERGE cycle and delivery discipline in `FLEET.md` (windwardline/windwardline) — find → refute → verify yourself → fix → re-rank → test → update → report; enumerate the gates rather than counting them, stage explicit paths, validate before mutating, preserve standing claims, derive populations rather than curating them, and never let a harness failure read as the subject refusing. `FLEET.md` governs where it and this summary differ.

## Stack — do not substitute without flagging

TODO(framework + notable deps with versions worth pinning or flagging)

## Commands

TODO(exact dev/test/lint/typecheck/build commands)

## Gates — CI in order

Every workflow this repository runs is named here by filename: `ci.yml`, `security.yml`, `claude-review.yml`, and `dependabot-auto-merge.yml`. `ci.yml` runs on pushes and pull requests against `main`; its `verify` job checks `index.html` when one exists and always validates `vercel.json`. Replace those placeholder gates with the generated repo's real ordered gates before release. `security.yml` runs Semgrep CE and Secret scan on pull requests, pushes to `main`, manual dispatch, and the Monday schedule. Secret scan also runs the fleet action-pin verifier. Its dependency and production-header jobs remain commented templates until a generated repo meets their stated prerequisites.

An advisory Claude review runs through `claude-review.yml` only on eligible same-repo pull-request events when `github.event.pull_request.user.login` (the stable pull-request author across reruns) is not `dependabot[bot]` and the base equals the repository's dynamic default branch. The workflow deliberately calls the fleet reusable at `@main`, so one merge updates every repo. It activates only when the `CLAUDE_CODE_OAUTH_TOKEN` secret is present; forks never receive that secret, and fork runs or same-repo runs without the credential skip by security design. Reviews bill the owner's Claude subscription, not Console credits.

Dependabot groups only GitHub Actions updates, under `github-actions`. `fetch-metadata` reports the highest semver change for the entire grouped pull request, so one held member holds the group; arming and holding are per grouped PR, not per action. `dependabot-auto-merge.yml` is the fleet's canonical unattended-update lane and merges nothing itself: on a pull request to `main` it arms GitHub's native auto-merge, so the branch ruleset stays the only thing that decides whether a merge happens. It acts only on `dependabot[bot]` pull requests raised in this repo under the `windwardline` owner, using the pull request author and head repository as its guards. Before arming, it verifies that auto-merge is enabled and at least one required check protects the base branch; the seed template itself has both and requires `verify`, `Semgrep CE`, and `Secret scan`. It holds for a `no-automerge` label, a release that changed maintainers, a pre-1.0 bump with no compatibility contract (Dependabot labels 0.9.1 → 0.10.0 "minor"), empty or unverifiable metadata, or a major. An unrecognized update type is a distinct hold, not a major; only a major receives the `deferred-major` label. Every hold withdraws a previously armed auto-merge when a later push turns a compliant pull request non-compliant. The credential upgrades itself: with `FLEET_AUTOMERGE_APP_ID` and `FLEET_AUTOMERGE_PRIVATE_KEY` present as Dependabot secrets, not Actions secrets — a Dependabot-triggered run cannot read Actions secrets and resolves them to an empty string — it mints a GitHub App token; otherwise it falls back to `GITHUB_TOKEN`, whose pushes create no workflow run at all, so the run summary names which credential was used. Its job id carries no `name:`, so the check renders exactly `dependabot-auto-merge`, the string the fleet conformance checker excludes from its required-checks audit: this lane must never become a required check.

Every third-party `uses:` in these workflows is pinned to a full commit SHA and carries a trailing comment naming an immutable tag that SHA actually carries — `# v7.0.1`, never a floating major (`# v7`). The comment is the only readable version signal when a Dependabot bump rewrites forty hex characters, and a major alias goes stale in place the moment upstream re-points it. The fleet conformance checker resolves every pin and fails the comment that disagrees.

## Laws

- TODO(the 3-6 non-obvious facts a fresh agent would get wrong — encode what the gates cannot catch: silent failure modes, generated files, deliberate exceptions, spec locations that are source of truth)

## Declared gates

The machine-readable gate set. `scripts/fleet-conformance.sh` requires this block
and the workspace done-gate hook runs every `gate:` line before a session may
finish, so what runs is what is written here rather than what a hook guessed from
`package.json`. Each key states its own boundary: `gate:` runs at session end and
must be local and quick; `release:` runs before a pull request and may be slow;
`cadence:` is scheduled or needs the live machine and is run by neither.
The two below are the template's own gates and they run as they stand; replace
them with this project's real gates as it grows one. A placeholder here would
fail CI on the first pull request, which is the point — a repo cannot inherit a
gate list that was never runnable.

```fleet-gates
gate: if [ -f index.html ]; then npx --yes html-validate@9 index.html; else echo "No index.html in this repo yet; this gate activates when one exists."; fi
gate: node -e "JSON.parse(require('fs').readFileSync('vercel.json','utf8'))"
```
