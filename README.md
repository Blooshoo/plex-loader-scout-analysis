# Plex-associated Linux loader and VMware-discovery scout

**By Blooshoo · CC BY 4.0 · 19 November 2026.** Evidence reviewed through 19 November 2026, including the completed bounded loader failure experiment. This is a single-incident technical account, not attribution, a vulnerability advisory, or a clean-host declaration.

## Executive summary

A preserved Linux shared-object loader and a statically linked Go reconnaissance program were associated with suspicious owner-level Plex API activity and a remote-carrier/transcoder sequence. Application logs establish the API and transcoder chronology; retained files establish exact sample identities. They do **not** directly establish successful historical scout execution, reconnaissance completion, or remote receipt of its report.

Static review identifies a loader that connects, forks after successful connection, retrieves an exact-size body, and attempts direct execution. With valid configuration, the scout actively discovers network and VMware-like directory identities and attempts JSON reporting even when discovery fails or no VMware candidate exists. Its optional progress endpoint is not an established follow-on code execution channel.

No-NIC, guest-local experiments exercised the unchanged scout with synthetic DNS/LDAP and a local report collector. They confirm reporting on discovery failure, finite immediate-error retries and positive fixture classification. No traced canary-file access, exact canary values in collected reports, or local virtualization-tool inspection was observed in bounded runs; userspace environment reads were not observable. They do not reconstruct the original host, original arguments, or adversary infrastructure. Genuine QEMU/virsh tool presence did not change the top-level synthetic reconnaissance result; the paired runs were not single-variable causal proof.

Zero antivirus detections in archived VirusTotal reports did not mean benign: the vendor scout sandbox ran without required arguments and printed usage. Two bounded loader failure runs now corroborate connection-refused return without fork and a local empty-body HTTP 404 path with fork/session/socket setup and a retrieval request. No next-stage payload was delivered or executed; the success path and real VMware targets were not tested.

## Reading guide

- [Full technical report](REPORT.md): chronology, static analysis, experiments and claim limits.
- [Portable findings](artifacts/findings.json): hand-selected, sanitized structured observations, not raw telemetry.
- [Provenance](PROVENANCE.md): source document identities, transformation policy and verification limits.
- [Redaction and publication review](REVIEW.md): unresolved gates and reviewer checklist.
- [Manifest](MANIFEST.md), with SHA256SUMS: exact package inventory and local byte-integrity checks.

No samples, executable payloads, raw logs, credentials, personal network addresses, host identities or clickable malicious URLs are included. This repository publishes only the reviewed sanitized writeup and derived summaries; restricted incident evidence is not included.

## Attribution and license

Copyright © 2026 Blooshoo. The original writeup and derived summaries are licensed under [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/); see [LICENSE](LICENSE). Attribute “Blooshoo, Plex-associated Linux loader and VMware-discovery scout”, link to this repository, retain the license notice and indicate changes. Third-party facts/identifiers do not imply ownership or endorsement. This work is provided as-is, without warranties.
