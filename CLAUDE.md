# CLAUDE.md — COER Platform

> **Project:** COER — *Coordinated Operations & Emergency Response* platform
> **Audience:** Human engineering team + AI coding agents working in this repo
> **Status:** Planning / pre-build (v0 spec)
> **License:** AGPL-3.0 (adopted 2026-07-22; full text in `LICENSE`). Network use counts as distribution; all deployments and modifications, including hosted/server-side, disclose source under the same license. Every source file declares AGPL-3.0.
> **Last updated:** 2026-07-22

---

## 0. Read this first

This file is the single source of truth for *what we are building and why*. It synthesizes the
capabilities of four incumbent systems and folds them into one platform, with deliberate
departures from how mass notification works today (see **§5**).

**Naming note:** "COER" is expanded here as *Coordinated Operations & Emergency Response* as a
working placeholder. If the team has a fixed expansion, replace it globally — nothing downstream
depends on the words.

**Provenance of the specs below:** distilled from public vendor documentation and reporting as of
mid-2026 (ESO, ImageTrend, Everbridge, Genasys/Zonehaven, USFA/FSRI NERIS materials). Product
feature sets change frequently; treat the competitor tables as a design baseline, not gospel, and
re-verify before making a build-vs-buy or parity decision.

---

## 1. Why this project exists

Fire/EMS software has consolidated under a handful of private-equity-backed vendors. A December 2025
investigation documented small and volunteer departments seeing renewal costs jump several-fold
(e.g., a department paying ~$795/yr told to move to a ~$5,000 product after their tool was acquired;
another going from ~$4,000 to ~$12,000), with legacy products retired to force migrations and
limited affordable alternatives once a vendor buys up the competition. The three dominant fire
records vendors — **ESO**, **ImageTrend**, and **First Due** — are all PE-funded.

COER's thesis is therefore as much about *market posture* as features:

- **Open by default.** Built natively on federal/open standards (NERIS, NEMSIS, CAP) so an agency's
  data is always exportable and the agency is never locked in. Data portability is a *product
  requirement*, not a courtesy.
- **Tiered, transparent pricing** that does not price out volunteer/rural departments. Pricing logic
  must not be hard-coded around per-seat extraction; the data model should support
  small/volunteer-department tiers from day one.
- **Interoperable, not walled.** Every capability ships with a documented public API.

These are design constraints. When a tradeoff arises between "easier to build a closed integration"
and "preserves agency data portability," portability wins.

---

## 2. Scope

### In scope (two product categories + a differentiator with three parts)

1. **Records & operations** (parity with ESO / ImageTrend) — incident reporting, ePCR, inspections,
   pre-plans, assets, scheduling, personnel, NERIS/NEMSIS reporting.
2. **Mass notification** (parity with Everbridge / Genasys) — multi-channel public + responder
   alerting, IPAWS, geo-targeting, two-way acknowledgement.
3. **THE DIFFERENTIATOR (see §5)** — three capabilities built on one shared geolocation engine:
   1. **Dynamic areas** — notification areas are drawn and adjusted in real time by **on-scene
      commander(s)** as the incident evolves, and that geometry drives who gets alerted.
   2. **Citizen reply** — people inside an area can reply with situational intelligence (text,
      photo, ≤30-second video) that feeds the commander's common operating picture.
   3. **Community responders** — people with verified lifesaving credentials (starting with
      CPR/AED) can opt in to be alerted to a nearby, time-critical emergency they could reach
      before the professional response arrives.

### Non-goals (explicitly out, for now)

- We are **not** rebuilding a CAD (computer-aided dispatch) or a 911 call-taking system. We
  **integrate** with CAD/NG911, we do not replace them.
- We are **not** a national data repository. NERIS (run by USFA, free to departments) is the system
  of record at the national level; COER is the **agency-side platform that feeds NERIS** and gives
  the agency better day-to-day tooling than the bare federal app.
- We are **not** building hardware. Acoustic/siren hardware (cf. Genasys LRAD) is an *optional
  integration*, not a COER deliverable.
- No predictive wildfire/traffic modeling in v1 (revisit in a later phase; see §12).

---

## 3. Competitive baseline — Records & Operations

