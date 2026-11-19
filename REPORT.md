# Technical report: two Linux payloads associated with a Plex incident

## 1. Scope, terminology and evidence strength

“Observed” means a retained application record or an instrumented controlled run records the event. “Static capability” describes reviewed instructions/control flow, not proof of historical use. “Assessment” is an explicitly qualified explanation. Negative findings are restricted to the acquisition and execution coverage stated below. A successful parent transcoder exit is not evidence that every detached operation succeeded.

The report concerns two exact binaries. The label “Plex-associated” describes incident context; it does not establish a Plex vendor attribution, a named malware family, or a newly reproduced vulnerability. No attribution to an operator, campaign, country, hosting company or other victim is made.

## 2. Sample identities

| Role | SHA-256 | Bytes | Format |
|---|---|---:|---|
| Shared-object loader | c7d514f7aa568b42380b18e6536d58e746e262d61b59b07e9b4ff3290018b050 | 1,036,800 | Linux x86-64 ELF shared object |
| Reconnaissance scout | 409301c30b0fd8571345f150b352bac8f97de72cff61703c3ac54c3b58bd33ca | 6,287,522 | Linux x86-64 static Go ELF executable |

These values are corroborated by the retained evidence register, static reports and archived VirusTotal projections. This publication preparation did not reacquire or execute sample bytes. Canonical scout hash was checked before/after the cited controlled runs. Hash identity is not provenance of authorship.

Scout compilation metadata reports Go 1.26.8, module plexgoods, package plexgoods/cmd/vcenterscan, and an application scanner format vcenterscan-go-%s-%s/3.2.2. These are embedded identifiers, not authenticated author or product identities. Original Go source was not recovered.

## 3. Corrected incident chronology

Dates below are 2026. Historical application clock accuracy was not independently authenticated. October precedes the local daylight-saving transition; it must not be automatically relabeled PST. November incident clock references use the case's recorded local convention. Vendor timestamps are explicitly UTC. Acquisition dates are not event dates.

| Recorded date/time | Evidence-backed event | Important qualification |
|---|---|---|
| Oct 29 | Identity probes followed by denied preferences/resources requests | An earlier narrow negative search did not establish a clean period |
| Oct 30 03:45 and 07:29 | Companion subscription; nested plugin fetch; subsequent owner-labeled preferences response | Exact historical authorization acquisition remains unresolved |
| Nov 2 13:35–14:36 | Denied probe progresses to nested fetch and owner-labeled API access | Source correlation does not prove a single actor or token |
| Nov 2 14:39:28–31 | Resource access, preference mutation and item response | Supports a malicious staging chain, not an end-to-end reproduced exploit |
| Nov 2 14:39:32.472 | First recorded transcoder start; exit 0 after 0.573 s | Parent duration is not payload download duration |
| Nov 2 14:39:44.757 | Second transcoder start; exit 0 after 0.744 s | Later sample write metadata is compatible with a detached child; not proof |
| Nov 2 around 14:39:45–48 | Scout birth/write/access metadata reported | Identifies temporal consistency, not the accessing process |
| Nov 2 14:40:31 | Preference cleanup recorded | Cleanup does not establish absence of other effects |
| Nov 9–12 | Fourteen paired scanner jobs lasted about 133.6–136.4 s | Compatible with blocking/retry behavior; not observed socket attempts |
| Nov 15 | Diagnostic loading of the library contemporaneously reported, followed by quarantine/service-stop records | Agent narrative, not original invocation/strace; broad “never executed” wording is wrong |
| Nov 17–19 | Preservation, static review, archived reputation review and isolated scout experiments | Later lab execution must not be conflated with historical compromise |

The retained chronology correction found eight short baseline scanner pairs, fourteen long Nov 9–12 pairs, and 32 Nov 15 scanner pairs, of which 28 were killed near 30 s. These are application job associations, not a count of outbound connections. No exact historical packet count, successful callback, or collector downtime is established.

## 4. Initial-access hypothesis and uncertainty

The leading explanation combines a Companion/nested-plugin request chain, possible authorization-material exposure, owner-labeled preferences access, a dump-yuv write/transcoder carrier, and codec autoload. Installed plugin source and log correlation support that hypothesis, but historical source identity, outgoing header bytes, token equality, and remote receipt were not captured. A complete universal preauthentication RCE or zero-day finding is not claimed. No vulnerability identifier or patch-fix assertion is made here.

