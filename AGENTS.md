# {{NAME}} — operating contract

Operating contract for AI work in this repo; the global `~/AGENTS.md` still applies. {{NAME}} is TODO(one sentence: what this is). Live at {{DOMAIN}}.

Work here follows the CONVERGE cycle and delivery discipline in `FLEET.md` (windwardline/windwardline) — find → refute → verify yourself → fix → re-rank → test → update → report; enumerate the gates rather than counting them, stage explicit paths, validate before mutating, preserve standing claims, derive populations rather than curating them, and never let a harness failure read as the subject refusing. `FLEET.md` governs where it and this summary differ.

## Stack — do not substitute without flagging

TODO(framework + notable deps with versions worth pinning or flagging)

## Commands

TODO(exact dev/test/lint/typecheck/build commands)

## Gates — CI in order

TODO(ci.yml steps in order). Push to main deploys production. A parallel `security.yml` (PRs, pushes, weekly cron) gates Semgrep and secret scan; a post-deploy job asserts the production security headers. An advisory Claude review runs on every same-repo PR via `claude-review.yml`, which deliberately calls the fleet reusable at `@main` — one merge updates every repo. It activates only when the `CLAUDE_CODE_OAUTH_TOKEN` secret is present — reviews bill the owner's Claude subscription, not Console credits; fork PRs never receive secrets, so they skip it by security design.

A fourth workflow, `dependabot-auto-merge.yml`, is byte-identical in every fleet repo that takes it and merges nothing itself: on a pull request to `main` it arms GitHub's native auto-merge, so the branch ruleset stays the only thing that decides whether a merge happens. It acts only on `dependabot[bot]` PRs raised in this repo under the `windwardline` owner — keyed to the PR author rather than `github.actor`, so a manual re-run cannot disable the guard, and forks are excluded without naming a repo. It asserts the gate rather than assuming it, because `gh pr merge --auto` degrades to an immediate merge when the repo has auto-merge off or no required checks; fleet-template deliberately carries neither, so here every Dependabot PR is held for a human by design rather than erroring. It also holds on a `no-automerge` label, a release that changed maintainers, a pre-1.0 bump with no compatibility contract (Dependabot labels 0.9.1 → 0.10.0 "minor"), empty or unverifiable metadata, or a major — which it labels `deferred-major` before any hold can fire — and withdraws a previously armed auto-merge when a rebase turns a compliant PR non-compliant. The credential upgrades itself: with `FLEET_AUTOMERGE_APP_ID` and `FLEET_AUTOMERGE_PRIVATE_KEY` present as Dependabot secrets, not Actions secrets — a Dependabot-triggered run cannot read Actions secrets and resolves them to an empty string — it mints a GitHub App token; otherwise it falls back to `GITHUB_TOKEN`, whose pushes create no workflow run at all, so the run summary names which credential was used. Its job id carries no `name:`, so the check renders exactly `dependabot-auto-merge`, the string the fleet conformance checker excludes from its required-checks audit: this lane must never become a required check.

Every third-party `uses:` in these workflows is pinned to a full commit SHA and carries a trailing comment naming an immutable tag that SHA actually carries — `# v7.0.1`, never a floating major (`# v7`). The comment is the only readable version signal when a Dependabot bump rewrites forty hex characters, and a major alias goes stale in place the moment upstream re-points it. The fleet conformance checker resolves every pin and fails the comment that disagrees.

## Laws

- TODO(the 3-6 non-obvious facts a fresh agent would get wrong — encode what the gates cannot catch: silent failure modes, generated files, deliberate exceptions, spec locations that are source of truth)