| Capability | ESO (ESO Suite) | ImageTrend (Elite) | **COER requirement** |
|---|---|---|---|
| ePCR / EHR | ESO EHR, NEMSIS v3.5 | Elite ePCR, NEMSIS v3.5, AI Assist (voice-to-text, image/OCR), Dynamic Power Tools | NEMSIS v3.5 ePCR, offline-first, configurable forms, optional AI-assisted entry |
| Fire RMS | Fire Incidents, Properties & Inspections, Personnel, Scheduling, Asset Mgmt, Checklists, Hydrants, Permits | Elite Fire (incidents, inspections, investigations, pre-plans, scheduling, exposure tracking, permits) | Full RMS module set; configurable; combined-department friendly |
| Combined Fire + EMS | Yes (shared incident data) | Elite Rescue (single solution) | Single incident object spanning fire + medical; enter once, reuse |
| Offline field capture | Yes | Elite Field (encrypted, syncs on reconnect) | **Offline-first** is mandatory (degraded comms is the norm at scene) |
| National reporting | NERIS-ready (formerly NFIRS) | NFIRS 5.0 + NERIS; 43 state EMS repositories | **NERIS-native** (not an NFIRS retrofit) + state repository submission |
| Hospital interoperability | Health Data Exchange (HDE), EMPI matching, ESO Alerting | Carelytics (bidirectional EMS↔hospital, encounter matching) | Bidirectional EMS↔hospital exchange; patient/encounter matching |
| Analytics | ESO Insights | Continuum (benchmarking), Data Mart | Built-in analytics + benchmarking; export to BI tools |
| Billing | via acquisitions | Billing Bridge | Billing export / partner integration (not core v1) |
| Platform / integrations | Cloud; CAD + monitor integrations | Cloud; CAD, EKG monitors (ZOLL/Philips/Physio), hospital EMRs, Tableau/SAS, MS SQL; Android/iPad/Windows | Cloud, cross-platform field app; documented CAD + EMR integration APIs (no cardiac-monitor integration in v1) |

**Takeaways for COER:** parity here is *table stakes* — both incumbents are mature. We win on (a)
NERIS-native + open export, (b) offline-first field UX, and (c) the fact that records and
notification live in **one** incident object, so a commander's actions and a citizen's reports
attach to the same record that later becomes the NERIS submission.

---

## 4. Competitive baseline — Mass Notification

| Capability | Everbridge (360 / Public Warning) | Genasys (Protect: EVAC + ALERT + Acoustic) | **COER requirement** |
|---|---|---|---|
| Channels | 25+ paths: SMS, voice, email, push, desktop, digital signage, collaboration tools, audio | Multi-channel alerting + 360° outdoor acoustic (LRAD) | SMS, voice, email, app push, IPAWS; acoustic as optional integration |
| Public alerting / IPAWS | IPAWS (WEA, EAS, NWEM), FEMA forms, follow-up & cancel; cell broadcast; location- & address-based SMS; Device-Based Geo-Fencing | IPAWS-WEA integration; no opt-in for in-area warnings | IPAWS via CAP (WEA/EAS/NWEM); **no opt-in** for in-area public warning; follow-up & all-clear |
| Geo-targeting | Pre-loaded polygons, draw circles/areas; targets individuals not devices | **Drawable/editable intelligent zones**, all-hazard, GIS-layer import; zone status updated by responders | **Commander-drawn dynamic zones** (see §5) + GIS import + address/contact targeting |
| Two-way | Acknowledge, confirm safe, share status, request help; real-time delivery visibility | Public-facing status map + app (largely one-way status viewing) | Acknowledgement **plus** rich inbound citizen reporting (see §5) |
| Public-facing situational picture | Visualize incident impact, crowd movement, device density | Public map + app (no login) showing zone status, road closures, shelters, animal collection points | Public status map/app (no login to view) + a **secure inbound reply path** |
| Modeling | AI workflows, alert mapping (2025) | Predictive simulation; TRAFFIC AI | Defer to later phase |
| Common operating picture | Cross-system visibility | COP across agencies/mutual aid (claims ~90% faster response) | Real-time COP shared across agencies/mutual aid |
| Ops controls | Templates, escalation, call-throttling, scheduled comms, notification-cycle controls | Zone library, all-hazard switching | Templates, escalation, throttling, scheduling, multi-language |
| Security/compliance | ISO 27001, GDPR, 325+ controls, data residency | Public-safety deployments incl. Esri/ArcGIS integration | See §6 |

**Takeaways for COER:** Everbridge is the broadest notification engine; Genasys owns the
**dynamic-zone + first-responder-updates-the-map** model and the no-opt-in in-area warning. COER
combines Everbridge's channel breadth with Genasys's zone model — and then adds the inbound channel
neither treats as a first-class capability.

---

