# TASKS — open-map-gaps

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

## How these tasks map to Elyos

Each task below becomes an Elyos **Task JSON** validated against
`packages/schema/src/schemas.ts`. Field mapping:

- `id` — stable slug ID from the tables (e.g. `open-map-gaps-template-004`).
- `title` — the table's Title.
- `project` — `open-map-gaps`.
- `type` — one of `code | research | writing | data | design-spec | maintenance` (per table).
- `lane` — `donated` for all tasks here (no funded escrow). A funded task would add `fundedBudgetUsd`.
- `priority` — `high | medium | low`.
- `domain` — array, e.g. `["humanitarian","geospatial","open-data","osm"]`.
- `riskTier` — `low | medium | high`. Most packs are **medium**; sensitive/conflict/vulnerable-
  population contexts escalate to **high** (expert/ethics sign-off before mapping).
- `urgent` — boolean; `false` for all current tasks (a real disaster-response pack could be `true`).
- `deliverable` — `pr | dataset | document | translation`. Packs/guides/playbooks → `document`;
  reviewed candidate-feature sets / MapRoulette challenges (derived from OSM) → `dataset` (ODbL);
  tooling → `pr`; translated mapping instructions → `translation`. **There is no "OSM changeset"
  deliverable** — the edit itself is the human/steward's last-mile action.
- `tokenEstimate` — `small | medium | large` (Size column).
- `status` — `open | in-progress | review | delivered | done`; all start `open`.
- `context`, `objective`, `acceptanceCriteria[]`, `resources[]`, `output` — per task.
- `requestor` — **TO BE SECURED** until a partner/local community is confirmed.
- `verifiedNeed` — **`false`** until a named partner confirms the area, the real use, and local
  consent (general humanitarian-mapping need is real; per-pack delivery need is unproven).
- `outputLicense` — **`ODbL-1.0`** for any output derived from OSM data (candidate sets, MapRoulette
  challenges, the upstream edits); **`MIT`** for code; **`CC-BY-4.0`** for standalone docs/guides
  not derived from OSM data.

**Hard rule reflected in every task:** no task may write to the OSM API. AI detects, drafts, and QA-
checks; a human maps and a validator confirms. Any bulk/mechanical contribution follows the OSM
Import Guidelines, Automated Edits CoC, and OSMF Organised Editing Guidelines.

---

## Milestone M0 — Foundation & cold-start

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| open-map-gaps-reviewer-001 | Name/secure the blocking reviewer roles (licensing/imagery + do-no-harm + experienced validator) | research | small | low | document | — | Maintainer |
| open-map-gaps-imagery-gate-002 | Imagery + source-data licensing gate (blocking, ODbL-compatibility + authorized-imagery checklist) | design-spec | small | medium | document | — | Licensing/imagery |
| open-map-gaps-harm-gate-003 | Do-no-harm / privacy & sensitive-feature screen (blocking) | design-spec | small | high | document | — | Do-no-harm |
| open-map-gaps-template-004 | Task-pack template + canonical pack model | writing | small | low | document | — | Technical |
| open-map-gaps-tagging-005 | Tagging / feature-schema profile (HOT presets) | design-spec | small | medium | document | template-004 | Technical |
| open-map-gaps-outreach-006 | Partner / local-community outreach + region shortlist | research | small | low | document | — | Steward |
| open-map-gaps-pilot-007 | One pilot task pack end-to-end → mapped, validated, merged | data | medium | medium | document | reviewer-001, imagery-gate-002, harm-gate-003, template-004, tagging-005, outreach-006 | Licensing/imagery, Do-no-harm, Validator |

**Acceptance criteria — key tasks**

- **reviewer-001 (secure blocking reviewer roles)**
  - [ ] A named, qualified **licensing/imagery reviewer** recorded (can assess OSM imagery
        permissions, ODbL compatibility, SPDX/CC/ODbL terms); ≥ 2 named to avoid a bottleneck.
  - [ ] A named **do-no-harm reviewer** recorded (humanitarian-protection / HOT-ethics background).
  - [ ] A named **experienced OSM validator** recorded (real TM-validator or equivalent experience).
  - [ ] Roles documented in PLAN.md Governance; until filled, all packs stay `verifiedNeed:false`
        and no pack may be mapped. Filling these is a **blocking prerequisite** to `pilot-007`.

