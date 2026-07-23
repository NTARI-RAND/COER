# COER — Open Questions

*The living open-questions deliverable required by Janus-Facing Architecture (standard check 7). Reviewed and re-shipped with each release; staleness is nonconformance. An entry leaves only by resolution or by an explicit values call made by the people who live with the outcome. Invariant IDs cite the registry in `JFA/jfa-conformance-suite.py`. This document is the canonical home of the spec's §13 decisions.*

## 1. Sequencing the layer inheritance
**Problem.** COER's economy-education shape bends — public-benefit records and alerting, not a credit marketplace — but the layers below bend for no one: sovereign substrate, the witnessed record, and the covenant bind uniformly. Which surfaces bind first, and when? Community-responder coordination is the natural covenant surface (verified credentials, accountability, accept/decline, stand-down are proto-assessment mechanics), and the immutable AuditEvent is the natural record seam.
**Constraints inherited.** The shape may bend; the floors do not. Every binding not yet built is a named interim with a committed path (SUB-1, REC discipline, COV discipline as the responder layer matures) — never a standing exemption. Silently treating COER as outside the covenant would be the erosion the tension protocol names.
**Status.** Open — sequence the bindings; responder coordination is the first covenant surface to reach build.

## 2. Hosting posture — the multi-tenant SaaS chokepoint
**Problem.** The spec targets a FedRAMP-authorized, centrally operated, multi-tenant cloud. As an end-state that is a hosting chokepoint (SUB-1).
**Constraints inherited.** No unremovable hosting chokepoint; the spec's own §1 portability thesis (open standards, export as product requirement, documented APIs) is the exit mechanism and must stay load-bearing. A self-hostable deployment path, or a named interim with a committed exit, keeps this honest. FedRAMP constraints are real and may *be* the values call — but it must be called.
**Status.** Open.

## 3. License — RESOLVED
**Problem.** No license was stated anywhere in the spec.
**Constraints inherited.** NTARI's fundamental commitment (bylaws §1.4) is open source for all systems the Corporation develops; provenance is inbound = outbound copyleft with no CLA (GOV-10). Sibling seeds use AGPL-3.0.
**Status.** **Resolved 2026-07-22 by decision (Jodson): AGPL-3.0.** `LICENSE` file landed at the repo root; the spec header now declares it. Entry retained as record per the removal discipline — resolved, not silently absorbed.

## 4. Community responder policy (spec §13.9 — the highest-stakes cluster)
**Problem.** Good Samaritan posture and consent waiver; qualifying incident types (v1 cardiac arrest only?); scene-safety exclusion rules; public locations vs. private residences (rural value vs. patient privacy); responder background checks; accepted credentials and re-verification cadence; AED registry source; responder debrief; per-scene activation cap.
**Constraints inherited.** Scene safety precedes response speed, always; activation supplements and never replaces professional dispatch; responder location ephemeral, no persistent tracking; the platform supplies guardrails, the agency makes the legal call.
**Status.** Open — decide before Phase 5.

## 5. Citizen-report retention vs. public-records law
**Problem.** Default purge windows for citizen submissions against jurisdictions' public-records obligations.
**Constraints inherited.** The record split the architecture uses: operational/audit facts are immutable (the spec's AuditEvent already is), while media and PII carry retention tags and purge — deletion applies to the erasable layer, never to the audit trail.
**Status.** Open (spec §13.6).

## 6. Remaining §13 build decisions
**Problem.** Official name (§13.1); comparator framing (§13.2); build-vs-integrate the IPAWS/cell-broadcast transport (§13.3); geospatial stack — open MapLibre/PostGIS vs. Esri (§13.4); native vs. cross-platform mobile (§13.5); GovCloud vs. Azure Government hosting (§13.7); volunteer/rural pricing tier (§13.8).
**Constraints inherited.** §13.8 carries the anti-lock-in thesis into the data model from day one; §13.4 weighs an open stack against agency interop — prefer the open one where parity holds (legibility and exit). The rest are product calls with no architectural bind.
**Status.** All open; §13.8 shapes the schema and should be decided first.