## 5. THE DIFFERENTIATOR — dynamic areas, citizen reply & community responders

This is the reason COER exists as a distinct product. **Build this with the same rigor as a
safety-critical system**, because it is one. Three pillars, all built on the same geolocation engine
("who is in the area of this incident?"):

### 5A. Dynamic, commander-controlled notification areas

Today, public-warning polygons are usually pre-loaded or drawn from an EOC/dispatch console.
Genasys lets responders update *zone status*; we go further: **the on-scene incident commander (and
delegated command staff) define, move, grow, shrink, split, merge, and retire the notification area
itself, live, from a field device, as the incident evolves** — and that geometry is what drives who
gets alerted.

**Functional requirements**
- Commander can define an alert area by (a) drawing a polygon, (b) dropping a circle (point +
  radius), or (c) selecting from the agency's pre-built GIS zone library (Esri/ArcGIS or
  GeoJSON import).
- Areas are **mutable in real time**: resize, drag, rotate, split, merge, retire. Each mutation
  recomputes the alert audience and re-targets within seconds.
- Each area carries: hazard type, **status** (e.g., *evacuation order / evacuation warning /
  shelter-in-place / advisory / all-clear*), protective-action instructions, effective time, and
  the issuing role.
- **"Entering the area" semantics:** devices that enter an active area *after* issuance also receive
  the warning (cf. Device-Based Geo-Fencing / cell broadcast). Configurable per area.
- **Multi-commander / Unified Command (ICS/NIMS-aligned):** multiple authorized users edit
  concurrently. The system shows live presence, prevents silent overwrite (locking or
  section-scoped ownership mapped to ICS Divisions/Groups), and resolves conflicts deterministically
  with a full audit trail.
- **Authority gating:** issuing a public **WEA/IPAWS** alert (which is legally weighty and not
  cleanly recallable) is gated to specific credentialed roles (e.g., IC / authorized Alerting
  Authority). Lower-privilege roles can draft/stage areas and send internal-only or app-only
  notifications, subject to approval.
- **Versioning & replay:** every area state is timestamped and attributed; the full incident
  timeline is replayable for after-action review and is exportable as a public record.

**Acceptance criteria (v1)**
- [ ] A commander on a tablet over a congested/FirstNet link can draw an area and trigger a
      multi-channel alert to everyone inside it in **< 60 s** end-to-end.
- [ ] Resizing an active area updates the target audience and reissues/cancels appropriately, with
      no duplicate alerts to recipients who were already covered.
- [ ] Two commanders editing the same incident never silently clobber each other; every change is
      attributable in the audit log.
- [ ] WEA/IPAWS issuance is impossible for an un-credentialed role and always produces an immutable
      audit entry.
- [ ] Targeting reaches non-app users via cell broadcast/SMS (no opt-in required for in-area public
      warnings) and app users via push.

### 5B. Bidirectional citizen situational-awareness intake

**This is the genuinely novel piece.** Everbridge and Genasys public-facing experiences are
predominantly *outbound* (alerts) plus lightweight acknowledgements/status viewing. COER makes the
public a **sensor network**: people inside an active area can reply with intelligence that sharpens
the commander's picture.

**Functional requirements**
- Anyone who receives an alert can submit, **without creating an account**:
  - **Text** (bounded length),
  - **Photo(s)**,
  - **One video, ≤ 30 seconds.**
  - App users submit in-app; non-app users get a **one-time secure web link** in their SMS.
- Each submission is auto-enriched: geolocation (with explicit consent), timestamp, the area/zone it
  came from, and an anonymized device/report ID.
- Submissions land in a **moderated commander feed**, clustered on the map by location, with triage
  states (*new / under review / actioned / dismissed*), severity tagging, and de-duplication of
  near-identical reports.
- **AI triage (human-in-the-loop, never autonomous):** classify content (smoke/flame, flooding,
  injury/trapped, road blocked, downed lines, etc.), OCR text in images, transcribe video audio,
  surface high-signal reports, and **flag** spam/abuse/graphic content for human review. AI ranks
  and flags; humans decide.
- A reviewed report can be **promoted into the incident record** (attached to the NERIS narrative)
  and **shared cross-agency** on the common operating picture.

**Non-functional requirements (these are the hard part — design in from day 1, not later)**
- **Trust & safety:** rate limiting; abuse reporting; block/quarantine; automated CSAM/illegal-
  content scanning with a defined mandated-reporting workflow; graphic-content gating before a human
  reviewer sees it; misinformation handling (a report is *input*, never auto-broadcast back out).