- **imagery-gate-002 (licensing + imagery gate)**
  - [ ] Imagery section pins an **allowlist** of OSM-authorized layers (Bing, Esri, Maxar-via-OSM,
        Mapbox) with required attribution, and a **forbidden list** (Google et al.) = hard exclude.
  - [ ] Source-data section: PASS only if `odblCompatible: true` is recorded from a cited clause/URL
        (PD/CC0/ODbL/OSM-compatible waiver); CC-BY/share-alike/ambiguous → defer to the import-
        acceptance policy (`import-playbook-014`); unparseable/missing evidence = FLAG/EXCLUDE.
  - [ ] Requires a license/permission **snapshot**: committed copy + SHA-256 + Wayback URL (bare URL
        insufficient), recorded in `snapshotRef`.
  - [ ] Produces a committed, reviewable PASS/FLAG/EXCLUDE artifact per area recording which checks
        ran and what fired.

- **harm-gate-003 (do-no-harm screen)**
  - [ ] A rubric assessing whether mapping the area/feature set could expose vulnerable people
        (persecuted/displaced groups, shelters, safe houses, sensitive infrastructure).
  - [ ] Decision options: PROCEED / RESTRICT feature set / LOWER geo-precision / EXCLUDE; default to
        caution on any doubt.
  - [ ] Escalation rule: conflict/vulnerable-population contexts → `riskTier: high` + expert/ethics
        sign-off **before** mapping begins.
  - [ ] Enforces "people are not features": no occupant/household/individual data; partner-side
        de-identification + consent required for any field data.
  - [ ] Produces a committed, reviewer-signed do-no-harm artifact per pack (100% coverage).

- **pilot-007 (pilot pack, end-to-end)**
  - [ ] Pilot gated on a realistic *validated-merge* path first: a partner/local community agreed to
        map+validate a small area, or a self-serve-validatable fallback (low-sensitivity area with an
        active local community + independent validator).
  - [ ] Area passed `imagery-gate-002` (authorized imagery + ODbL-compatible sources) and
        `harm-gate-003` (signed) with both artifacts committed.
  - [ ] Pack assembled from `template-004` + `tagging-005`: AoI GeoJSON, tag schema, mapping +
        validation instructions, attribution, chosen channel (TM/MapRoulette/Notes).
  - [ ] Effort registered under OSMF Organised Editing (wiki page URL recorded).
  - [ ] Features mapped by a human and **validated + merged into OSM (not reverted)**; validator
        sign-off + changeset/TM references recorded in `outcomes/<pack-id>.json` — or, if no channel
        materializes, pack fully prepared with the blocker surfaced.
  - [ ] All OSM-derived outputs licensed `ODbL-1.0`; no OSM write credentials used by any tool.

**M0 Definition of Done:** blocking reviewer roles named (before pilot mapping); pack template +
canonical model + tagging profile + licensing/imagery gate + do-no-harm gate published and applied
to one area; one pilot pack mapped, validated, and **merged into OSM** under ODbL (evidence artifact
recorded) — or fully prepared with the blocker surfaced; effort registered as Organised Editing; ≥ 1
partner/community outreach thread opened.

---

## Milestone M1 — Gap-analysis toolchain & first validated edits

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| open-map-gaps-conflation-008 | Read-only gap-detection / conflation tool (authorized source vs OSM) | code | medium | medium | pr | template-004, imagery-gate-002 | Technical, Licensing/imagery |
| open-map-gaps-candidates-009 | Reviewed candidate-feature dataset format + generator | data | small | medium | dataset | conflation-008, harm-gate-003 | Technical, Do-no-harm |
| open-map-gaps-maproulette-010 | MapRoulette v2 challenge generator from a pack | code | medium | low | pr | candidates-009 | Technical |
| open-map-gaps-partner-011 | Secure first confirmed partner + (where applicable) local-community sign-off | research | small | low | document | outreach-006 | Steward |
| open-map-gaps-pack-012 | Task pack #2 delivered → mapped + validated + merged | data | medium | medium | document | pilot-007, conflation-008, partner-011 | Licensing/imagery, Do-no-harm, Validator |

**Acceptance criteria — key tasks**

- **conflation-008 (gap-detection / conflation tool)**
  - [ ] **Read-only**: queries OSM via Overpass/API and reads an authorized source dataset; **no
        write/upload code path; no OSM credentials.**
  - [ ] Emits a **reviewed candidate list** of missing/incomplete features (never an upload), with
        provenance + the source's `odblCompatible` evidence attached.
  - [ ] Ships committed golden fixtures (synthetic/public OSM-vs-source inputs → expected candidate
        list); `pnpm build && pnpm test && pnpm lint` green; MIT; DCO signed-off.
  - [ ] Pinned interface versions (OSM API v0.6, Overpass QL) recorded in `specVersions`.

