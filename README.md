# fleet-template

The starting point for every new Windward Line repository. Create from it:

```bash
gh repo create windwardline/<name> --private --template windwardline/fleet-template --clone
```

The template seeds the fleet standard's artifacts; the authority is
`FLEET.md` in `windwardline/windwardline`, enforced by
`scripts/fleet-conformance.sh` there. A template can go stale — the checker
cannot. Run it before first release.

## After creating a repo

1. Replace every `{{DOMAIN}}` and `{{NAME}}` placeholder (LICENSE, SECURITY.md,
   AGENTS.md, security.yml).
2. Fill in `.github/workflows/ci.yml` with the repo's real gates, and complete
   the AGENTS.md operating contract.
3. App-class repos: swap `.github/dependabot.yml` for the npm+actions form
   (copy from mimic), and uncomment the OSV and Headers live blocks in
   `security.yml`.
4. Enable auto-merge:
   `gh repo edit windwardline/<name> --enable-auto-merge`
5. Create the ruleset (required checks = every PR-running CI and scan job by
   name, linear history, no bypass actors) — copy an existing repo's:
   `gh api repos/windwardline/craft/rulesets --jq '.[0]'` as the shape.
6. Propagate the review secret:
   `security find-generic-password -s anthropic-actions -w | gh secret set ANTHROPIC_API_KEY -R windwardline/<name>`
7. On launch (production URL live): the launch registry rule applies — Labs
   register, portfolio, and both READMEs update in the same change set.
8. Verify: `windwardline/windwardline/scripts/fleet-conformance.sh` passes.
