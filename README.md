# fleet-template

The starting point for every new Windward Line repository. The template seeds
the fleet standard's artifacts; the authority is `FLEET.md` in
`windwardline/windwardline`, enforced by the conformance checker there. A
template can go stale. The checker cannot.

## Creating a repository

New repositories will be created through the canonical fleet bootstrap command
maintained in `windwardline/windwardline`. That command is forthcoming, not yet
part of the standards repository's `main` branch. Until it lands, follow the
creation procedure currently published by `FLEET.md`; do not reconstruct the
retired manual creation flow from this repository's history.

The bootstrap will turn this seed into a concrete repository: select the
recorded visibility, install the house license, replace every placeholder,
write the real ordered CI gates, configure the matching Dependabot and security
jobs, install auto-merge and the exact branch ruleset, propagate the required
credentials, and verify the result against live GitHub state. This template
itself already carries auto-merge, its credential pair, and required checks for
`verify`, `Semgrep CE`, and `Secret scan`.

Before first release, the generated repository must pass the fleet conformance
checker. App-class repositories also need the live dependency and production
header jobs, a committed lockfile, a header contract test, and a CSP tailored
to the application. Any preferred-stack deviation requires owner approval
recorded in the repository's `AGENTS.md`. A launched application must complete
the Labs and portfolio registry update in the same change set. A product that
collects user data must publish and link the fleet privacy notice.

## License

The template ships under MIT (root `LICENSE`). Its purpose is to be copied, so
the license and the "Use this template" button say the same thing.

Repositories created from it are proprietary. `templates/LICENSE` carries the
house notice with the `{{DOMAIN}}` placeholder intact, and step 1 above moves
it into place. Skip that step and the new repo ships MIT by accident.