Remote-carrier delivery does not require an infected local movie. Similar techniques or client labels are not actor attribution. Long job durations and sample timestamps cannot substitute for historical EXECVE, DNS, flow or report-receipt telemetry.

## 5. Loader static findings

The finalized instruction review supersedes an earlier contradictory prose account. Entry calls a socket/connect helper before fork. A negative result returns without fork. Following successful connection, the parent closes its inherited descriptor and returns; the child establishes a session, redirects standard descriptors to the socket and enters the worker.

The worker derives its output context from process mappings and opens a second connection for retrieval. Reviewed instructions validate HTTP/1.x status 200 and compare the received body length with 0x5ff0a2 (6,287,522 bytes). A differing length returns through cleanup rather than setting up execution. Equal length is **not** a cryptographic integrity or authenticity check. The reviewed path constructs arguments and invokes direct execve; failure emits an error and exits through cleanup. Socket-backed standard I/O alone does not establish an interactive shell.

Useful instruction references in the finalized review: entry 0x19e9; first connect syscall 0x1a8f; fork 0x1a00; second connection helper call 0x1e93; length compare 0x2153; execve 0x22e3. These are ELF image-relative virtual addresses, not raw offsets or runtime absolute addresses. The loader has repeated byte clusters; duplicated file offsets must not be promoted into three executed routines.

### Completed bounded loader failure experiment (Nov 19)

Finalized report and selected result/exclusion receipts (S14–S18) document two unchanged-loader runs in separate fresh no-NIC guests. A trusted minimal constructor host loaded the library only inside each disposable guest; no genuine Plex process or host injection was used. All benign acceptance gates preceded sample introduction.

- **Refused connection:** guest-local connect returned ECONNREFUSED; no fork or setsid was traced. Constructor-host/strace run naturally exited 0 after 0.025604 s (about 26 ms).
- **Empty-body HTTP 404:** the first guest-local socket was accepted and held without commands. The loader forked; the child called setsid and dup2 for standard descriptors, opened a second local connection, and requested the inert path /agent.exe. The fixture returned HTTP 404 with Content-Length 0 and closed. Constructor-host/strace run naturally exited 0 after 0.025347 s (about 25 ms). No shell functionality is established.

These timings measure the instrumented constructor-host/strace run, not VM boot, a direct uninstrumented constructor call, or historical incident latency. Whole-VM runs took about 161 s including boot. The tracer followed descendants; benign acceptance separately demonstrated detached-child lifetime coverage and dedicated-UID timeout cleanup, including a preserved initial benign failure caused by missing setsid telemetry before selectors were corrected.

No scout was attached and zero payload bytes were delivered. Each sample trace contained only the trusted constructor-host execve, no second executable and no execveat. No write-mode file open was observed. Selected-root snapshots found no differences in mode, owner/group, size or regular-file SHA-256 across six guest trees (system configuration, home, optional software, temporary trees and Plex service data). Observation scratch, directory metadata and timestamps were excluded; symlink contents were not hashed; per-file 16 MiB and fewer-than-10,000-entry bounds applied. This is **not** a whole-filesystem, memory or no-write proof. Loader hash matched before/after each cell; no live target-UID processes remained after cleanup, and final independent cleanup found no active VMM.

The failure branches corroborate selected static behavior only. No successful download/size-gate/exec path, actual VMware/vCenter target, longer-term operation, historical execution or credential clearance was tested. No external endpoint was contacted; numeric callback details and the victim marker are deliberately omitted.

## 6. Scout static findings

The scout enumerates local interfaces/routes and resolver information, expands discovered private/CGNAT network hints within a host cap, probes TCP ports, performs DNS classification, and can issue anonymous LDAP RootDSE queries. It is active reconnaissance, not merely passive inspection. LDAP identity probing is not credential guessing, authenticated directory modification, or a VMware exploit.

Recovered report fields include host/network/DNS details, scanned host counts, candidates, errors, parsed LDAP identity attributes and base64 raw LDAP response bytes. Review of the JSON marshaler/type linkage distinguishes report schema from unrelated strings.