- **Privacy & civil liberties:** location is used **only** for alert targeting and report context —
  **no persistent tracking** of individuals; explicit, plain-language consent for location and
  media; clear notice that submissions may become **public record** and may be used operationally;
  configurable retention with automatic purge; bystander/minor protection (faces, plates, PII).
- **Evidence integrity:** hash each submission on ingest; preserve chain-of-custody metadata;
  immutable audit; export package suitable for investigation/legal discovery.
- **Scale & resilience:** survive a **surge** (thousands of concurrent uploads during a large
  incident) via progressive/chunked upload, server-side transcoding, queue + backpressure, and a
  **low-bandwidth mode**; degrade gracefully on congested cellular/FirstNet.
- **Accessibility & equity (§6):** alternate submission paths for users without smartphones;
  multi-language; WCAG 2.2 AA; ADA/Section 508.

**Acceptance criteria (v1)**
- [ ] A non-app user can open the SMS link and submit text + photo + a ≤30 s video over a weak
      cellular connection without the app, and it appears geotagged on the commander feed.
- [ ] The system rejects/queues gracefully under a simulated 5,000-concurrent-upload surge with no
      data loss and bounded latency.
- [ ] Every inbound submission is hashed, retention-tagged, and audit-logged at ingest.
- [ ] Graphic/abusive/illegal content is quarantined and never auto-renders to a commander without a
      content warning + an explicit reveal action; the mandated-reporting path is exercised in tests.
- [ ] A commander can promote a citizen report into the incident's NERIS narrative in one action,
      with provenance preserved.

### 5C. Community responder activation (verified skilled bystanders)

In rural and volunteer-served areas the professional response can be many minutes out — crews may
have to reach the station before they even roll. For out-of-hospital cardiac arrest (OHCA — the
heart stopping outside a hospital), bystander CPR and early use of an AED (Automated External
Defibrillator) are the highest-leverage interventions there are; survival drops roughly 10% for
every minute without them. COER lets people register **verified** lifesaving credentials (starting
with CPR/AED) and opt in to be alerted when a nearby, time-critical emergency happens that they
could reach **before** the professional response. This is the model PulsePoint Respond and GoodSAM
have proven; COER's additions are formal credential verification and tight coupling to the same
incident / zone / common-operating-picture engine as the rest of the platform.

**Functional requirements**
- **Credential upload & verification:** a user uploads a CPR/AED certification (e.g., American Heart
  Association, American Red Cross, ASHI) — and optionally a higher credential (EMR, EMT, RN, MD).
  The system verifies via issuer lookup where one exists (e.g., AHA eCards, Red Cross validation),
  otherwise OCR + manual review. Store credential type, issuer, ID, issue/expiry dates, and
  verification status/method/date. Track expiry, auto-lapse, and prompt re-verification (CPR cards
  typically expire ~2 years).
