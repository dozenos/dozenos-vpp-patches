# AGENTS.md

## Project purpose
A series of `.patch` files applied to upstream VPP (Vector Packet Processing) during the DozenOS VPP build, to add DozenOS-specific features and fixes.

## Tech stack
- Plain text quilt-style patches (numbered `0001-…`, `0002-…`, …) in `patches/vpp/`.
- No build system in tree; patches are consumed by `dozenos/vpp` and/or an internal repository build pipelines.

## Build / test / run
- Apply with `quilt push -a` (or `git am`) inside the upstream VPP source tree.
- No tests in this repo; verification happens in the VPP package build and image-level smoketests run by `dozenos-build`.

## Repository layout
- `patches/vpp/` — numbered patch series, e.g. `0001-linux-cp-add-support-for-xfrm-netlink-notifcation.patch`, `0013-pppoe.-Automated-session-management.patch`, `0020-version-Implement-DozenOS-specific-versioning-format.patch`.
- `LICENSE` — GPL-3.0.
- `README.md` — one-line pitch.
- `.github/` — minimal reusable-workflow wiring.

## Cross-repo context
- Consumed by `dozenos/vpp` (the DozenOS-flavored VPP fork — C) and an internal repository (the integration package). Together with an internal repository's `dozenos-reusable-vpp-build.yml`, this provides VPP fast-path support for DozenOS images.
- Patches touch areas like Linux-CP (`linux-cp-…`), IPsec/XFRM, PPPoE session mgmt, AEAD/AES-GCM crypto.

## Conventions
- One feature/fix per numbered patch; keep them rebased against the upstream VPP tag the build targets.
- Commit / PR title: `component: T12345: description` (Phorge ID mandatory).
- Default branch `rolling`.

## Notes for future contributors
- Patch order matters — renumber carefully on rebase.
- Patches encode DozenOS task IDs (T7770, T7775, T8551, …) in subjects; preserve the `linux-cp-T8XXX-…` style.
- Verify each patch still applies cleanly to the upstream VPP version pinned by the consumer build.
