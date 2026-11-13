# Downstream patch inventory

This is a lightweight MIT fork of [openclaw/acpx](https://github.com/openclaw/acpx)
for Mandala's embedded ACP transport. Upstream owns its roadmap and acceptance
decisions. Downstream release readiness does not mean a PR has been accepted.

- Release base: upstream v0.22.0, `1ed28f2842182fdc0526927b8fbe2f5ba906ab2e`.
- Independent contribution base: upstream main, `0701de4e9e31d39ee652453f240eba81b94f483b`.
- Maintained downstream branch: `mandala`. Release: `v0.22.0-mandala.1`.
- The original MIT license remains unchanged. Mandala's application license does
  not apply to this fork or its upstream contributions.

## Patches

| Change                                             | Downstream commit    | Independent upstream contribution                                         |
| -------------------------------------------------- | -------------------- | ------------------------------------------------------------------------- |
| Host-controlled spawning and invalid child cleanup | `778e7ee`, `166e5d2` | [PR 1018](https://github.com/openclaw/acpx/pull/1018), draft API proposal |
| CodeBuddy legacy permission kinds                  | `880ea26`            | [PR 1019](https://github.com/openclaw/acpx/pull/1019)                     |
| Grok no-leader authentication                      | `44298ed`            | [PR 1020](https://github.com/openclaw/acpx/pull/1020)                     |
| Transient session request metadata                 | `819a577`            | [PR 1021](https://github.com/openclaw/acpx/pull/1021), draft API proposal |
| OpenClaw strict session recovery                   | `55a6905`            | [PR 1022](https://github.com/openclaw/acpx/pull/1022)                     |
| Explicit Antigravity question callback             | `3acf5ce`            | [PR 1023](https://github.com/openclaw/acpx/pull/1023), draft API proposal |
| Handshake-only diagnostic metadata                 | `699a808`            | [PR 1024](https://github.com/openclaw/acpx/pull/1024)                     |

The source for the spawning hook is
[Paperclip's acpx patch at ff8ec41](https://github.com/paperclipai/paperclip/blob/ff8ec41e6a1236694d28f463b60c7d136442f9c9/patches/acpx%400.22.0.patch).
Only process creation is adapted; its extension RPC and subagent routing changes
are not included. The complete original MIT notice is in `THIRD_PARTY_NOTICES.md`
and is included in packed releases. All other compatibility fixes are separate
from Paperclip's patch.

## Validation boundary

The combined patch set passed `pnpm run check`, including the repository's
coverage thresholds, and `pnpm run check:docs`. Independent PR branches include
their own focused regression checks. The canonical autoreview helper reviewed
the combined code range at P0–P2 with no accepted actionable findings; it did not
execute code or certify unchanged repository context.

Tests exercise real child processes and ACP fixtures. They are not claims of
live account verification for every CLI, a Windows desktop acceptance run, or
uniform support for every ACP feature. Question callbacks remain opt-in and
cannot inherit automatic permission approvals. Mandala keeps its existing
non-ACP native adapters and its own capability and permission policy.

## Updating

1. Fetch upstream tags and main; inspect release notes and code changes since
   the recorded base. Choose an explicit tested upstream commit.
2. Create an update branch from that base and replay the focused patches. Drop
   patches already covered upstream; preserve stricter session identity and
   permission behavior when resolving conflicts. Keep packaging-only changes
   separate from contributions.
3. Run `pnpm install --frozen-lockfile`, `pnpm run check`, `pnpm run check:docs`,
   and independent review. Run Mandala's complete tests, build, ACP subprocess
   regressions, SSH bundle checks and applicable native CLI acceptance.
4. Publish a new immutable version tag and built npm tarball in GitHub Releases.
   Never replace an existing asset. Update Mandala's exact asset URL and npm
   integrity lock, then verify a clean installation without this checkout.
5. Update this inventory with the new base, remaining patches and PR status.

`pnpm pack` builds the distributable; source archives are not a substitute for
the release tarball. Changes to native CLI versions are tested separately.
Fork Actions are disabled; releases and tests run locally. There is no automatic
scheduled update, merge, publish or installation.
