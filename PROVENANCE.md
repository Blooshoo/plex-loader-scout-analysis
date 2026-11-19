# Provenance and claim map

This is a new sanitized synthesis of finalized local reports, not a verbatim original export or direct reanalysis of binaries. All source documents remain in restricted custody and unchanged. Source names below are case-relative document labels only; original host/user paths are deliberately omitted. Hashes identify reviewed document bytes. Source hashes are not sample hashes and do not independently prove source assertions.

| ID | Document | SHA-256 | Bytes |
|---|---|---|---:|
| S1 | CASE-FILE.md | 9cebc4420a5953bac44347691a9febbe0a6312eceae2a9f053fc43f930fc7d63 | 15139 |
| S2 | EVIDENCE-REGISTER.md | 7ccf749ba8fa31e5b897df8d728af0094184f98ce90b14ec038d4622f2acb02d | 16097 |
| S3 | reports/scout-REPORT.md | 89ebc17028945a9fea57a042b6115218c2e80058461aa52224cf6c31e9332afd | 19143 |
| S4 | reports/scout-GHIDRA-REPORT.md | 31b21d6f36a68c5a3b85cfbd4af75539fdfd2826c8797fad36bb97251cfd427c | 10697 |
| S5 | evidence/static-followup-20261118/LOADER-FINDINGS.md | 78b7d8e90f5fc47b6705c1ece2391c94ebc98b32fe8ee307bab4478de2c7c6aa | 3319 |
| S6 | evidence/chronology-correction-20261118/REPORT.md | 88cb279bafeccd5b8101b39a91eb9e8e18ef20918f6e5efc35e7ff30e756d10d | 10722 |
| S7 | evidence/sandbox-band-experiments-20261118/REPORT.md | 2abdb169614820587ff7bfc6e178fda9d74a439c272caa54f3e061d334ecbf5f | 11891 |
| S8 | evidence/sandbox-canaries-controls-20261118/REPORT.md | 5a029749a913e18a4364bf890325e85e003678e739df1994827f788a9b808296 | 8763 |
| S9 | evidence/sandbox-fuller-20261119/REPORT.md | 5a3e111202688e7d83e51f935277dee9c4aa59e6593cd7be538f97ac3475c002 | 4094 |
| S10 | evidence/immutable-vmtools-20261119/REPORT.md | b6e578d4290482ed6b616fc48817b052f9efe49f41540d65d84ef25efe570a6b | 6542 |
| S11 | evidence/vmtools-presence-20261119/REPORT.md | 8162c63a09a3168d38ab6979acd2ecc53b63281ea174074a326163a2c3b10369 | 2799 |
| S12 | evidence/sandbox-fuller-20261119/FULLER-LIMITS-20261119.md | f4ab9e315adbc7bae5f8849c309e50cd7288176c2cade1237900dae8f933f967 | 3379 |
| S13 | evidence/vt-publication-review-20261118/REPORT.md | a7adb8c5b176382b33449ec4df30092d74e1ac224f9a6c98d05df550508af385 | 5530 |
| S14 | evidence/loader-failure-experiment-20261119/REPORT.md | d7198969466f741f1057e65036dec7e49d2ccf476ce97cf45f0b59b77bd23b78 | 7211 |
| S15 | evidence/loader-failure-experiment-20261119/SUMMARY.json | 87ca9622484444dc3cd3e2180fada20f8cd52622e0a8348a42a870afe2770ac2 | 3835 |
| S16 | evidence/loader-failure-experiment-20261119/STAGE-EXCLUSION-CHECKS.json | 763b4b069b3717e861fcae32c88f1c7ac839723cf51d7293fc8fc95df2d286c3 | 600 |
| S17 | evidence/loader-failure-experiment-20261119/CLEANUP.json | 1168cd4f48db31e04f9dab942f74c30336949918f2c27745cafa162a4869be10 | 49 |
| S18 | evidence/loader-failure-experiment-20261119/guest.py | 5df031b82d8b50c6a8e0c53334ead7ee560f6c55e485cf341141a8aa593c9922 | 7866 |

## Claim mapping

- Samples and incident chronology: S1/S2, corrected by S6.
- Scout static control flow, report schema and decompilation limits: S3/S4.
- Loader ordering, size/status gate and direct execution: S5 supersedes earlier contradictory ordering prose in S13.
- Default callback and HTTP matrix: S7.
- Canary/environment and neutral LDAP comparisons: S8.
- Fuller matched file-presence pair and scope: S9/S12.
- Genuine-tools matched pair: S10; earlier blocked benign preparation is S11, not a specimen result.
- Archived VirusTotal counts, timestamps, voting and coverage caveats: S13, cross-checked with S1/S2.

Loader failure findings and measurement/filesystem scope: S14/S15; no-stage exclusion: S16; final VMM cleanup: S17; exact snapshot field/root/size exclusions: S18. Source S18 is hashed and referenced only, not included as executable publication content.

## Transformation and verification limits

Narrative was rewritten manually with direct sample hashes, selected measurements and source attribution. No raw directory copy, original traces, executable snippets, malicious endpoints, private addresses, victim markers or host identities were copied. Portable findings JSON is hand-selected derived data; it does not claim byte-equivalence with original telemetry. Existing root evidence manifests were not regenerated or altered. Source-document hashes were calculated directly during draft preparation, not copied from memory. The draft does not rehash the preserved multi-gigabyte evidence set.

Loader failure experiment is complete for two bounded failure branches (S14–S18); successful full-chain replay and real VMware targets remain untested. Independent technical/privacy review passed according to the owning-lane receipt; owner approved public Blooshoo/plex-loader-scout-analysis, credited Blooshoo, CC BY 4.0.