Reporting follows both successful discovery and discovery failure. No confirmed VMware candidate is required. Discovery errors produce scan_failed reports rather than an unconditional silent exit. The report routine attempts at most three submissions, accepts HTTP 2xx, uses a 120 s HTTP client timeout and increasing retry sleeps. The general 100 ms TCP connect-probe setting does not bound this report wait or the entire discovery operation. Static analysis establishes a minimum one-second candidate-DNS context budget (S4) and a minimum two-second LDAP transaction budget (S3); neither should be represented as a 100 ms total-operation deadline.

Compiled defaults include port 8999, 256 workers and 500 ms TCP timeout. Preserved loader analysis reports overrides including port 9090, 1024 workers and 100 ms. Defaults, labels and historical invocation must remain separate. In particular, report_transport containing shared9090 did not override the experimentally observed default destination port 8999.

The optional stage URL sends best-effort progress GETs and discards response bodies. No follow-on downloaded-code execution was established in that scout path. Optional marker refresh validates a bounded lowercase-hex marker; it is not an established command channel. No scout-side persistence, self-delete, shell launch or install logic was established in the reviewed implementation. That bounded negative does not clear the loader, other stages, unreviewed code or the whole intrusion.

Static review recovered 6,606 function entries and examined main-package control flow. A later Ghidra cross-check decompiled 15 selected functions with 31 returned text exports hash-matched. Automatic Go recovery had known toolchain/type limitations; C-like output was checked against instructions and recovered schemas. Neither source recovery nor complete formal whole-program coverage is claimed.

## 7. Controlled experimental design

Only explicitly authorized analysis copies ran in cited experiments. Each scout cell used a fresh disposable overlay, no-NIC QEMU guest, outer namespace isolation, no host share/clipboard/guest agent, no external network route, no genuine credentials or enterprise services. DNS, anonymous LDAP and HTTP collectors were synthetic and wholly guest-local. Historical numeric callback destinations were mapped only inside the isolated guest; this package omits those unnecessary address values.

Most later cells constrained the unchanged scout to two synthetic hosts, one worker, 100 ms connect probes, LDAP port 389 and an inert synthetic marker. These are not recovered historical arguments. Telemetry consisted of selected strace file/network/process/read/write events with 256-byte strings, full independently captured callback bodies, fixture counters and selected-root file comparisons. No packet capture was acceptance-tested. Synthetic wildcard DNS intentionally overproduced name signals and is a material realism limitation.

Pre-specimen benign gates checked isolation, local HTTP and tracing. Later work also demonstrated an actual sample-free QEMU stop receipt to reaping in about 63 ms. This is not a zero-latency monitor or proof of guest-timeout receipt generation latency. Failed preparation attempts were preserved rather than relabeled sample behavior.

## 8. Experimental results

### 8.1 Default callback and response matrix

An omitted-callback no-target run delivered one 520-byte scan_failed report to the guest-local default port 8999 and exited 0 in about 0.109 s. Thus “no discovered target means no report” is empirically false for the tested configuration.

| Synthetic collector behavior | Attempts / complete bodies | Outcome |
|---|---|---|
| 200, empty response body | 1 / 1 | exit 0, about 0.104 s |
| No listener | 3 / 0 | exit 1, about 6.12 s |
| HTTP 400 | 3 / 3 | exit 1, about 6.13 s |
| HTTP 500 | 3 / 3 | exit 1, about 6.11 s |
| Malformed HTTP | 3 / 3 | exit 1, about 6.12 s |
| Read report then close | 3 / 3 | exit 1, about 6.11 s |
| Read report, withhold response | 2 / 2 observed | killed at 140 s wall cap; eventual outcome unknown |

Immediate-error cells show three total attempts, not three additional retries. A fully received request body with an error response is not application success. Cells were single executions, not replicated causal trials. The stalled run stopped the sequence for review; subsequent work repaired/accepted stop handling before new specimens. No cap was increased to finish that stalled sample. Kernel blackhole and redirect variants remain untested.

### 8.2 Synthetic secrets, files and discovery controls

Six subsequent canary/control cells naturally exited 0 and each delivered one report. No exact planted secret appeared in delivered reports and no canary path or write-mode open appeared in the selected traces. Synthetic file and environment values were nonfunctional and never copied from real accounts.