- **Opt-in & consent:** explicit opt-in with a plain-language scope ("you may be asked to help
  before EMS arrives; it is voluntary; you may decline any request"). An availability toggle
  (available / off duty). Credential tier recorded on the profile.
- **Activation logic:** when dispatch/CAD (or an on-scene unit) codes a qualifying incident — **v1:
  cardiac arrest only** — the system finds verified, available responders within a proximity/ETA
  threshold and invites those who can plausibly arrive before the apparatus. It compares estimated
  responder ETA (their location → scene) against estimated professional ETA and gates on "can beat
  or meaningfully bridge." It caps how many are activated (no scene-crowding) and prioritizes by ETA
  and credential tier.
- **Navigation:** route the responder to the patient **and** to the nearest registered AED (AED
  registry), with access notes where available.
- **Accept / decline / accountability:** the responder accepts or declines in one tap; accepted
  responders appear on the commander's COP (en route / on scene) so Incident Command can account for
  them (an ICS span-of-control concern), and the commander can **stand them down** at any time.

**Non-functional requirements (safety, privacy, and legal — the hard part; design in from day 1)**
- **Scene safety is non-negotiable:** the system **never** activates community responders to scenes
  flagged for violence, weapons, fire, hazmat, or other danger. Screen incident type and CAD
  scene-safety flags *before* inviting anyone. Responder safety comes before response speed, always.
- **Legal posture / Good Samaritan:** a platform that *recruits and directs* responders is a
  different legal posture than a spontaneous bystander. Operate within Good Samaritan statutes (which
  vary by state) and recognized community-first-responder frameworks; require an informed-consent
  waiver; define scope-of-practice per credential tier. The platform supplies the guardrails; the
  legal/policy decision is the agency's (see §13).
- **Patient privacy (a real tension):** activating a neighbor discloses that someone at a location
  needs help. Established programs restrict this to *public* locations partly for that reason — but
  the rural value is highest at private residences. Resolve this per §13; minimize disclosure to
  location + nature of emergency, never patient identity.
- **Responder privacy:** location is shared only while the responder is available/activated, with
  consent, and purged after the event. No persistent tracking (same rule as §5B).
- **No substitution:** community activation **never** replaces or delays the full professional
  dispatch — it supplements and bridges. Turning the feature off must not change professional
  dispatch.
- **Responder wellbeing:** many OHCA patients do not survive; provide or signpost debrief and support
  resources, as established CFR programs do.

**Acceptance criteria (v1)**
- [ ] A user can upload a CPR/AED cert and reach a verified state (issuer lookup where available,
      else manual review), with expiry tracked and auto-lapse on expiry.
- [ ] On a cardiac-arrest dispatch, only verified + available responders inside the ETA threshold are
      invited — and **no** responder is invited to any incident carrying a scene-safety flag.
- [ ] An invited responder can accept/decline in one tap; on accept they are routed to the patient
      and the nearest AED, appear on the commander COP, and can be stood down by the commander.
- [ ] The full professional response is dispatched regardless of community activation; disabling the
      community feature never alters professional dispatch.
- [ ] Responder location is shared only during an active activation and purged afterward; no
      persistent location history is kept.

**Design principle for §5:** all three pillars turn the public and the field into real-time
participants — while the professional/command layer stays in control. The citizen channel must
*augment* command judgment, never replace it, and unverified content is never rebroadcast; community
responders *supplement* the professional response, never substitute for it and never get routed into
danger. Keep humans — and scene safety — firmly in the loop on anything that touches the public.

---

## 6. Standards & compliance (must-haves)

Build to these from the start; retrofitting compliance is the most common reason gov platforms fail
procurement.

- **NERIS** — National Emergency Response Information System (USFA/DHS S&T/FSRI). Replaced NFIRS as
  the sole national fire/all-hazards data system (NFIRS sunset ~Dec 2025 / early 2026). Cloud-based,
  modular, API-driven, GIS-compatible, free to departments. **COER must submit to NERIS via its API
  and stay current with schema releases.** Support NFIRS historical-data import (NERIS does not carry
  legacy data forward).
- **NEMSIS v3.5** — national EMS data standard (ePCR).
- **IPAWS / CAP** — public alerting via FEMA's Integrated Public Alert & Warning System using the
  **Common Alerting Protocol (CAP)**; channels include **WEA** (Wireless Emergency Alerts / cell
  broadcast), **EAS** (Emergency Alert System), **NWEM**, and public feeds. Support follow-up &
  cancellation. Note recent IPAWS message-design updates (MDD 2.0) — track FEMA changes.
- **NENA i3 / NG911** — for CAD / Next-Generation 911 integration and geospatial call data.
- **FirstNet** — public-safety broadband (priority/preemption). The field/commander app must behave
  well on FirstNet and on congested commercial networks; support deployable/satellite fallback.
- **CJIS Security Policy** — if any law-enforcement/criminal-justice data is touched (MFA/advanced
  auth, encryption, audit, personnel screening).
- **HIPAA** — ePCR/patient data (PHI) handling, BAAs, encryption, minimum-necessary access.
- **FedRAMP / StateRAMP** — host on a FedRAMP-authorized cloud (e.g., AWS GovCloud / Azure
  Government); pursue authorization for gov sales.
- **SOC 2 Type II, ISO/IEC 27001** — baseline security posture.
- **WCAG 2.2 AA / ADA / Section 508** — mandatory for all public-facing surfaces (alerts, status
  map, submission flow) and strongly recommended internally.
- **Data residency & retention** — configurable per agency/jurisdiction; documented retention/purge
  for citizen submissions and PII.

---

## 7. Architecture overview

Cloud-native, event-driven, multi-tenant (one logical tenant per agency, with mutual-aid sharing).

```
                         ┌─────────────────────────────────────────────┐
   Field/Command app  ───┤  API Gateway (authn/authz, RBAC, rate-limit) ├─── Public alert app / SMS link
   (offline-first)       └───────────────────┬─────────────────────────┘
                                              │
         ┌───────────────┬───────────────┬───┴────────────┬────────────────────┬──────────────────┐
         │ Records svc   │ Notification  │ Zone/Geo svc    │ Citizen-intake svc │ COP / realtime    │
         │ (NERIS/NEMSIS)│ gateway       │ (PostGIS/ArcGIS)│ (media + moderation│  (WebSocket/push) │
         └──────┬────────┴──────┬────────┴───────┬─────────┴─────────┬──────────┴─────────┬─────────┘
                │               │                │                   │                    │
        Event bus (Kafka/NATS) ─┴────────────────┴───────────────────┴────────────────────┘
                │
   ┌────────────┴───────────────┬───────────────┬────────────────────┬─────────────────────┐
   │ PostgreSQL + PostGIS        │ Object store  │ Search/analytics   │ ML triage (async)    │
   │ (incidents, zones, records) │ (media)       │ (reports, BI)      │ (classify/OCR/STT)   │
   └────────────────────────────┴───────────────┴────────────────────┴──────────────────────┘
                │
   External: NERIS API · NEMSIS state repos · IPAWS/CAP gateway · carrier cell-broadcast/SMS ·
             CAD/NG911 · hospital EMR/HIE · Esri ArcGIS · AED & credential registries
```

**Suggested stack (proposals, not mandates — confirm in §13):**
- **Backend:** TypeScript/Node or Go (latency-sensitive paths in Go); **PostgreSQL + PostGIS** for
  geospatial; Redis (cache/locks); **Kafka or NATS** for the event bus; S3-compatible object store
  for media.
- **Geospatial:** PostGIS + **MapLibre GL** (open) and/or **Esri ArcGIS** (Genasys/agency GIS
  interop). Support GeoJSON import/export.
- **Notification gateway:** pluggable providers (carrier cell-broadcast partner, SMS/voice provider,
  push); a dedicated **CAP/IPAWS connector**.
- **Citizen-intake pipeline:** chunked upload → virus/CSAM scan → transcode (video) → EXIF/geo
  extract → moderation queue → ML triage (async).
- **Frontend:** React + map SDK for web consoles; **React Native or native iOS/Android** for the
  field/command app and the public app (native preferred for offline + background + push reliability).
- **Infra:** containers on Kubernetes, FedRAMP-authorized cloud; IaC; per-tenant isolation;
  encryption in transit + at rest.

**Cross-cutting principles:** offline-first field clients with conflict-free sync; every
state-changing action is audited; geometry changes and alert issuances are first-class domain events;
no PII in logs.

---

## 8. Core data model (entities)

Keep records, notification, zones, and citizen reports unified under a single **Incident**.

- **Agency** — tenant; jurisdiction GIS; pricing tier; NERIS/NEMSIS credentials.
- **User / Role** — RBAC; ICS role mapping (IC, Ops Chief, Division/Group Supervisor, etc.);
  alerting-authority flag; MFA.
- **Incident** — the spine. Links records + zones + alerts + citizen reports. Maps to a NERIS
  submission.
- **PatientCareRecord (ePCR)** — NEMSIS v3.5; offline-capable; hospital-exchange linkage.
- **RmsRecord** — inspections, pre-plans, hydrants, assets, scheduling, personnel, exposure logs.
- **Zone / AlertArea** — geometry (polygon/circle/GIS-ref), status, hazard type, instructions,
  effective window, issuing role; **versioned** with full history.
- **Notification** — channel set, audience snapshot, CAP/IPAWS payload, delivery + acknowledgement
  telemetry; follow-up/cancel linkage.
- **CitizenReport** — text/photo/video; geo + timestamp + area ref; anonymized reporter ID;
  moderation state; triage labels; integrity hash; retention tag; promotion-to-incident link.
- **Responder** — opt-in community/skilled responder; availability state; credential tier; consent
  record; location retained only during an active activation.
- **Credential** — type, issuing body, cert ID, issue/expiry dates, verification status/method/date;
  linked to a Responder/User; auto-lapses on expiry.
- **AED** — registered defibrillator: location, access notes/hours, readiness status.
- **Activation** — a community-responder dispatch tied to an Incident: qualifying event, responders
  invited + their ETAs, accept/decline, check-in/out, nearest-AED reference, outcome; fully audited.
- **AuditEvent** — immutable; every state change, alert issuance, zone mutation, moderation action,
  and responder activation.

---

## 9. Integrations

- **NERIS API** (submit incidents; track schema versions) + NFIRS historical import.
- **NEMSIS** state repositories.
- **IPAWS/CAP** gateway (WEA/EAS/NWEM).
- **Carrier cell-broadcast + SMS/voice** providers.
- **CAD / NG911 (NENA i3)** — inbound incident/dispatch + geospatial call data.
- **Hospital EMR / HIE** — bidirectional ePCR exchange + encounter matching.
- **Esri ArcGIS** — agency GIS layers ↔ zone library.
- **Credential verification** — issuer lookup where available (e.g., AHA eCards, American Red Cross);
  OCR + manual-review fallback; covers CPR/AED and higher clinical credentials.
- **AED registry** — registered defibrillator locations/status for routing responders (e.g.,
  PulsePoint AED / national & regional registries).
- **BI tools** (export) — Tableau/Power BI/etc.
- **Acoustic hardware** (optional) — siren/voice broadcast as an alert channel.

Every integration ships with a documented public API and a data-export path (portability requirement
from §1).

---

## 10. Security, privacy & trust-and-safety (first-class)

This section is **not optional** and is owned by engineering, not "added later by legal."

- **AuthN/AuthZ:** SSO + MFA; least-privilege RBAC; ICS-role-aware permissions; alerting-authority
  gating for WEA/IPAWS.
- **Data protection:** encryption in transit + at rest; per-tenant isolation; PHI handled to HIPAA;
  no PII in logs/telemetry; configurable data residency.
- **Citizen privacy:** location used only for targeting/report context; **no persistent individual
  tracking**; explicit consent; plain-language public-records + operational-use notice; automatic
  retention purge; bystander/minor PII handling.
- **Trust & safety for inbound media:** CSAM/illegal scanning + mandated-reporting workflow;
  graphic-content gating; rate limits; abuse reporting; quarantine; spam/dedup; AI flags but humans
  decide; unverified content is never auto-rebroadcast.
- **Community responder safety & privacy:** **never** activate responders to scenes flagged for
  violence, weapons, fire, hazmat, or other danger (screen incident type + CAD scene-safety flags
  first — responder safety before response speed); share responder location only while
  available/activated, then purge; no persistent tracking; verify credentials + enforce expiry;
  community activation **supplements, never replaces or delays** the professional response.
- **Good Samaritan / legal framework:** a system that recruits and directs responders carries a
  different legal posture than a spontaneous bystander — require an informed-consent waiver, define
  scope-of-practice per credential tier, and operate within state Good Samaritan law and recognized
  community-first-responder frameworks. The platform enforces the guardrails; the legal/policy call
  is the agency's (see §13).
- **Evidence & accountability:** ingest hashing; chain of custody; immutable audit; legal-discovery
  export.
- **Resilience:** surge handling, backpressure, low-bandwidth mode, graceful degradation on
  congested/FirstNet networks; multi-AZ; tested failover.

---

## 11. Working in this repo (for humans and AI agents)

> Fill the bracketed commands in once the scaffold lands; keep this section accurate — agents rely
> on it.

- **Setup:** `[TODO: bootstrap command]`
- **Run (dev):** `[TODO]`
- **Test:** `[TODO]` — *no PR merges with failing tests or below coverage threshold.*
- **Lint/format:** `[TODO]`
- **Migrations:** `[TODO]`

**Conventions / guardrails for code changes:**
- Treat **§5 and §10 as safety-critical.** Changes to zone geometry → alert targeting, to
  WEA/IPAWS issuance, to citizen-media moderation, or to community-responder activation (scene-safety
  screening, ETA gating, credential verification) require tests + human review. No autonomous agent
  merges in those paths.
- **Audit everything** that changes incident, zone, notification, or report state. If an action
  isn't auditable, it isn't done.
- **No PII in logs.** Ever.
- **Standards are contracts:** if you touch NERIS/NEMSIS/CAP payloads, update the conformance tests
  in the same PR.
- **Offline-first:** field-client features must define their sync/conflict behavior, not assume
  connectivity.
- Prefer small, reviewable PRs. Document any new external integration's API + export path.

---

## 12. Roadmap (phased)

- **Phase 0 — Discovery & foundation:** ICS/NIMS workflow research + ride-alongs with departments;
  finalize standards/compliance scoping; lock the data model; choose stack (§13); set up
  FedRAMP-track infra, CI, audit framework.
- **Phase 1 — Records core (parity):** NERIS-native incident + NEMSIS ePCR, offline-first field app,
  core RMS modules, CAD ingestion, state-repository submission, open export.
- **Phase 2 — Notification engine (parity):** multi-channel outbound, IPAWS/CAP (WEA/EAS),
  templates, escalation, throttling, two-way acknowledgement, delivery analytics, public status map.
- **Phase 3 — Dynamic commander-controlled areas (§5A, *differentiator*):** field-drawn/mutable
  zones, ICS-aligned multi-commander concurrency, authority gating, versioning/replay.
- **Phase 4 — Bidirectional citizen intake (§5B, *novel differentiator*):** text/photo/≤30 s video
  inbound, account-free SMS-link path, moderated commander feed + COP fusion, full trust-&-safety,
  privacy, evidence integrity, surge resilience.
- **Phase 5 — Community responder activation (§5C, *differentiator*):** credential upload +
  verification, opt-in responder profiles, AED registry, dispatch-triggered activation with
  scene-safety screening + ETA gating, COP visibility + stand-down. Ships only behind the full
  trust-&-safety, privacy, and legal framework (§10/§13).
- **Phase 6 — Intelligence & hardening:** AI triage maturation, benchmarking analytics, predictive /
  traffic modeling (revisit non-goal), acoustic-hardware integration, scale + security hardening,
  certifications.

Phases 3–5 are the bet; do not let "parity" phases consume the whole budget.

---

## 13. Open questions / decisions for the human team

> **Canonical home:** these now live in [`OPEN-QUESTIONS.md`](./OPEN-QUESTIONS.md) — the living open-questions deliverable required by Janus-Facing Architecture — with the constraints each resolution inherits (including two this section doesn't carry: the JFA-canon question and the missing license). Update there; this list is a summary.

1. **COER expansion** — confirm the official name (placeholder: *Coordinated Operations & Emergency
   Response*).
2. **Primary competitor framing** — we benchmarked **ImageTrend** (records) and **Genasys** (notify)
   as the closest analogues. Confirm; First Due (records) and Everbridge-only (notify) are
   alternative comparators if procurement wants them.
3. **Build vs. integrate the alert transport** — partner with an existing IPAWS/cell-broadcast
   provider vs. building the CAP gateway in-house? (Affects timeline + certification path.)
4. **Geospatial stack** — open (MapLibre + PostGIS) vs. Esri ArcGIS vs. both? Agencies already run
   Esri; weigh interop vs. licensing cost.
5. **Native vs. cross-platform mobile** — offline + background + push reliability argue for native;
   speed/cost argue for React Native.
6. **Citizen-report retention defaults** — what is the default purge window, and how do public-
   records laws in target jurisdictions constrain it?
7. **Hosting** — AWS GovCloud vs. Azure Government for the FedRAMP/StateRAMP path.
8. **Pricing model** — define the volunteer/rural tier now so the data model and licensing reflect
   the §1 anti-lock-in thesis from day one.
9. **Community responder policy (§5C)** — the highest-stakes new decisions: (a) legal/Good Samaritan
   posture + informed-consent waiver; (b) which incident types trigger activation (v1: cardiac arrest
   only?); (c) scene-safety exclusion rules; (d) public locations only vs. private residences (the
   rural-value-vs.-patient-privacy tension); (e) background checks for responders — yes/no?; (f)
   accepted credential types + re-verification cadence; (g) AED registry source; (h) responder
   debrief/wellbeing support; (i) cap on responders activated per scene.

---

## 14. Glossary

- **AED** — Automated External Defibrillator (portable device that can restart a heart in cardiac arrest).
- **CAD** — Computer-Aided Dispatch.
- **CAP** — Common Alerting Protocol (the standard IPAWS messages use).
- **CEM** — Critical Event Management.
- **CFR** — Community First Responder (a trained volunteer who responds to nearby emergencies).
- **COP** — Common Operating Picture.
- **ePCR / EHR** — electronic Patient Care Report / Electronic Health Record (EMS documentation).
- **EAS / WEA / NWEM** — Emergency Alert System / Wireless Emergency Alerts / Non-Weather Emergency
  Messages (IPAWS channels).
- **ICS / NIMS** — Incident Command System / National Incident Management System (the command
  structure "on-scene commander(s)" refers to).
- **IPAWS** — Integrated Public Alert & Warning System (FEMA).
- **NEMSIS** — National EMS Information System (EMS data standard).
- **NERIS** — National Emergency Response Information System (fire/all-hazards data standard; replaced
  NFIRS).
- **NG911 / NENA i3** — Next-Generation 911 and its architecture standard.
- **OHCA** — Out-of-Hospital Cardiac Arrest (the heart stopping outside a hospital setting).
- **RMS** — Records Management System.
