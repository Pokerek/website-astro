---
change_id: v1-release-staging
title: V1 release staging
status: implementing
created: 2026-07-23
updated: 2026-07-23
archived_at: null
---

## Notes

<!-- Free-form notes for this change: links, ad-hoc context, decisions that don't belong in research/frame/plan. -->

### Phase 1 — observed release topology (2026-07-23)

**Vercel Production Branch — observed pre-change value: `main`. No edit was required.**

The plan flagged this as the load-bearing unknown: if the Production Branch had been `development`,
every merge to `development` would already have been publishing live to chrobok.dev. It was not.

Evidence (GitHub Deployments API, `gh api repos/Pokerek/website-astro/deployments`), which records the
environment Vercel targets for each ref:

| Ref | Branch | Vercel environment |
| --- | --- | --- |
| `5a9b9ce` (`docs: add roadmap v1`) | tip of `origin/main` | **Production** |
| `c5b902e` (`feat: add husky config (#12)`) | tip of `origin/development` | **Preview** |
| `6b63754` (`docs: add plan for v1-release-staging`) | `docs/v1-release-staging` | **Preview** |

`development` produced a Preview, not a Production, deployment. Production Branch is therefore `main`,
even though GitHub's default branch is `development` — the two settings are independent, as expected.

**Consequence:** PR #12 was *not* a production release. The roadmap's F-02 risk framing was
precautionary rather than understated, and no partial-page window was ever open.

**GitHub protection, confirmed intact:**

- `main` — PR required (`required_approving_review_count: 0`), `required_linear_history: true`,
  `allow_force_pushes: false`, `allow_deletions: false`.
- `development` — unprotected; the protection endpoint returns HTTP 404, which is the intended state
  until F-03 (`inspection-gate`) attaches required status checks.

**Production baseline (pre-Phase-3 merge), captured 2026-07-23:**

- `HTTP/2 200`, serves `Work in progress...`
- `etag: "3ed38505cf3185439ceea33a74be45ab"`
- `last-modified: Wed, 22 Jul 2026 15:13:42 GMT`
- HTML `shasum`: `34695f98d998730b2786a5fbeeab733c8abc44e1`
