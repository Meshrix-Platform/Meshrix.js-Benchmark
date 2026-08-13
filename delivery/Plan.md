# PLAN-001 Meshrix.js Traceability and Recovery Cost Benchmark

Phase: ready · Revision: unsealed

This document is a render-only projection of `Plan.json`. Edit `Plan.json`; never edit this file.

## Intent

**Goal**: Deliver an independent black-box benchmark that quantifies the latency, throughput, CPU, memory, storage amplification, fault recovery, and rollback cost of Meshrix.js traceability and recovery behavior.

**In scope**
- Standalone benchmark controller, open-loop driver, synthetic side-effect fixture, external observers, fault scenarios, reducers, report schemas, privacy-safe evidence, and executable capacity/fault/soak profiles.

**Out of scope**
- Changes to Meshrix.js production source, imports from Meshrix.js internals, integration with Meshrix.js workflows or release gates, production endpoints, user runtime data, and automatic product-readiness promotion.

**Success**
- Repeated isolated runs produce workload-complete and privacy-safe capacity, fault, and soak reports that quantify incremental cost against matched baselines and fail on evidence loss, duplicate effects, indeterminate recovery, or incomplete observation.

**Risk boundary**
- The benchmark may create and delete only its own isolated synthetic runtime data and may control only benchmark-started Meshrix.js processes; it never targets production or user-configured services and never changes Meshrix.js.

## Decisions

Dossier status: resolved

### Q-001 Which repository boundary should own the benchmark?

Context: The benchmark can either remain independent or be integrated into Meshrix.js; the user explicitly required separation.

Resolution: standalone (user selection)

- [x] `standalone` Standalone repository
  - All benchmark implementation, fixtures, profiles, schemas, reports, and plans belong to Meshrix.js-Benchmark rather than Meshrix.js.
  - Meshrix.js remains a black-box system under test and receives no benchmark imports, workflow entries, release gates, or source changes.
- [ ] `integrated` Meshrix.js integration
  - Benchmark implementation and commands become part of Meshrix.js.
  - Benchmark delivery participates in Meshrix.js repository tooling.

### Q-002 Which visibility should the benchmark repository use?

Context: The benchmark contains private product contracts and operational evidence rules; the user explicitly required a private repository.

Resolution: private (user selection)

- [x] `private` Private repository
  - The remote repository is private from creation.
  - Benchmark design, implementation details, reports, and history are not published through the public Meshrix repository.
- [ ] `public` Public repository
  - The benchmark source and history are publicly visible.
  - Private product details must be excluded from the repository.

### Observed repository facts

- The benchmark repository is a standalone private GitHub repository with its own Git history and no files shared with the Meshrix.js repository. (source: repository initialization)
- Meshrix.js protected operations use finite proof profiles including receipt, on-change receipt, and a full two-stage intent/outcome lifecycle, while operation audit remains a distinct retained evidence stream. (source: Meshrix.js operation proof contracts)
- The governed-evidence contract defines receipt payload and on-disk amplification targets but explicitly requires representative measured amplification rather than estimates from serialized payload size. (source: Meshrix.js governed evidence contract)
- Existing Meshrix.js stress and high-risk resource checks are bounded product verification tools and do not provide an independent matched-baseline capacity, fault, and soak characterization of the complete proof lifecycle. (source: Meshrix.js performance tooling inspection)
- Capacity evidence requires an external open-loop driver, fresh service processes, fixed-range histograms, schedule-delay observation, bounded collection, and workload-integrity reduction before latency interpretation. (source: Meshrix.js performance evidence contract)
- A durable benchmark report must contain aggregate finite-vocabulary metrics only and must exclude paths, addresses, process identifiers, commands, environment values, payloads, responses, credentials, runtime rows, and machine identity. (source: Meshrix.js privacy evidence contract)

## Requirements

