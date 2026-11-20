# Plex-associated Linux loader and VMware-discovery scout

**By Blooshoo · CC BY 4.0 · 19 November 2026.** Single-incident technical writeup. Not attribution, not a vulnerability advisory, and not a statement that any host is clean.

## TL;DR

- Someone used a home Plex Media Server's API with owner-level access and abused its transcoder to drop two Linux binaries.
- **Loader** (ELF shared object): connects out, forks, downloads a body of *exactly* 6,287,522 bytes, then `execve`s it. That byte count matches the scout exactly.
- **Scout** (static Go binary, `plexgoods/cmd/vcenterscan`): maps the local network, probes ports, does DNS classification and anonymous LDAP RootDSE queries looking for VMware vCenter. It **reports home even when it finds nothing**.
- Archived VirusTotal results showed **0 detections for both**. That doesn't make them benign: the vendor sandbox ran the scout without its required arguments, so it just printed usage and exited.
- Our read: someone is scanning compromised home media servers to find ones that sit next to VMware infrastructure.

## If you run Plex

These are generic hygiene steps that follow from this incident. They are not a validated detection procedure.

1. **Check for the hashes below** anywhere on the host, especially under Plex's data directories and in temp paths.
2. **Review server preferences** for changes you didn't make, particularly transcoder or debug options (this incident involved a `dump-yuv`-style write via the transcoder) and anything touching codec loading.
3. **Audit plugins and Companion-related activity** in Plex logs. Look for denied preference/resource requests, followed later by successful owner-level ones.
4. **Look for unexplained transcoder jobs**, especially short ones that exit 0 right after preference changes, or scanner jobs that run unusually long (about 2+ minutes here).
5. **Assume the token is burned** if any of that matches. Sign out all devices, change the account password, enable 2FA, and rebuild the host rather than trusting cleanup. This loader's success path was never tested, and its next stage could be anything.

## Indicators

| Role | SHA-256 | Bytes |
|---|---|---:|
| Loader (x86-64 ELF .so) | `c7d514f7aa568b42380b18e6536d58e746e262d61b59b07e9b4ff3290018b050` | 1,036,800 |
| Scout (x86-64 static Go ELF) | `409301c30b0fd8571345f150b352bac8f97de72cff61703c3ac54c3b58bd33ca` | 6,287,522 |

Weaker, build-specific identifiers: Go module `plexgoods`, package `plexgoods/cmd/vcenterscan`, scanner format string `vcenterscan-go-%s-%s/3.2.2`, and the loader requesting the path `/agent.exe`. On their own, ports 8999/9090 and generic Go/LDAP strings are **not** reliable indicators. Network endpoints are deliberately omitted.

## What's proven vs. not

| Claim | Status |
|---|---|
| Suspicious owner-level Plex API activity and transcoder sequence | Supported by application logs |
| Exact sample identities | Supported by retained files |
| Loader connect → fork → size-gated download → exec flow | Static analysis, with failure paths confirmed in a lab |
| Scout reports even on discovery failure | Confirmed in isolated lab runs |
| Scout actually ran on the original host and its report reached the attacker | **Not proven** |
| How owner authorization was obtained | **Unresolved** |
| A universal pre-auth Plex RCE | **Not claimed** |

Lab work ran the unchanged samples in disposable, no-network QEMU guests with fake DNS/LDAP/HTTP services. No external endpoint was ever contacted. No payload was delivered, and no real VMware target was tested.

## Reading guide

- [Full technical report](REPORT.md): chronology, static analysis, experiments and claim limits.
- [Portable findings](artifacts/findings.json): hand-selected, sanitized structured observations, not raw telemetry.
- [Provenance](PROVENANCE.md): source document identities and transformation policy.
- [Redaction and publication review](REVIEW.md): review status and checklist.
- [Manifest](MANIFEST.md) and `SHA256SUMS`: package inventory and integrity checks.

This repository contains no samples, executable payloads, raw logs, credentials, personal network addresses, host identities or malicious URLs.

## Attribution and license

Copyright © 2026 Blooshoo. The writeup and derived summaries are licensed under [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/); see [LICENSE](LICENSE). Attribute “Blooshoo, Plex-associated Linux loader and VMware-discovery scout”, link to this repository, keep the license notice, and indicate changes. Third-party names and identifiers don't imply ownership or endorsement. Provided as-is, without warranties.