- **candidates-009 (candidate-feature dataset format)**
  - [ ] Defines an ODbL-licensed candidate-feature format (GeoJSON) with per-feature provenance,
        confidence, and a `humanReviewRequired: true` flag — explicitly *not* an OSM-ready upload.
  - [ ] Passes through the do-no-harm filter: no people-level data; sensitive features dropped/
        flagged per `harm-gate-003`.
  - [ ] `outputLicense: ODbL-1.0` (derived from OSM); documents the share-alike obligation.

- **partner-011 (first confirmed partner)**
  - [ ] A named HOT hub / Missing Maps partner / NGO / local community confirms a specific area + a
        real use, and (where a local community exists) signs off / consents.
  - [ ] Contribution + validation channel documented (TM project / MapRoulette / Notes; who validates).
  - [ ] Tasks for that partner flip to `verifiedNeed: true` with `requestor` set.

**M1 Definition of Done:** conflation tool emits reviewed candidate lists from authorized sources
(read-only, golden fixtures, CI green); MapRoulette generator produces a valid v2 challenge; ≥ 2
packs mapped + validated + merged; ≥ 1 confirmed partner (+ local sign-off where applicable); ≥ 95%
validation pass rate on sampled edits with 0 license/imagery/PII reverts.

---

## Milestone M2 — Scale via catalog, QA & import governance

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| open-map-gaps-validation-013 | QA/validation tool (Osmose / iD-rule integration) for human fixing | code | medium | low | pr | conflation-008 | Technical |
| open-map-gaps-import-playbook-014 | Import + mechanical-edit governance playbook (Import Guidelines + Automated Edits CoC + Organised Editing) | design-spec | medium | medium | document | imagery-gate-002 | Licensing/imagery, Steward |
| open-map-gaps-notes-015 | OSM Notes batch generator from authorized sources (human-posted) | code | small | medium | pr | candidates-009, harm-gate-003 | Technical, Do-no-harm |
| open-map-gaps-packs-016 | Task packs #3–#5 across ≥ 2 regions → mapped + validated + merged | data | large | medium | document | pack-012, validation-013, import-playbook-014 | Licensing/imagery, Do-no-harm, Validator |

**Acceptance criteria — key tasks**

- **import-playbook-014 (import / mechanical-edit governance)**
  - [ ] Decides, in writing, when an effort triggers the full **Import Guidelines** vs. stays human
        MapRoulette/TM micro-tasking (default: prefer micro-tasking).
  - [ ] Encodes the **Automated Edits CoC** and **OSMF Organised Editing** requirements: dedicated
        import account, wiki documentation, imports@ discussion, license-compatibility check.
  - [ ] Decides the CC-BY / share-alike / ambiguous-source acceptance rule (default exclude/escalate),
        unblocking `imagery-gate-002`'s deferred cases.
  - [ ] Must be merged **before** any bulk/mechanical contribution runs.

- **validation-013 (QA/validation tool)**
  - [ ] Pulls Osmose / iD-validation-style issues for a pack's AoI and emits a human-fixable
        worklist; read-only; no auto-fix/upload.
  - [ ] Golden fixtures + CI green; MIT; pinned Osmose interface version in `specVersions`.