| Code | Statement | Sources |
| --- | --- | --- |
| REQ-001 | Implement the benchmark entirely in Meshrix.js-Benchmark and interact with Meshrix.js only through documented process, HTTP, CLI, and on-disk observation boundaries; never import Meshrix.js source, internals, test helpers, or product verifier libraries and never write Meshrix.js. | user-request, resolved-dossier |
| REQ-002 | Create synthetic data only below a benchmark-owned per-run root, bind listeners only to loopback, start fresh Meshrix.js and fixture processes for every measured cell, control only processes started by the controller, and delete ephemeral runtime rows and values after reduction. | user-request, risk-boundary |
| REQ-003 | Run same-payload and same-effect direct-fixture diagnostic baselines beside legal full-stack Receipt plus Audit, Full Intent plus Outcome plus Audit, On-change, state/checkpoint, queue, backup, and restore profiles, while recording exactly which comparisons are matched, diagnostic, component-attributed, or unsupported. | user-request |
| REQ-004 | Permit usability conclusions only from legal production profiles; never present a direct fixture, audit-disabled diagnostic, unlike operation pair, unavailable external seam, or product-internal test as production evidence or as a causal cost isolation. | user-request |
| REQ-005 | Schedule capacity work from intended monotonic start times independently of completions, record schedule delay and scheduled-to-terminal latency, cap in-flight work and timeouts, and retain no per-request latency array or unbounded promise queue. | user-request, repository-contract |
| REQ-006 | Report relative throughput, P50/P95/P99/max latency, CPU per completed operation, peak and settled memory pressure, workload completion, errors, timeouts, schedule delay, and driver overhead for every valid measured cell. | user-request |
| REQ-007 | Separately measure process active write bytes, SQLite WAL peak and growth, and post-quiescence logical and allocated bytes per operation; classify the environment and require qualified Linux process/device evidence before making a physical-write claim. | user-request, repository-contract |
| REQ-008 | Quantify inclusive and component-attributed costs of Operation Audit, terminal Receipt, stable and changed On-change behavior where an external change seam exists, and Full Intent plus Outcome without fabricating an audit-off or proof-off production mode. | user-request, repository-contract |
| REQ-009 | Measure synthetic state mutation, checkpoint frequency, checkpoint restore/revert duration, restored-byte volume, service unavailability, and exact preimage recovery using only governed external workspace operations. | user-request |
| REQ-010 | Measure durable job admission, running-lease interruption, replacement-worker takeover, checkpoint continuity, terminal convergence, duplicate execution, and recovery time through formal server and background-worker process surfaces. | user-request, repository-contract |
| REQ-011 | Measure coherent backup publication, backup interference, preview verification, confirmed offline restore, rollback cost, restart reconciliation, RTO, and RPO against a bounded synthetic dataset through formal external operator surfaces. | user-request, repository-contract |
| REQ-012 | Synchronize Full-proof faults before fixture effect, after unique effect but before response, and after acknowledged Outcome; synchronize queue takeover from the public running projection and use honest in-progress observation plus whole-generation oracles where backup or restore exposes no external commit phase. | user-request |
| REQ-013 | Fail before performance interpretation on missing evidence, duplicate effect, blind retry, indeterminate terminal state, RPO violation, mixed restore generation, incomplete phase observation, workload loss, or histogram overflow. | user-request |
| REQ-014 | Provide exact quick, characterization, fault, and soak scales, make characterization/fault/soak opt-in, and enforce fixed duration, operation, memory, disk, in-flight, artifact, and recovery ceilings. | user-request |
| REQ-015 | Apply declared relative ratios only to their stated comparison class, enforce absolute safety ceilings, require repeated-run stability for characterization, and never turn a benchmark result into automatic readiness promotion. | user-request |
| REQ-016 | Persist only schema-validated aggregate reports with finite keys and enum strings, atomically replace report files, and exclude paths, hosts, ports, process identifiers, commands, environment values, payloads, responses, credentials, identities, runtime rows, and machine identity. | user-request, repository-contract |
| REQ-017 | Supply unit, contract, integration, privacy, quick black-box, and fault-smoke checks owned by this repository plus one complete regression command; do not call or integrate Meshrix.js smoke, stress, verifier, workflow, report, or release-gate code. | user-request |

## Architecture

One complex heavy Task owns the coupled standalone benchmark product. A black-box contract Node freezes the legal Meshrix.js process and protocol surfaces, then controller, driver, fixture, observer, evidence-schema, and profile branches proceed independently; reducers and fault control join only the branches they consume, and capacity/soak plus fault/recovery integrations join once before one executable closure.