No-target comparisons produced scan_failed, zero scanned hosts and 520-byte reports. Positive synthetic LDAP/DNS cells scanned two hosts and accepted one fabricated VMware-like RootDSE. A neutral LDAP control retained other DNS/port signals and returned a reported candidate, but not LDAP-confirmed status; it is not a zero-candidate control. Userspace getenv/memory access is invisible to syscall tracing, so no-secret report values do not prove no environment reads.

The fuller Nov 19 matched file-presence pair exited in about 0.125 s per arm, scanned two synthetic hosts and confirmed one fake vCenter. No canary-path access, exact canary report values, extra exec or write-mode opens were observed. Selected-root cryptographic differences showed only collector-created output. This excludes neither omitted roots nor memory-only effects.

### 8.3 Genuine virtualization-tool presence comparison

An initial benign package-extraction attempt failed at the inherited 64 MiB overlay file limit. No specimen ran in that attempt. This is a preparation failure, **not anti-analysis behavior**.

The completed comparison used a separately benign-prepared immutable image with genuine distro QEMU/qemu-img 8.2.2 and virsh 10.0.0 executables. Version probes passed. They were not mocked or simply renamed tools; they also were not a fully configured management stack: no libvirt daemon, virt-install frontend, real VM inventory, management sockets, nested access or genuine credentials were supplied. A larger allowance was used only for sample-free image preparation; specimen overlay caps remained 64 MiB.

| Arm | Natural exit / runtime | Report result | Process/file observations |
|---|---|---|---|
| Tool-absent control | 0 / 0.125424 s | completed, two hosts, one synthetic LDAP-confirmed candidate | one expected exec, no extra exec or write-mode opens |
| Genuine-tools present | 0 / 0.125321 s | same top-level reconnaissance outcome | no observed QEMU/virsh/libvirt/KVM-path inspection or management socket access |

The tool-arm and control reports were **not byte-identical**: timestamp and four DNS canonical-name leaves differed. Do not attribute that variation to tools or claim causal equivalence. Preparation/system-state differences remain confounders. The narrow conclusion is no observed inspection/use of these public tools in these particular runs, not absence of every anti-VM technique or every VM-sensitive branch.

## 9. Archived VirusTotal context

At the retained Nov 17 acquisition, loader engine counts were 0 malicious, 0 suspicious, 64 undetected, 11 unsupported; scout 0/0/63/11. Loader community votes separately included one malicious vote and reputation -1. Community votes are not engine detections.

Vendor first-submission times were 2026-11-05 04:18:32 UTC (loader) and 2026-11-17 08:37:55 UTC (scout). These are vendor submission metadata, not sample creation, compromise date, attribution, another-victim evidence or submitter identity. The archived scout sandbox launched without required configuration, printed usage and exited nonzero. Its CLEAN/harmless label is weak coverage of the configured discovery/report path. Loader behavior data was absent, not affirmative clean evidence. No new VirusTotal requests were made for this draft.

## 10. Defensive interpretation

Exact hashes are the strongest portable identities in this package. A combination of package/scanner identifiers, anonymous RootDSE discovery, report schema and callback behavior may help analysts distinguish this exact build, but no production YARA or network rule is claimed validated against benign corpora or variants. Ports 8999/9090 and generic Go/LDAP strings are not independently reliable malicious indicators.

Investigators should separate application job timing from direct process/flow evidence, retain historical start rather than exit windows, and distinguish full collector receipt from HTTP acceptance. Suspected authorization exposure warrants separately verified recovery; this report neither performs remediation nor declares an account or host clean. No new vulnerability fix, current firewall state, or full persistence exclusion is established.

## 11. Outstanding questions and release gates

1. Completed loader failure evidence is incorporated with bounded timing/filesystem claims; successful full-chain replay remains untested and separately gated.
2. Historical scout execution, report receipt and exact authorization acquisition remain unproved.
3. Realistic DNS/NXDOMAIN, LDAPS, repeat-marker and broader branch coverage remain incomplete; no implicit follow-on execution is authorized.
4. Public source-code/example telemetry excerpts need a second privacy review before inclusion. Current portable artifacts are derived summaries, not independently replayable raw evidence.
5. Independent technical/privacy review passed, and owner approved publication as Blooshoo/plex-loader-scout-analysis, credited Blooshoo under CC BY 4.0. Full-chain replay remains untested; future experimental work requires separate authorization.

See PROVENANCE.md for source hashes and REVIEW.md for redaction gates. The original restricted case remains authoritative and untouched.