- **packs-016 (packs #3–#5)**
  - [ ] Each pack passes both gates with committed artifacts; effort registered as Organised Editing.
  - [ ] Mapped + validated + merged across ≥ 2 distinct regions; validator sign-off + references
        recorded per pack.
  - [ ] Validator-time per 100 validated features recorded; median improves vs. the M0/M1 baseline
        with **no** drop in pass rate.

**M2 Definition of Done:** QA tool flags likely errors for human fixing; import/mechanical-edit
governance playbook published and applied; ≥ 5 packs mapped+validated+merged cumulatively across ≥ 2
regions; measurable validator-time reduction vs. baseline with no pass-rate drop.

---

## Milestone M3 — Outcomes, local handover & sustainability

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| open-map-gaps-handover-017 | Local-community handover + validator training | writing | medium | medium | document | packs-016, partner-011 | Steward, Validator |
| open-map-gaps-outcomes-018 | Verify beneficiary/local use + reuse events | research | small | low | document | pack-012, packs-016 | Steward |
| open-map-gaps-maintenance-019 | Staleness / re-map refresh process (incl. post-disaster) | maintenance | small | low | document | validation-013 | Maintainer |

**Acceptance criteria — key tasks**

- **handover-017 (local handover + validator training)**
  - [ ] ≥ 1 region's pack/validation workflow handed to a local OSM community with a trained
        validator able to sign off independently.
  - [ ] Handover doc covers gates, tagging profile, channel setup, and Organised Editing registration.

- **outcomes-018 (verify beneficiary use)**
  - [ ] ≥ 1 externally verifiable **use event** recorded (a partner used the completed map for a real
        decision; evidence = written confirmation / report / TM project completion).
  - [ ] Validated-feature counts, pass rate, and reverts recorded in the outcome ledger; reverts-for-
        cause have a logged root-cause + gate/tooling fix.

**M3 Definition of Done:** ≥ 1 verifiable beneficiary/local use event; ≥ 1 region handed to a local
community with a trained validator; documented staleness/re-map process + named steward; ≥ 8 packs
validated+merged cumulatively.

---

## Backlog / future

| ID | Title | Type | Size | Risk | Deliverable | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| open-map-gaps-i18n-020 | Translate a pack's mapping instructions for local mappers | translation | small | medium | translation | Widens local uptake; needs language reviewer |
| open-map-gaps-buildings-021 | ODbL building-footprint candidate adapter (e.g., MS Buildings) | code | medium | medium | pr | **Contentious** — strictly candidate-list-only + Import Guidelines; never auto-import |
| open-map-gaps-dashboard-022 | Outcome dashboard (validated features, pass rate, use events) | code | medium | low | pr | Reads the outcome ledger; supports success metrics |
| open-map-gaps-rapid-023 | AI-assisted-tracing instruction guide (RapiD, human-confirmed) | writing | small | medium | document | Keeps AI assistance within OSM community norms |
| open-map-gaps-disaster-024 | Rapid-response pack template for active disasters | design-spec | medium | high | document | `urgent:true` variant; expert/ethics + HOT activation required |

---

## Example task JSON

```json
{
  "id": "open-map-gaps-reviewer-001",
  "title": "Name/secure the blocking reviewer roles (licensing/imagery + do-no-harm + experienced validator)",
  "project": "open-map-gaps",
  "type": "research",
  "lane": "donated",
  "priority": "high",
  "domain": ["humanitarian", "geospatial", "open-data", "osm", "governance"],
  "riskTier": "low",
  "urgent": false,
  "deliverable": "document",
  "tokenEstimate": "small",
  "status": "open",
  "context": "open-map-gaps contributes humanitarian map data upstream into OpenStreetMap (ODbL share-alike) only via OSM-authorized, human-in-the-loop processes. Three reviews are hard, non-skippable gates before any pack can be mapped: (1) a licensing/imagery reviewer who verifies imagery is OSM-authorized and every conflated/import-candidate source is ODbL-compatible; (2) a do-no-harm reviewer who screens for harm to vulnerable people and can restrict or exclude an area; (3) an experienced OSM validator who confirms edits are correct and merged (not reverted) before a deed counts as delivered. Until these roles are filled, no pack can pass the gates and all packs remain verifiedNeed:false.",
  "objective": "Identify and record named, qualified reviewers for the licensing/imagery, do-no-harm, and experienced-validator roles, so the M0 pilot pack can be reviewed and merged.",
  "acceptanceCriteria": [
    "A named, qualified licensing/imagery reviewer is recorded (can assess OSM imagery permissions, ODbL compatibility, and SPDX/CC/ODbL terms); at least two are named to avoid a single-person bottleneck.",
    "A named do-no-harm reviewer is recorded with humanitarian-protection or HOT-data-ethics background.",
    "A named experienced OSM validator is recorded with real validation experience (e.g. Tasking Manager validator role).",
    "Roles, names, and qualifications are documented in PLAN.md Governance; the licensing/imagery and do-no-harm roles are confirmed filled BEFORE pilot-007 is mapped.",
    "Until all roles are filled, the backlog stays verifiedNeed:false and no pack may be mapped; this constraint is recorded.",
    "No OSM write credentials are requested or stored; this task produces documentation only."
  ],
  "resources": [
    "C:\\code\\elyos\\planning\\projects\\open-map-gaps\\PLAN.md",
    "C:\\code\\elyos\\docs\\good-deed-definition.md",
    "Humanitarian OpenStreetMap Team (HOT) data-ethics / do-no-harm guidance",
    "OSM Automated Edits code of conduct; OSMF Organised Editing Guidelines",
    "OSM imagery permissions (Bing, Esri, Maxar via OSM agreement, Mapbox)"
  ],
  "output": "A governance document naming the licensing/imagery, do-no-harm, and experienced-validator reviewers (with qualifications and the ≥2-reviewer anti-bottleneck note), recorded in PLAN.md Governance and ready to gate the M0 pilot.",
  "requestor": "TO BE SECURED",
  "verifiedNeed": false,
  "outputLicense": "CC-BY-4.0"
}
```