- A single Task is necessary because the controller lifecycle, SUT adapter, open-loop driver, fixture oracle, observers, reducers, schemas, and profile semantics form one executable contract and share the root package graph, CLI, report publication, and end-to-end acceptance. Splitting them would create cross-Task API and integration prerequisites.
- Meshrix.js is a read-only system under test. The benchmark starts its documented server entry point with an isolated data directory, dynamic loopback port, and private ready file; it may start the documented external import-worker entry point for queue trials. It discovers and exercises the external interface catalog, health operation, authentication, upstream publication/transit, Operation Permission audit, console-state On-change, workspace checkpoint/revert, job/work-queue, and storage backup/restore operations over external protocols.
- The SUT locator accepts one repository path, proves its package identity, selects only a known source or packaged server/worker entry point, and rejects symlinks, arbitrary command templates, remote URLs, non-loopback binding, and data or backup roots outside the benchmark run root. Source and packaged layouts are direct bounded branches, not a plugin system.
- Every measured cell creates a new private run root and fresh fixture/SUT processes. Setup and authorization are untimed; warmup precedes measurement; graceful shutdown and a 30-second quiescence bound precede final allocation measurement. Cleanup runs in `finally`, removes only the validated run root, and never searches for or signals a process by name.
- The controller keeps explicit child handles and, on POSIX, an exclusive process group created by that child. It sends graceful termination before a bounded kill only to those handles/groups, revalidates ownership before signaling, and never uses `killall`, PID discovery, or a user-supplied PID. Unsupported process isolation makes the profile unavailable rather than broadening control.
- The neutral fixture has separate loopback data and control surfaces. Data operations return fixed 256-byte responses for fixed 1,024-byte requests; the write operation commits only an idempotency key and finite status to an ephemeral SQLite unique table. The control surface can hold `arrived`, `effect_committed`, and `response_released` barriers and exposes counts, never request or response content.
- Fixture rows exist only during a run and are deleted with the run root. A unique constraint provides the duplicate-effect oracle without retaining an in-memory key set during soak. The fixture has a profile-derived maximum row count and rejects admission beyond it.
- Diagnostic direct-read/direct-write cells call the same fixture route, payload size, response size, delay, and effect rule as their corresponding Meshrix.js transit cells. They quantify driver, network, and fixture cost but cannot support a production usability conclusion.
- Legal production modes are `receipt_audit_read` through a read-only projected upstream operation, `full_audit_write` through a safe-write projected upstream operation, `on_change_stable` through the current console-state contract, matched `state_mutation_only` and `state_checkpoint` workspace cells, `queue_no_fault` plus paired `queue_lease` recovery trials through job admission and the formal import worker, paired `backup_interference`, and paired `restore_recovery` through storage operator contracts.
- Current upstream projection fixes metadata-only Audit and derives Receipt or Full proof from legal risk. There is no legal external Audit-off toggle. Audit latency/CPU therefore remains `unsupported_no_legal_toggle`; the report may attribute audit database bytes and report inclusive full-stack overhead, but may not label the difference causal Audit cost.
- Current On-change exposes a stable console-state read but no matched externally controllable change on that same operation. Stable no-write behavior is a production measurement. A changed On-change cell runs only when capability discovery finds a formal change seam for that projection; otherwise the changed-cost field is `unsupported_no_change_seam`, not an inferred number.
- A report marks each comparison `matched_full_stack`, `diagnostic_transport`, `component_attribution`, `unmatched_profile`, or `unsupported`. Direct-to-production ratios remain diagnostic; absolute production measurements and repeated legal production-to-production profiles alone can support usability conclusions.
- The open-loop scheduler uses `process.hrtime.bigint()` and a scalar sequence number: intended start is `epoch + sequence * interval`, so scheduling is O(1) and never rebased on completion. A bounded active set admits at most the profile limit; a missed admission is counted as a drop and invalidates the cell rather than entering a hidden queue.
- Each request records service latency from actual dispatch, end-to-end latency from intended start, and schedule delay from intended to actual dispatch into separate fixed-range HdrHistograms. Histograms cover 1 microsecond through 120 seconds at three significant digits; out-of-range values increment overflow and invalidate the cell. Only count, mean, P50/P95/P99/max, and overflow are persisted.
- HdrHistogram is selected because its official implementation provides fixed recording cost and memory independent of sample count (https://hdrhistogram.github.io/HdrHistogram/). `hdr-histogram-js` is the only measurement dependency. A hand-built bucket scheme is rejected because tail error and overflow semantics would become benchmark code to validate.
- wrk2's constant-throughput model and intended-send latency accounting (https://github.com/giltene/wrk2) are adopted as scheduling invariants. Autocannon (https://github.com/mcollina/autocannon) is rejected as the driver because this benchmark needs phase barriers, separate schedule-delay histograms, exact workload oracles, and controller-owned bounded state; its own documentation also identifies driver CPU saturation. Node's documented monotonic high-resolution clock is the time source (https://nodejs.org/api/process.html#processhrtimebigint).
- Toxiproxy (https://github.com/Shopify/toxiproxy) is rejected for the core fault path: a generic TCP fault cannot prove whether the synthetic effect committed. The fixture's application barrier and owned-process termination give the required deterministic phase with no external daemon; network impairment is outside this goal.
- Observers sample once per second into fixed accumulators. Capacity cells retain aggregate extrema and sums only. Soak folds 1-second values into 60-second windows and retains at most 360 windows, then persists only first/last steady-hour summaries and trend slopes.
- On Linux, the observer sums `utime+stime`, RSS, page faults, and `write_bytes` for the owned SUT process tree; WAL files are observed by suffix and all paths are reduced immediately to finite categories. On other POSIX systems CPU/RSS and `st_blocks*512` may be reported when available; unsupported fields are null with an observation-status enum.
- Storage metrics are distinct: `activeWriteBytesPerOp` is the delta of Linux procfs per-process `io.write_bytes`; `walPeakBytes` and `walGrowthBytes` come from active sidecar size; `logicalBytesPerOp` is post-quiescence `st_size`; and `allocatedBytesPerOp` is post-quiescence `st_blocks*512`. Reflink backups therefore retain separate logical and allocated facts.
- Environment classes are `linux_process_io`, `linux_device_qualified`, `posix_filesystem_only`, and `correctness_only`. A physical device-write result requires Linux, a benchmark-owned data root mapped to one readable block device, explicit exclusive-device mode, monotonically readable device-sector counters, all workload processes owned by the controller, and idle before/after deltas no greater than 1% of workload delta; otherwise device bytes are absent and no physical-amplification claim is allowed.
- Capacity reduction first proves scheduled/admitted/completed accounting, terminal response classes, fixture effect cardinality, evidence cardinality, observer coverage, and zero histogram overflow. Only then does it calculate throughput, latency, CPU/op, memory, storage/op, WAL, and comparison ratios.
- `quick` runs one repetition of eight cells: direct read, Receipt plus Audit read, stable On-change, direct write, Full plus Audit write, state mutation without checkpoint, the matched state mutation with checkpoint, and no-fault queue processing. Each cell uses 3 seconds warmup, 10 seconds measurement, 20 offered operations/second, exactly 200 scheduled operations, 64 maximum in-flight, 10-second request timeout, and 30-second drain; the checkpoint cell checkpoints every 20 successful 4-KiB mutations.
- `characterization` is opt-in. It runs those eight cells in a deterministic rotated order over offered rates `[10, 50, 100, 250]` operations/second, with three fresh-process repetitions per rate and mode, 15 seconds warmup, 60 seconds measurement, scheduled counts `[600, 3000, 6000, 15000]`, 1,024 maximum in-flight, 30-second request timeout, and 60-second drain. Both state cells use 4-KiB mutations and identical sequence seeds, with one checkpoint per 100 successes only in the checkpoint cell; jobs carry 4 KiB.
- Characterization creates an untimed 256-MiB backup dataset of exactly 4,096 64-KiB synthetic objects. Backup interference uses three paired fresh runs: a no-backup cell and a backup cell, each with 30 seconds warmup plus 150 seconds measurement at `min(100, floor(0.7 * safeRate))` operations/second; backup begins at measured second 60. Restore characterization uses three matched fresh-root pairs built from the same dataset and deterministic changes to exactly 512 objects: the control performs the same offline stop and restart without rollback, while the restore leg previews and applies the selected backup before restart. Reports expose restore-minus-control wall time, CPU, active writes, allocated bytes, and peak RSS as rollback cost, but publish a valid restore result only when the restored generation is exact.
- A capacity cell is workload-complete when drop count and histogram overflow are zero, at least 99.9% of scheduled work reaches the declared expected terminal class, unexpected errors plus timeouts are at most 0.1%, observer coverage is at least 99%, and schedule-delay P99 is no more than `max(20 ms, 2 * offered interval)`. The safe rate is the highest workload-complete rate; any evidence loss or duplicate effect invalidates the whole run, not only the cell.
- Characterization is stable only when the coefficient of variation of safe throughput is at most 10% and repetition P99 max/min is at most 1.25. Receipt and Full transit cells must retain at least 50% of their direct diagnostic safe throughput and have P99 no greater than `4 * direct P99 + 25 ms`; this is a diagnostic guard, not a production-baseline claim. The checkpoint cell must retain at least 80% of the matched mutation-only safe throughput, have P99 no greater than `2 * mutation-only P99 + 25 ms`, and peak RSS no greater than 120% of the mutation-only cell. These are benchmark interpretation defaults, not readiness SLOs.
- Backup interference passes its characterization threshold when throughput is at least 80% of the paired no-backup run, P99 is at most `2 * paired P99 + 25 ms`, peak RSS is at most 120% of the pair, publication completes within 600 seconds, and both pre-existing evidence and the published backup remain valid. Restore preview must finish within 120 seconds and 256-MiB apply within 600 seconds with exact selected-generation recovery; the paired control is reported rather than subtracted from these safety ceilings.
- `fault` is opt-in and runs ten matched fresh-root trial pairs for each of seven scenarios: `full_before_effect`, `full_after_effect_before_outcome`, `full_after_outcome_ack`, `queue_lease_takeover`, `checkpoint_restore_in_progress`, `backup_publish_in_progress`, and `restore_apply_in_progress`. The control leg reaches the identical public or fixture-barrier phase and completes without process termination; the fault leg terminates only its owned process at that phase. Each leg allows 60 seconds startup, 30 seconds phase acquisition, 120 seconds recovery, and 60 seconds cleanup; backup/restore datasets are exactly 64 MiB and queue trials admit 128 4-KiB jobs. Reducers report fault-minus-control RTO, CPU, active-write, allocation, and RSS deltas only after both legs pass correctness.
- Full-proof synchronization is exact: hold at fixture arrival and kill the SUT for before-effect; commit the unique effect, hold the response, and kill for after-effect/before-Outcome; or wait for the successful client terminal and kill for after-Outcome. The effect count at the fault boundary must be zero, one, and one respectively. Restart uses the same isolated data root and idempotency key; after recovery the first trial may end with zero effects only when public state declares a safe no-effect terminal, otherwise an explicitly permitted retry may bring it to one, while the latter two trials require exactly one. A second effect or a retry without explicit public permission is always failure.
- Queue takeover waits until the public work-queue projection reports a benchmark job running, kills only the controller-started import worker, waits for the old lease to expire, starts a replacement worker, and requires one terminal job, one effect, the last public checkpoint where available, and no stale-worker settlement.
- Checkpoint restore, backup publication, and offline restore do not expose internal commit failpoints. The controller kills only while the public request is pending and the external storage observer has seen more than 1 MiB of new checkpoint, backup, staging, or rollback allocation. Reports mark phase precision `external_in_progress`; the oracle accepts only an entirely old or entirely committed generation and rejects partial publication, mixed generations, missing evidence, or indeterminate recovery.
- Backup restart must list either no new backup or one completely preview-verifiable backup; an unpublished partial tree is never accepted. Restore restart must converge within 120 seconds to exactly the pre-restore or selected backup generation according to the public terminal receipt, with RPO zero relative to the chosen generation.
- `soak` is opt-in and requires one selected legal production mode, defaulting to `full_audit_write`, plus a compatible characterization report. It uses 5 minutes warmup and exactly 6 hours measurement at `max(1, min(100, floor(0.7 * safeRate)))` operations/second, no more than 2,160,000 scheduled operations, 1,024 maximum in-flight, a 30-second timeout, and 360 one-minute in-memory windows. The default mode starts backups at measured hours two and four.
- Soak is valid only with the same correctness gates, zero dropped work, at least 99.9% expected terminals, at most 0.1% unexpected errors/timeouts, and 99% observer coverage. Last steady-hour RSS P95 must be no more than 115% of first steady-hour RSS P95 plus 64 MiB, last-hour allocated bytes/op no more than 120% of first-hour, and each scheduled backup must meet the paired interference thresholds.
- Global safety ceilings are 512 MiB for controller plus fixture RSS, 2 GiB for the owned SUT process tree, 8 GiB allocated across the run and backup roots, 1,024 in-flight requests, 120 seconds histogram range, 1 MiB per persisted report, and the declared profile duration plus 60 seconds drain. A ceiling breach terminates only owned processes and publishes a finite failed summary.
- Reports are `capacity`, `fault`, or `soak`, validate with `additionalProperties: false`, and contain only schema/profile/mode/status/comparison/environment enums, booleans, nulls, bounded numeric aggregates, and semver-shaped benchmark/SUT versions. No timestamps, UUIDs, hashes, raw histogram buckets, samples, request records, errors, logs, or arbitrary strings are persisted.
- Report publication writes a private temporary sibling, synchronizes it, atomically renames it to the kind-specific latest report destination, and synchronizes the parent. A recursive schema allow-list and adversarial privacy scan run before publication. Failure diagnostics map to finite reason codes; captured child output is a bounded ring used only in memory and is discarded.
- Correctness and recoverability are hard gates. Performance status is `interpretable`, `insufficient_workload`, `unstable`, `safety_ceiling`, or `not_comparable`; the benchmark never writes a Meshrix.js report, gate, workflow receipt, readiness claim, or release decision.
- The current Meshrix.js surface has a formal online backup create/preview route but confirmed restore rejects any active storage runtime, while the public CLI forwards to that online runtime and exposes no one-shot offline restore command. The adapter must capability-probe a future formal offline operator entry point and otherwise fail restore and full fault profiles with `unsupported_external_restore`; importing the storage provider or invoking existing product verifier scripts is forbidden. This is the one remaining external-surface blocker to a successful current-version restore measurement.

## Tasks

Every Task belongs to the same mutually independent parallel frontier.

### TASK-001 deliver-independent-black-box-benchmark

Tier: complex · Workload: heavy · Verification: code · Frontier: parallel

Outcome: Meshrix.js-Benchmark provides a bounded executable controller that independently measures legal Meshrix.js capacity, traceability cost, storage amplification, fault recovery, and soak behavior against neutral synthetic fixtures, publishes only privacy-safe aggregate evidence, and fails honestly when correctness, observation, or an external recovery seam is incomplete.

Risks: performance, concurrency, persistent_state, observability, privacy, security, operations, shared_resource, protocol, public_interface, quality
Requirements: REQ-001, REQ-002, REQ-003, REQ-004, REQ-005, REQ-006, REQ-007, REQ-008, REQ-009, REQ-010, REQ-011, REQ-012, REQ-013, REQ-014, REQ-015, REQ-016, REQ-017
Writes: package.json, package-lock.json, tsconfig.json, src, profiles, schemas, tests, reports, .gitignore, README.md
Exclusive resources: benchmark-owned `.work` runtime and backup roots, benchmark loopback fixture and SUT port allocation, benchmark-started Meshrix.js server and background-worker process handles, `reports/capacity/latest.json`, `reports/fault/latest.json`, and `reports/soak/latest.json` atomic publication targets, benchmark root dependency graph and lockfile

In scope
- Benchmark-owned package metadata, TypeScript source, fixture, process controller, HTTP client, open-loop scheduler, histograms, observers, fault orchestration, reducers, profiles, JSON Schemas, reports directory contract, documentation, and tests.
- Read-only discovery and execution of the sibling Meshrix.js documented server, background-worker, HTTP, CLI, and filesystem observation surfaces against benchmark-owned isolated data.
- Exact quick, characterization, fault, and soak profiles, privacy-safe reports, and focused/full verification.

Out of scope
- Any Meshrix.js source, package, test, helper, workflow, report, release gate, verifier, smoke, stress, or acceptance-reducer change or reuse.
- Production, public, remote, or user-configured services; user runtime data; arbitrary commands/endpoints; credentials or values supplied by a real operator; automatic readiness promotion; and retained request, response, audit-row, proof-row, fixture-row, log, or private-value evidence.
- Product failpoints, internal module imports, internal SQLite row interpretation, causal Audit-off claims without a legal toggle, physical-write claims without qualified Linux evidence, and fabricated restore results without a formal offline operator surface.

Outputs
- OUT-001 Standalone benchmark CLI and owned-process lifecycle (`src/cli.ts`): Every run is loopback-only, fresh-process, bounded, cancellable, and confined to a validated benchmark-owned root without writing the SUT repository.
- OUT-002 Intended-time scheduler and fixed-memory metrics (`src/driver/open-loop.ts`): Load is completion-independent, coordinated omission is exposed through scheduled latency and schedule delay, and memory is independent of request count apart from the bounded active set.
- OUT-003 Synthetic upstream and deterministic phase oracle (`src/fixture/server.ts`): Direct and full-stack requests have matched bytes/effects, barriers identify external effect phases, unique effects are counted exactly, and ephemeral rows are removed after reduction.
- OUT-004 Process, memory, CPU, filesystem, WAL, and device observers (`src/observers/index.ts`): Metrics distinguish active writes, WAL, logical, allocated, and qualified device bytes and emit unavailable facts instead of unsupported claims.
- OUT-005 Process-safe black-box fault and recovery controller (`src/faults/orchestrator.ts`): Full proof, queue lease, checkpoint, backup, and restore trials use only external synchronization and fail on duplicates, loss, blind retry, indeterminate state, RPO, or observation gaps.
- OUT-006 Exact bounded workload and threshold definitions (`profiles/characterization.json`): Quick, characterization, fault, and soak scales and safety ceilings are immutable inputs, with expensive profiles opt-in and no hidden defaults.
- OUT-007 Closed aggregate evidence schemas and atomic reducer (`schemas/capacity-report.schema.json`): Capacity, fault, and soak outputs contain only finite aggregate vocabulary, pass privacy scanning, remain at most 1 MiB, and never promote product readiness.
- OUT-008 Independent unit, integration, black-box, and privacy evidence (`tests/acceptance/black-box.test.ts`): Focused tests prove scheduling, bounds, process ownership, phase oracles, reducers, schemas, cleanup, and current SUT behavior without consuming Meshrix.js test code.
- OUT-009 Reproducible usage and interpretation boundary (`README.md`): Operators can run each profile, understand comparison validity and environment classes, and see that unsupported restore or physical observation fails rather than being inferred.

Internal Node graph
- NODE-001 freeze-black-box-contracts · after: none · Encode the documented SUT identity, source/packaged start surfaces, loopback readiness, external operation capability probe, process-ownership guard, and current offline-restore gap in benchmark-owned contracts and contract tests.
- NODE-002 build-controller-and-sut-adapter · after: NODE-001 · Implement run-root custody, fresh process lifecycle, loopback HTTP/auth/publication setup, owned worker control, timeouts, cleanup, and source/packaged SUT adapters with no arbitrary command or internal import.
- NODE-003 build-open-loop-driver · after: NODE-001 · Implement the O(1) intended-time scheduler, bounded active set, HdrHistogram recording, workload accounting, driver overhead, fake-clock tests, and overflow behavior.
- NODE-004 build-neutral-fixture · after: NODE-001 · Implement the fixed-byte read/write fixture, ephemeral unique-effect SQLite oracle, private barrier control surface, profile row ceilings, and fixture contract tests.
- NODE-005 build-external-observers · after: NODE-001 · Implement process-tree CPU/RSS/write observation, one-second aggregation, WAL/logical/allocated accounting, Linux device qualification, environment classification, soak windows, and unavailable-field tests.
- NODE-006 build-evidence-contracts · after: NODE-001 · Implement closed capacity/fault/soak schemas, finite reason vocabulary, aggregate-only reducer types, privacy adversaries, report-size budget, and synchronized atomic publication.
- NODE-007 build-profile-catalog · after: NODE-001 · Implement and validate the exact quick, characterization, fault, and soak workload matrices, dataset shapes, thresholds, opt-in gates, and global safety ceilings.
- NODE-008 build-fault-orchestrator · after: NODE-002, NODE-004, NODE-005 · Implement exact fixture barriers, matched no-fault/fault legs, owned-process termination/restart, public queue-running takeover, external in-progress checkpoint/backup/restore observation, RTO/RPO and terminal-state oracles, and capability-unavailable failure.
- NODE-009 build-aggregate-reducers · after: NODE-003, NODE-005, NODE-006, NODE-007 · Implement workload-first capacity, comparison-validity, storage attribution, repeat-stability, state/checkpoint, backup-interference, restore/control, paired recovery correctness, and soak-trend reduction without retaining raw rows or samples.
- NODE-010 integrate-capacity-and-soak · after: NODE-002, NODE-003, NODE-004, NODE-005, NODE-006, NODE-007, NODE-009 · Connect the controller, matched fixture routes, legal production modes, driver, observers, reducers, quick/characterization/soak execution, backup interference, state/checkpoint and queue workloads, cleanup, and report publication.
- NODE-011 integrate-fault-and-recovery · after: NODE-008, NODE-009, NODE-006, NODE-007 · Connect all seven matched fault/control scenarios, evidence/effect/terminal oracles, queue worker takeover, checkpoint and backup generations, formal offline restore capability, ten-pair reduction, fault-smoke scale, cleanup, and fault report publication.
- NODE-012 close-executable-benchmark · after: NODE-010, NODE-011 · Join capacity/soak and fault/recovery paths; add the root CLI/scripts/dependency lock, self-contained fake-SUT integration suite, real sibling quick and fault-smoke acceptance, report verification, privacy scan, and operator documentation, then run every focused command.

Design
- comparison-defaults
  - The report always identifies the numerator/denominator mode and comparison class. Relative performance thresholds run only for same-fixture direct/transit diagnostics, the state-mutation/checkpoint pair, no-fault/fault trial pairs, backup/no-backup windows, and restore/clean-restart pairs. Stable On-change and any queue metric without its declared pair expose absolute values and explicit context, not causal deltas.
  - The audit component exposes audit-category logical/allocated delta and retained-count delta from public summaries where available. It never reads or serializes audit rows. Without an external legal toggle, latency and CPU delta remain unavailable.
- component-ownership
  - `src/controller` owns the finite run lifecycle and cleanup; `src/sut` owns only formal process/protocol adaptation; `src/driver` owns rate and histograms; `src/fixture` owns synthetic effects; `src/observers` owns OS/filesystem facts; `src/faults` owns external fault actions; `src/reducers` owns interpretation; `src/reporting` owns schema/privacy/publication. Dependencies point inward through immutable value records and never expose live paths or secrets to reducers.
  - Setup produces ephemeral handles containing roots, ports, sessions, and tokens. Those handles are accepted only by controller-owned execution code, are never passed to reports, and are zeroed/discarded at cleanup. Reducers accept numeric/enumerated observations only.
- data-flow
  - Validate profile and owned roots; start fixture; start fresh SUT; authenticate and publish the neutral operations; capability-probe required routes; perform untimed dataset setup; warm; reset histograms/counters/observers; execute intended-time load or one leg of a paired fault trial; drain; query public aggregate/evidence projections; stop and quiesce; reduce; privacy/schema validate; atomically publish; delete the run root.
  - Direct and production cells share immutable request templates and fixture delay/effect parameters. Operation keys are generated in a bounded streaming sequence, retained only by the fixture's ephemeral unique table, and represented in reports only as attempted/effected/duplicate counts.
- fault-state-machine
  - The controller uses explicit states `created`, `ready`, `phase_observed`, `faulted`, `restarting`, `recovered`, `reduced`, and `cleaned`. A pure transition table rejects skipped or repeated destructive transitions; cleanup is idempotent and reachable from every state. No process signal occurs unless the current state contains an owned live handle.
  - A fault leg persists no lifecycle transcript. It returns a finite result record containing scenario enum, control/fault enum, phase precision enum, counts, RTO, RPO, and reason enum; the reducer joins each control/fault pair, then aggregates ten pairs.
- measurement-defaults
  - HdrHistogram auto-resize is disabled. One histogram instance exists per measured dimension per active cell; cells reduce before the next cell. Periodic observers never enumerate or retain file names after category reduction.
  - CPU/op divides owned SUT-tree CPU nanoseconds by workload-complete operations; fixture and driver CPU are separate. Memory reports peak and settled RSS. Missing platform support yields null plus `not_observed`, never zero.
  - Active process writes, WAL, logical size, allocation, and device sectors are not interchangeable. Only `linux_device_qualified` may populate physical device bytes/op; no threshold silently substitutes another metric.
- patterns
  - pattern_catalog: refactoring-guru-catalog-22-v1; candidate: State; decision: reject; pressure: controller and fault lifecycles have finite destructive transitions; expected_benefit: none beyond a pure transition table whose states and owned handles are directly testable; simpler_alternative: a discriminated union plus total transition function is sufficient; application: keep transitions in one controller module and test every allowed/denied edge; costs_and_rejections: state classes would fragment process ownership and cleanup without adding replaceable behavior.
  - pattern_catalog: refactoring-guru-catalog-22-v1; candidate: Strategy, Adapter, Command; decision: reject; pressure: workload modes, source/packaged entry layouts, observers, and fault actions vary; expected_benefit: none beyond immutable mode records and small functions selected at the composition root; simpler_alternative: closed discriminated maps keep the finite supported surface visible and schema-aligned; application: no generic plugin, command bus, undo log, or class hierarchy is created; costs_and_rejections: indirect dispatch would make unsupported external seams, destructive authority, and report vocabulary harder to audit.
- privacy-defaults
  - Child stdout/stderr is capped at 64 KiB per process in memory. Known conditions map to enums; unknown text becomes `unclassified_failure` and is not persisted or returned by report commands. Console progress uses counts and phase enums only.
  - Reports have no run ID or timestamp. Repetition evidence is nested fixed arrays of aggregate cells whose maximum lengths come from the profile schema. `latest.json` replacement is the only default durable output; optional history is outside automated execution.
- profile-defaults
  - Quick is the only default executable profile. Characterization, fault, and soak require their explicit CLI names; soak additionally requires a characterization report and selected legal mode. No environment variable may silently raise a rate, duration, file count, dataset size, in-flight limit, or safety ceiling.
  - Fixed payload bodies are generated from a repository constant and sequence without private input. Profile edits are schema validated, and the report records profile/version enums plus actual numeric scale so a renamed profile cannot hide changed work.
- recovery-defaults
  - A successful client response is an acknowledged effect. RPO is zero for every acknowledged effect. An unacknowledged Full request may resolve to no effect or one in-doubt effect, but it may never be blindly retried, duplicated, hidden, or left without a public terminal/recovery classification.
  - Backup/restore whole-generation oracles use deterministic synthetic object counts and byte patterns through public reads. They compare finite counts and byte equality in memory during the run, emit only equal/not-equal and counts, and discard content immediately.

Acceptance
- AC-001 covers REQ-001, REQ-004, REQ-017, OUT-001, OUT-009
  - Given A clean Benchmark repository and a sibling Meshrix.js tree containing both source code and existing product tests
  - When the dependency-boundary and contract checks run
  - Then every Benchmark import resolves inside this repository or an approved third-party package, every SUT interaction is process/HTTP/CLI/read-only observation, the sibling worktree is byte-unchanged, and product smoke/verifier/report/workflow code is never invoked
  - Oracle: `npm run test:contract` exits zero with zero forbidden imports, zero forbidden command targets, a recognized external surface catalog, and an unchanged sibling status snapshot
  - Evidence: command from npm run test:contract
- AC-002 covers REQ-005, REQ-006, REQ-014, REQ-015, OUT-002, OUT-006
  - Given Fake time, responses delayed beyond later intended starts, histogram boundaries, in-flight saturation, timeouts, millions of synthetic records, and driver CPU pressure
  - When driver unit tests run
  - Then intended times never depend on completion, scheduled/service/delay histograms are distinct, memory remains bounded, every request is accounted, and overflow/drop invalidates rather than disappears
  - Oracle: `npm run test:unit -- --group driver` exits zero with exact scheduled counts, maximum active work at configuration, constant histogram storage, zero retained latency arrays, and expected invalid reason codes
  - Evidence: command from npm run test:unit -- --group driver
- AC-003 covers REQ-002, REQ-007, REQ-012, REQ-013, OUT-003, OUT-004, OUT-005
  - Given Adversarial roots, symlinks, non-loopback targets, unrelated processes, fixture duplicate keys, every barrier order, cleanup failure, and Linux/POSIX/correctness-only observer fixtures
  - When controller, fixture, observer, and fault integration tests run
  - Then only owned processes/data are controlled, effects are exactly counted, barriers are deterministic, storage metrics remain distinct, unsupported physical evidence stays absent, and cleanup cannot escape the run root
  - Oracle: `npm run test:integration -- --group isolation-fault-observation` exits zero with zero foreign signals/deletes, exact effect counts, expected phase enums, qualified/unqualified device outcomes, and no remaining ephemeral fixture rows
  - Evidence: command from npm run test:integration -- --group isolation-fault-observation
- AC-004 covers REQ-016, REQ-013, OUT-007
  - Given Capacity, fault, and soak aggregate candidates containing oversized data, unknown keys, paths, addresses, process metadata, command text, environment values, payloads, responses, credentials, identities, runtime rows, and arbitrary error strings
  - When schema and privacy verification runs
  - Then every adversarial candidate is rejected, valid bounded aggregates are at most 1 MiB, and interrupted publication leaves the previous valid report intact
  - Oracle: `npm run verify:reports` exits zero with all negative fixtures rejected, all positive schemas closed, atomic-replacement crash cases preserving one valid generation, and the forbidden-value scan at zero
  - Evidence: command from npm run verify:reports
- AC-005 covers REQ-003, REQ-004, REQ-008, REQ-009, REQ-010, REQ-014, REQ-016, OUT-001, OUT-002, OUT-003, OUT-004, OUT-006, OUT-007
  - Given The real sibling SUT, fresh data for every quick cell, matched fixture routes, legal Receipt/Audit, Full/Audit, On-change, paired state mutation/checkpoint, and queue operations, and bounded process/filesystem observers
  - When the quick profile runs twice
  - Then both runs schedule exactly 200 operations in each of eight modes, complete or explicitly invalidate every cell, produce no duplicate effect or missing acknowledged evidence, publish schema-valid aggregate capacity reports, and leave the SUT repository unchanged
  - Oracle: `npm run benchmark:verify:quick -- --sut-root ../Meshrix.js --repeat 2` exits zero only when both reports pass correctness/privacy/schema gates, contain the exact quick scale, classify every comparison, and cleanup all owned roots/processes
  - Evidence: command from npm run benchmark:verify:quick -- --sut-root ../Meshrix.js --repeat 2
- AC-006 covers REQ-009, REQ-010, REQ-011, REQ-012, REQ-013, OUT-005, OUT-008
  - Given A self-contained fake SUT with public evidence, queue, checkpoint, backup, and offline-restore surfaces plus injected crashes at all seven scenario boundaries
  - When the fault-smoke suite runs one control/fault pair per scenario
  - Then exact phase or honest in-progress synchronization is used, every leg converges to one allowed whole generation, paired recovery cost is reduced only after correctness, and every duplicate, blind retry, indeterminate state, RPO breach, or observation gap causes nonzero failure
  - Oracle: `npm run benchmark:verify:fault-smoke` exits zero with seven paired scenarios, fourteen determinate legs, zero duplicate/missing/blind-retry/RPO counters, and bounded RTO; negative fixtures each exit nonzero with the expected finite reason
  - Evidence: command from npm run benchmark:verify:fault-smoke
- AC-007 covers REQ-001, REQ-011, REQ-013, REQ-004, OUT-001, OUT-005, OUT-009
  - Given The real sibling SUT and the current absence or future presence of a formal offline restore entry point
  - When external capability acceptance runs
  - Then backup create/preview and supported fault scenarios execute through public surfaces, while confirmed restore either runs through the discovered formal operator entry or the requested restore profile fails explicitly without importing internals, reusing a verifier, or publishing a passing fault/restore report
  - Oracle: `npm run benchmark:verify:external-recovery -- --sut-root ../Meshrix.js` exits zero after proving exactly one valid capability branch: a complete seam produces exact selected-generation recovery, or an absent seam makes the nested restore-profile invocation exit nonzero with solely `unsupported_external_restore`, zero SUT writes, zero internal imports, and no restore report replacement
  - Evidence: command from npm run benchmark:verify:external-recovery -- --sut-root ../Meshrix.js
- AC-008 covers REQ-017, REQ-014, REQ-015, OUT-008, OUT-009
  - Given All focused checks, a fake SUT capable of long virtual-time profiles, and the real sibling quick surface
  - When the complete Benchmark regression runs once
  - Then unit/contract/integration/privacy checks, exact profile schemas, virtual characterization/fault/soak reductions, real quick black-box execution, cleanup, and documentation all pass without running expensive real profiles or modifying Meshrix.js
  - Oracle: `npm run verify -- --sut-root ../Meshrix.js` exits zero, every focused command is included once, opt-in profiles remain opt-in, all generated acceptance reports validate, and the sibling worktree is unchanged
  - Evidence: command from npm run verify -- --sut-root ../Meshrix.js

Focused regression
- `npm run test:contract`
- `npm run test:unit`
- `npm run test:integration`
- `npm run verify:reports`
- `npm run benchmark:verify:quick -- --sut-root ../Meshrix.js --repeat 2`
- `npm run benchmark:verify:fault-smoke`
- `npm run benchmark:verify:external-recovery -- --sut-root ../Meshrix.js`
- paths: package.json, package-lock.json, tsconfig.json, src, profiles, schemas, tests, reports, .gitignore, README.md

## Full regression

Run inside the sole Reviewer session after every repair is integrated.

- `npm run verify -- --sut-root ../Meshrix.js`
- paths: package.json, package-lock.json, tsconfig.json, src, profiles, schemas, tests, reports, .gitignore, README.md
