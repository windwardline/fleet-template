# {{NAME}} — operating contract

Operating contract for AI work in this repo; the global `~/AGENTS.md` still applies. {{NAME}} is TODO(one sentence: what this is). Live at {{DOMAIN}}.

Work here follows the CONVERGE cycle and delivery discipline in `FLEET.md` (windwardline/windwardline) — find → refute → verify yourself → fix → re-rank → test → update → report; enumerate the gates rather than counting them, stage explicit paths, validate before mutating, preserve standing claims, derive populations rather than curating them, and never let a harness failure read as the subject refusing. `FLEET.md` governs where it and this summary differ.

## Stack — do not substitute without flagging

TODO(framework + notable deps with versions worth pinning or flagging)

## Commands

TODO(exact dev/test/lint/typecheck/build commands)

## Gates — CI in order

TODO(ci.yml steps in order). Push to main deploys production. A parallel `security.yml` (PRs, pushes, weekly cron) gates Semgrep and secret scan; a post-deploy job asserts the production security headers. An advisory Claude review runs on every same-repo PR via `claude-review.yml`, which deliberately calls the fleet reusable at `@main` — one merge updates every repo. It activates only when the `CLAUDE_CODE_OAUTH_TOKEN` secret is present — reviews bill the owner's Claude subscription, not Console credits; fork PRs never receive secrets, so they skip it by security design.

Every third-party `uses:` in these workflows is pinned to a full commit SHA and carries a trailing comment naming an immutable tag that SHA actually carries — `# v7.0.1`, never a floating major (`# v7`). The comment is the only readable version signal when a Dependabot bump rewrites forty hex characters, and a major alias goes stale in place the moment upstream re-points it. The fleet conformance checker resolves every pin and fails the comment that disagrees.

## Laws

- TODO(the 3-6 non-obvious facts a fresh agent would get wrong — encode what the gates cannot catch: silent failure modes, generated files, deliberate exceptions, spec locations that are source of truth)
