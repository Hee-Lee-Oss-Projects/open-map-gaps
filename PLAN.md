# PLAN — open-map-gaps

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated
>
> Humanitarian OpenStreetMap (OSM) data-completion **task packs** for under-mapped regions.
> ODbL share-alike. Contributed upstream only via OSM-authorized, human-in-the-loop processes.
> Risk tier: **medium** (escalates to **high** for conflict zones / vulnerable populations).

## Executive summary

In large parts of the world the basic map does not exist. Buildings, roads, health facilities,
schools, and water points that humanitarian responders need to plan a vaccination campaign, route
an ambulance, or estimate a population at risk are simply absent from every freely available map.
OpenStreetMap (OSM) is the one global commons where that gap can be closed in the open — and the
Humanitarian OpenStreetMap Team (HOT), Missing Maps, and local OSM communities have spent a decade
proving that filling it saves lives. The bottleneck is human attention: someone has to find *where*
the map is incomplete, scope a clean unit of work, define how to map it correctly and safely, then
do the careful tracing and validation.

**open-map-gaps produces the scoping, detection, quality-assurance, and documentation layer — not
the edits themselves.** Each deliverable is a **task pack**: a precisely-bounded mapping job for an
under-mapped area, containing the area-of-interest, the authorized imagery and source data to use,
the tagging/feature schema, mapping and validation instructions, a do-no-harm screen, and (where
appropriate) machine-readable **candidate-feature datasets**, **MapRoulette challenges**, or **OSM
Notes** that point a human mapper at exactly what is missing. AI is used to *detect probable gaps,
draft instructions, and run quality checks*. Humans — ideally including local mappers — make every
actual OSM edit, through OSM's own editors and authorized coordination tools.

This boundary is not a limitation we tolerate; **it is the design.** OSM is a community with binding
norms: the [Automated Edits code of conduct], the [Import Guidelines], and the OSMF [Organised
Editing Guidelines]. Edits that bypass them — especially AI-generated bulk edits — get reverted, and
the responsible accounts blocked, by the OSM Data Working Group. Hee-Lee Oss's own donated-lane rule is
identical in spirit: the platform "never invokes or authenticates a coding agent and never runs
headless; it prepares a task workspace... the human runs their agent." We map that rule onto OSM:
**no headless edits, ever; AI assists, a human decides and commits, the community validates.**

Risk tier is **medium**. The dominant risks are not software bugs but (1) contributing data under an
incompatible license or from unauthorized imagery, (2) **doing harm** by mapping vulnerable people
or sensitive features, (3) degrading the commons with low-quality AI-traced data that wastes
volunteer time, and (4) acting as an unaccountable "remote mapping" outsider with no local mandate.
The plan front-loads a licensing/imagery gate, a do-no-harm gate, an experienced-validator sign-off,
and explicit local-community partnership as **blocking** gates, not afterthoughts.

[Automated Edits code of conduct]: https://wiki.openstreetmap.org/wiki/Automated_Edits_code_of_conduct
[Import Guidelines]: https://wiki.openstreetmap.org/wiki/Import/Guidelines
[Organised Editing Guidelines]: https://wiki.openstreetmap.org/wiki/Organised_Editing_Guidelines

## Problem & beneficiaries

**Who is helped (the end beneficiaries).** People living in under-mapped regions, and the
humanitarian and public-service organizations that serve them: disaster responders routing relief,
epidemiologists estimating exposed populations, health ministries planning clinic catchments,
search-and-rescue teams, and local governments. A complete base map is a public good those
beneficiaries cannot otherwise buy — commercial maps under-serve exactly the low-income, rural, and
crisis-affected places that need them most.

**Intermediate beneficiaries.** The OSM ecosystem itself: local OSM communities gain validated data
and tooling; HOT/Missing Maps gain better-scoped, higher-quality task packs that reduce the
notorious "remote armchair mapping" quality problem.

**The verified need.** The *general* need is well established and externally documented (HOT,
Missing Maps, MSF "Missing Maps" campaigns, the academic literature on the global mapping gap). We
treat the general need as real. However, the **specific, per-region, per-partner need is TO BE
SECURED**: a task pack is only genuinely needed if a named humanitarian actor or local OSM community
actually wants *that area* mapped *now* for a real use. Without that, remote mapping can be
worthless or actively harmful (mapping the wrong thing, or exposing people). Therefore every task
carries **`verifiedNeed: false` until a named partner confirms the area, the use, and that local
stakeholders consent.** "Delivered, not merged" here means: features mapped, **validated by an
experienced mapper, accepted into OSM (not reverted), and usable by the requesting beneficiary.**

**Partner org.** TO BE SECURED. Candidate partners: the Humanitarian OpenStreetMap Team (HOT) and
its regional hubs, the Missing Maps coalition (HOT, MSF, British/American Red Cross), national/local
OSM communities and chapters, and field NGOs with a concrete mapping ask. M0 includes explicit
outreach; **no partner, region, or beneficiary is assumed.**

## Goals and non-goals

**Goals**
- Produce a reusable, standards-aligned **task-pack template + toolkit** (area-of-interest spec,
  tagging schema, imagery/source allowlist, mapping & validation instructions, do-no-harm screen).
- For each in-scope, partner-requested area, deliver a complete task pack — and where useful, a
  reviewed **candidate-feature dataset**, a **MapRoulette challenge**, or an **OSM Notes batch** —
  that lets human mappers close real gaps efficiently and correctly.
- Make **license/imagery verification** and the **do-no-harm screen** non-skippable, auditable gates.
- Use AI to *increase the quality and reduce the drudgery* of mapping (gap detection, instruction
  drafting, automated QA) **without ever performing an unattended edit.**
- Strengthen, not bypass, the OSM community: register as organised editing, route to local mappers,
  and follow Import / Automated-Edit / Organised-Editing governance.

**Non-goals (constraints that define the project)**
- We do **not** make automated, headless, or bot edits to OSM. Every edit is made by a human in a
  standard OSM editor (iD, JOSM, RapiD with human confirmation) or via human-validated micro-tasks.
- We do **not** bulk-import AI-detected geometry into OSM. AI output is a *reviewed candidate list a
  human maps from*, never a direct upload. Any genuine import follows the Import Guidelines in full.
- We do **not** trace from unauthorized imagery (e.g., Google Maps/Earth) or conflate data under an
  OSM-incompatible license. License/imagery failure = the area is excluded, not "best-guessed."
- We do **not** map vulnerable populations, individuals' private data, or sensitive features whose
  publication could enable harm (see Do-no-harm gate). On any doubt we stop and escalate.
- We do **not** map disputed borders partisanly; we follow OSM's on-the-ground rule and stay neutral.
- We do **not** "armchair map" a region with no local mandate or validation plan.
- We do **not** host or re-publish a parallel map; the upstream deliverable lands in OSM under ODbL.

## Success metrics (outcomes)

Outcome-based and beneficiary-centric. Vanity metrics ("packs written," "nodes drawn") are
explicitly excluded — a pack nobody maps, or edits that get reverted, count as **zero**.

| Metric | Baseline | Target (first 6 months) |
| --- | --- | --- |
| Features **mapped AND validated AND merged into OSM** (not reverted) via our packs | 0 | ≥ 5,000 validated features across ≥ 3 packs |
| Task packs **delivered to a partner and actually mapped** (last-mile) | 0 | ≥ 3 packs taken up and completed |
| Validation pass rate of edits made from our packs (validator-confirmed, not reverted in 30 days) | n/a | ≥ 95% of sampled edits accepted; **0 reverts for license/imagery/PII cause** |
| Confirmed partners / local communities engaged (named, consenting) | 0 | ≥ 2 secured (incl. ≥ 1 local community for the mapped region) |
| Do-no-harm screen coverage | n/a | **100%** of packs carry a completed, reviewer-signed do-no-harm artifact |
| Verified humanitarian/local **use** of the completed map (a partner uses it for a real decision) | 0 | ≥ 1 documented, externally verifiable use event |
| AI-assist efficiency: validator-time per 100 validated features vs. unassisted baseline | n/a | measurable reduction vs. recorded M0/M1 baseline, with **no** drop in pass rate |

Notes on attribution: a "use event" and a "validated feature" must be **externally verifiable** — an
OSM changeset history, a validator's sign-off record, a partner's written confirmation, a Tasking
Manager project completion. Self-reported impact does not count. Quality is the headline metric: it
is better to deliver one validated pack than ten that pollute the commons.

## Scope

**In scope**
- **Task packs**: bounded area-of-interest (AoI GeoJSON), feature/tagging schema, authorized
  imagery + source-data allowlist, mapping instructions, validation rules, do-no-harm screen,
  acceptance criteria, and the chosen contribution channel (Tasking Manager / MapRoulette / Notes).
- **Gap detection / conflation tooling**: compare OSM against **OSM-compatible** authorized open
  datasets (e.g., ODbL Microsoft Building Footprints, suitably-licensed government data, OSM's own
  quality tools) to find missing/incomplete features and emit a **reviewed candidate list**.
- **Quality-assurance tooling**: integrations with OSM QA tools (Osmose, OSM Inspector, iD/JOSM
  validation rules) to flag likely errors for human fixing.
- **Machine-readable outputs for human mappers**: MapRoulette challenge GeoJSON, OSM Notes batches
  (drafted, human-posted), JOSM/RapiD-ready candidate layers — all human-confirmed before any edit.
- **Documentation**: HOT-style mapping guides, organised-editing wiki pages, validator briefs.

**Feature classes we map (public infrastructure only).** Roads/paths, buildings (footprints, no
occupant data), health facilities, schools, water/sanitation points, markets, places of worship as
public landmarks, administrative/place names from authorized sources — i.e., the standard
HOT/Missing Maps feature set that supports humanitarian response.

**Out of scope**
- Any **automated/headless edit** to OSM; any **bulk import** of AI-detected geometry.
- Tracing from **unauthorized imagery**; conflating **OSM-incompatible** source data.
- **People-level data**: occupant names, household composition, individuals' locations, religion/
  ethnicity/health status of residents, anything re-identifying a person.
- **Sensitive features in at-risk contexts** (see Do-no-harm gate): e.g., mapping a persecuted
  group's settlements, shelters, or safe houses where visibility increases risk.
- **Disputed-boundary advocacy**; any partisan or contested civic editing.
- Hosting a competing map, re-licensing OSM data away from ODbL, or analytical "rankings" of places.
- Mapping a region with **no partner mandate, no local validation plan, or no real use.**

## Solution approach & architecture

This is a **content/data + light-software project**, not a pipeline that pushes data into OSM. The
software *finds and documents* gaps; humans *map* them.

**Pipeline (per task pack)**
1. **Need & region intake.** A named partner/local community requests an area and states the real
   use. Record requestor, use, and local-consent status. No request → no pack.
2. **Licensing & imagery gate (blocking).** Fix the *only* imagery layers and source datasets that
   may be used, each verified OSM-authorized / ODbL-compatible. Record evidence. Fail = exclude.
3. **Do-no-harm & privacy screen (blocking).** Assess whether mapping this area/feature set could
   expose vulnerable people. Escalate sensitive contexts to **high** risk + expert/ethics review.
   Fail = exclude or restrict feature set.
4. **Gap analysis (AI-assisted).** Conflate authorized open data and OSM QA signals against current
   OSM to locate missing/incomplete features. Output a **reviewed candidate list**, never an upload.
5. **Pack assembly.** Produce AoI, tagging schema (HOT presets), mapping instructions, validation
   rules, attribution, and the chosen channel artifact (TM project text / MapRoulette challenge /
   Notes batch). Register the effort under OSMF Organised Editing.
6. **Human mapping (authorized).** Local/remote mappers execute via iD/JOSM/RapiD (AI suggestions
   human-confirmed) or human-resolve the micro-tasks. Any true import follows the Import Guidelines
   (dedicated import account, wiki docs, imports@ discussion, license check).
7. **Validation.** An **experienced OSM validator** reviews the edits (sampled, via TM validation /
   Osmose / changeset review) before they count as delivered.
8. **Outcome capture.** Steward records validated-feature counts, pass rate, and any verified
   beneficiary use in an outcome ledger.

**Canonical pack model (single source of truth; all channel artifacts are projections of it):**
`id`, `region {name, aoiGeoJsonRef, adminLevel, partner, useStatement, localConsent}`,
`imagery {allowed[] {provider, layerUrl, osmPermissionRef, attribution}}`,
`sources[] {name, url, license {id, url, odblCompatible:boolean, permissionRef}, snapshotRef}`,
`features[] {key, tagSchemaRef, mappingNotes, validationRules}`,
`doNoHarm {assessed:boolean, riskLevel, reviewer, decision, restrictions[]}`,
`channel {type: tasking-manager|maproulette|notes|guided-edit, artifactRef}`,
`organisedEditingWikiRef`, `attribution`, `acceptanceCriteria[]`,
`outcome {validatedFeatures, passRate, revertCount, useEvents[]}`.

**Tech stack.** TypeScript, ESM, pnpm workspaces (Hee-Lee Oss convention). Geospatial tooling uses small
Node packages (`@turf/turf` for geometry, GeoJSON I/O); conflation/QA may shell out to read-only
queries against the OSM API / Overpass and Osmose. No runtime service; everything runs locally or in
CI and **emits files for human review** — never writes to the OSM API.

**Pinned external interfaces** (recorded in the pack model's `specVersions`, bumped only by a
deliberate task): OSM **API v0.6** (read-only use here), **Overpass QL**, **MapRoulette v2** challenge
schema, **Tasking Manager** project-creation fields, **Osmose** issue API, OSM standard tagging
(via the OSM wiki / iD presets and HOT preset set), and the OSMF Organised Editing activity template.

**Read-only OSM access protocol** (makes "AI never edits" enforceable): all programmatic OSM access
is **read-only** (Overpass/API reads, Osmose reads). The toolkit holds **no OSM write
credentials**; there is no code path that calls an OSM write/upload endpoint. Candidate layers are
written to local files for a human to open in an editor. CI has no OSM credentials.

**Key decisions (locked).**
- **AI assists, humans edit, community validates.** No headless/bot edits; AI output is always a
  human-reviewed candidate, instruction, or QA flag. This mirrors the Hee-Lee Oss donated-lane rule.
- **Candidate-list-first.** Detection produces reviewable candidates, never an upload — so the
  human-in-the-loop and OSM Import Guidelines are structurally enforced.
- **Local-first.** Prefer engaging the local OSM community for mapping/validation over remote-only;
  a pack with no local validation plan is not shipped.
- **Default to micro-tasking over import.** Prefer MapRoulette/Notes/TM (human-mapped, low-import-
  governance overhead) and only invoke the full Import process when a genuine bulk import is
  justified, partner-backed, and community-approved.

## Data, licensing & compliance

**This is the critical section.** OSM data is the **Open Database License (ODbL) v1.0** — a
share-alike open-data license. Two distinct licensing obligations apply and are both gated:

**(A) Imagery licensing (for tracing).** You may only trace from imagery OSM is *authorized* to use.
- **Allowed (with their terms/attribution):** Bing Aerial, Esri World Imagery (OSM agreement),
  Maxar Premium (via the OSM imagery agreement), Mapbox Satellite — each per its OSM-specific
  permission, with required attribution recorded in the pack.
- **Forbidden:** Google Maps/Earth/Street View and any imagery without an OSM tracing permission.
  Tracing from forbidden imagery is a copyright violation and gets edits reverted — **hard exclude.**
- Each pack pins the *exact* allowed imagery layer(s) + the OSM permission reference + attribution.

**(B) Source-data licensing (for conflation/import candidates).** Any external dataset we conflate
against OSM or propose as import candidates must be **ODbL-compatible** so its content can lawfully
enter OSM.
- **Accepted:** public domain / CC0; **ODbL** sources; datasets with an **explicit OSM-compatible
  waiver/permission** (the model HOT/OSM use for government data) — e.g., Microsoft Building
  Footprints (ODbL). Each requires a cited license clause/URL and `odblCompatible: true` recorded
  with evidence.
- **Case-by-case / default-exclude:** CC-BY (attribution-into-OSM is non-trivial — requires the
  community's accepted handling), and any dataset whose terms are unclear. Resolved by a written
  **import-acceptance policy** (task `import-playbook-014`) before such a source is used.
- **Forbidden:** any "all rights reserved," scraped, or non-redistributable source, and **anything
  derived from Google or other proprietary maps.** No proprietary-DB scraping, ever.

**Objective acceptance criterion.** A source PASSes only if (imagery) it is on the OSM-authorized
imagery list with attribution recorded, **and** (data) `odblCompatible: true` is set from a cited
clause/URL or a recorded OSM-compatible permission. Missing/unparseable evidence = FLAG/EXCLUDE,
never default-allow.

**Output license (ODbL share-alike).** Everything contributed to OSM is **ODbL-1.0** — that is
non-negotiable and inherited from OSM. Our **candidate-feature datasets / MapRoulette challenges
derived from OSM data are ODbL-1.0**. Project **code is MIT**; standalone **documentation/guides are
CC-BY-4.0** (when not a derivative of OSM data). Each artifact records its license explicitly so
share-alike obligations are never accidentally dropped.

**Provenance model.** Every pack records: requesting partner + use statement + local-consent
status; the exact imagery layer(s) + OSM permission ref + attribution; each source dataset's URL,
publisher, retrieval date, version, license id/URL + **snapshot** (committed copy + SHA-256 + Wayback
URL), and `odblCompatible` evidence; the OSMF Organised-Editing wiki page URL; and the resulting OSM
changeset / Tasking Manager project / MapRoulette challenge references.

**Privacy / PII stance (people are not features).** We map **public infrastructure**, never people.
Prohibited: occupant names, household-level attributes, individuals' locations, and any
religion/ethnicity/health/displacement status tied to identifiable people. Building footprints carry
**geometry + public function only**, no occupant data. We never add a living person's private
information. For any field-collected data offered by a partner, we require documented consent and
de-identification *by the partner* before use; we do not de-identify ourselves.

**Do-no-harm (the humanitarian-specific obligation).** Visibility is not always good. Mapping the
settlements, shelters, or movement of a persecuted or displaced group can expose them to harm.
Following HOT's data-ethics / "do no harm" practice, the do-no-harm gate (below) can **restrict the
feature set, lower geo-precision, or exclude an area entirely**, and escalates such contexts to
**high** risk requiring expert/ethics sign-off. This judgement always defaults to caution.

**Disputed territory & non-partisanship.** For contested boundaries we follow OSM's **on-the-ground
rule** and remain neutral; we do not take sides, and civic/governance features are mapped factually,
never partisanly.

## Quality, review & risk gates

**Risk tier: medium** (per the portfolio). **Escalates to high** for any pack touching conflict
zones, displaced/persecuted populations, or other sensitive contexts — those require expert/ethics
sign-off before mapping begins (per the good-deed definition's high-tier rule).

**Required review before a deed is "done":**
- **Licensing/imagery reviewer** (mandatory, every pack): confirms imagery is OSM-authorized and
  every conflated/import-candidate source is ODbL-compatible, with evidence recorded. Hard gate.
- **Do-no-harm reviewer** (mandatory, every pack): completes and signs the do-no-harm screen;
  decides feature-set restrictions or exclusion; escalates sensitive contexts to high + expert.
- **Experienced OSM validator** (mandatory before "delivered"): reviews the resulting edits
  (sampled) via Tasking Manager validation / changeset review / Osmose, confirming tagging
  correctness, geometry quality, and **no reverts for cause**. This is the "merged, validated"
  signal — production of a pack is *not* delivery.
- **Technical reviewer**: confirms tooling correctness (conflation/QA outputs, MapRoulette/Notes
  artifacts) and CI green, with golden fixtures.
- **Local-community sign-off** (required where a local community exists for the region): the
  community is informed and does not object; ideally they map/validate.

**Golden fixtures / CI (so "CI green" means something).** Each tool ships committed synthetic/public
fixtures exercised in CI: conflation tool — golden OSM-vs-source inputs → expected candidate list;
MapRoulette generator — golden pack → valid v2 challenge GeoJSON; Notes generator — golden input →
well-formed note text; validators assert against pinned schema versions. **No real protected data and
no OSM credentials in CI.**

**Definition of Shipped.** Features **mapped by humans, validated by an experienced OSM validator,
and merged into OSM (not reverted)** under ODbL — with imagery/source licensing verified, the
do-no-harm screen signed, the effort registered as Organised Editing, and the outcome (validated
count, pass rate) recorded by the Steward. **Ideally, plus a verified beneficiary use.** Producing a
pack is *not* shipped; community-validated, accepted edits are.

## Roadmap & milestones

**M0 — Foundation & cold-start (thin)**
- Goal: build the pack template + gates, secure the blocking reviewer roles, prove one pilot pack
  end-to-end to a *validated, merged* outcome, and open partner/community outreach.
- **Cold-start de-risking.** To avoid producing a pack nobody maps (or that harms), the pilot is
  gated, in priority order, on: (a) a **named partner or local community** who has agreed to map/
  validate a specific small area; failing that, (b) a **self-serve-validatable fallback** — a tiny,
  low-sensitivity area (e.g., a well-understood region with an active local community and an existing
  Missing Maps interest) where the Hee-Lee Oss maintainer can themselves map a sample and an independent
  experienced validator can confirm it. The pilot must reach a *validated, merged* outcome, not a
  "pack written, unused" one.
- Exit criteria: (1) pack template + canonical model + tagging schema published; (2) licensing/
  imagery gate and do-no-harm gate exist and are applied to one area; (3) blocking reviewer roles
  (licensing/imagery + do-no-harm + experienced validator) named; (4) one pilot pack mapped,
  validated, and merged into OSM under ODbL (or, if no channel materializes, fully prepared with the
  blocker surfaced); (5) ≥ 1 partner/community outreach thread opened; (6) the effort registered
  under OSMF Organised Editing.

**M1 — Gap-analysis toolchain & first validated edits**
- Goal: build the read-only conflation/QA tooling and get real, validated edits merged.
- Exit criteria: (1) conflation/gap-detection tool emits a reviewed candidate list from authorized
  sources, with golden fixtures + CI green; (2) MapRoulette challenge generator produces a valid v2
  challenge from a pack; (3) ≥ 2 packs mapped + validated + merged; (4) ≥ 1 confirmed partner and,
  where applicable, local-community sign-off; (5) ≥ 95% validation pass rate on sampled edits with 0
  license/imagery/PII reverts.

**M2 — Scale via task-pack catalog, QA & import governance**
- Goal: raise throughput and quality with QA tooling and a written import/mechanical-edit playbook.
- Exit criteria: (1) QA/validation tool (Osmose/iD-rule integration) flags likely errors for human
  fixing; (2) import & mechanical-edit governance playbook published (Import Guidelines + Automated
  Edits CoC + Organised Editing), decided before any bulk/mechanical contribution; (3) ≥ 5 packs
  delivered + validated cumulatively across ≥ 2 regions; (4) measurable reduction in validator-time
  per 100 features vs. the M0/M1 baseline, with no drop in pass rate.

**M3 — Outcomes, local handover & sustainability**
- Goal: demonstrate real beneficiary use, hand ownership to local communities, and set maintenance.
- Exit criteria: (1) ≥ 1 externally verifiable beneficiary/local **use event**; (2) ≥ 1 region's
  packs/validation handed to a local community with a validator trained; (3) documented staleness/
  re-map refresh process and a named steward for ongoing liaison; (4) ≥ 8 packs validated+merged
  cumulatively.

Dependencies: M1 tooling depends on the M0 model/gates; M2 scale depends on M1 tooling + a confirmed
partner; M3 outcomes depend on a body of merged, validated deliveries from M1–M2.

## Work breakdown

The itemized, schema-mapped backlog lives in `TASKS.md`, organized by the milestones above. Each
milestone has a task table (`ID | Title | Type | Size | Risk | Deliverable | Depends on |
Reviewer`), acceptance criteria for the most important tasks, and a Definition of Done. A
sized-but-unscheduled backlog and one complete, schema-valid example Task JSON are included there.
The deliverable enums map as: pack/guide/playbook → `document`; reviewed candidate-feature set /
MapRoulette challenge → `dataset` (ODbL); tooling → `pr`; translated mapping instructions →
`translation`. There is no "OSM changeset" deliverable in the schema — the actual edit is the
human/steward's last-mile action; our tasks deliver the packs, candidates, tooling, and validation
that enable and verify it.

## Governance, roles & stakeholders

- **Maintainer (Owner):** TBD — owns the template, toolkit, and pack backlog.
- **Licensing/imagery reviewer:** TBD (TO BE SECURED) — mandatory, **non-skippable** gate; must be
  filled **before the M0 pilot is mapped**. Can read OSM imagery permissions, ODbL compatibility,
  and SPDX/CC/ODbL terms. May rotate among ≥ 2 qualified reviewers but at least one must always
  exist or work halts.
- **Do-no-harm reviewer:** TBD (TO BE SECURED) — mandatory, non-skippable; completes the do-no-harm
  screen, decides restrictions/exclusion, and escalates sensitive contexts to high + expert/ethics
  review. Ideally HOT-experienced or with humanitarian-protection background.
- **Experienced OSM validator(s):** TBD — confirm edit quality before "delivered"; the "merged,
  validated" signal. Must hold real OSM validation experience (e.g., TM validator role).
- **Expert/ethics reviewer(s):** engaged for **high**-risk packs (conflict/vulnerable-population
  contexts) — humanitarian-protection or regional expert sign-off before mapping.
- **Local community liaison / Steward (last-mile owner):** TBD — owns the relationship with the
  local OSM community and the requesting partner, registers Organised Editing, and records the
  validated/merged + use outcomes. Critical because "delivered" = community-validated acceptance.
- **Partner / requestor:** TO BE SECURED — named HOT hub, Missing Maps partner, NGO, or local
  community with a concrete area + use.

## Dependencies & integrations

- **OSM ecosystem (read + human-write only):** OpenStreetMap (API v0.6, **read-only** from our
  tools), Overpass API, Tasking Manager, MapRoulette, OSM Notes, Osmose / OSM Inspector, iD / JOSM /
  RapiD editors (human-driven), OSM wiki (tagging + Organised Editing pages).
- **Authorized imagery providers:** Bing, Esri, Maxar (via OSM agreement), Mapbox — per their
  OSM-specific permissions.
- **OSM-compatible source datasets:** TO BE SELECTED per pack via the licensing gate — e.g., ODbL
  Microsoft Building Footprints, PD/CC0 government data, datasets with an OSM-compatible waiver. None
  assumed in scope yet.
- **Governance documents:** OSM Automated Edits CoC, Import Guidelines, OSMF Organised Editing
  Guidelines, OSM Foundation data-protection / DWG policies, HOT data-ethics / do-no-harm guidance.
- **Hee-Lee Oss pieces:** Task JSON schema (`packages/schema`), donated-lane CLI workspace/PR flow
  (`packages/cli`) for *our repo's* code/docs (not for OSM edits), good-deed definition + refusal
  guardrails. No funded-lane/runner dependency (donated lane).

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
| --- | --- | --- | --- | --- |
| AI-generated/bulk edits pushed into OSM → mass revert + account block + reputational harm to Hee-Lee Oss | Medium | High | Hard architectural rule: no OSM write credentials in any tool/CI; candidate-list-first; human-in-the-loop + validator gate; follow Import/Automated-Edit/Organised-Editing governance | Maintainer |
| Tracing from unauthorized imagery (e.g., Google) → copyright violation, reverts | Medium | High | Imagery gate pins only OSM-authorized layers per pack; reviewer sign-off; forbidden list enforced | Licensing/imagery reviewer |
| Conflating an OSM-incompatible source → license contamination of the commons | Medium | High | Source gate requires `odblCompatible:true` with cited evidence; default-exclude; import-acceptance policy | Licensing/imagery reviewer |
| **Doing harm** by mapping vulnerable people / sensitive features | Low | High | Mandatory do-no-harm screen; escalate sensitive contexts to high + expert/ethics; restrict/lower precision/exclude; default to caution | Do-no-harm reviewer |
| Low-quality remote "armchair" data wastes volunteer time / degrades OSM | Medium | Medium | Local-first; experienced-validator gate before "delivered"; QA tooling; pass-rate metric | Validator |
| No partner/local mandate → packs unused or unwanted ("not delivered"/harmful) | Medium | High | `verifiedNeed:false` until a named partner + use + local consent; M0 outreach; steward role | Steward |
| PII / people-level data enters the map | Low | High | People-are-not-features rule; building footprints carry no occupant data; partner de-identifies field data, not us | Do-no-harm reviewer |
| Disputed-boundary / partisan editing controversy | Low | Medium | On-the-ground rule; neutral civic mapping; exclude contested edits | Maintainer |
| OSM API / TM / MapRoulette / Osmose interface drift | Medium | Low | Pinned interface versions in `specVersions`; isolated adapters; version-bump task | Maintainer |
| Organised-editing non-compliance (DWG action) | Low | Medium | Register each effort per OSMF Organised Editing Guidelines before mapping; document on wiki | Steward |
| Scope creep into automated editing / non-infrastructure mapping | Medium | Medium | Explicit non-goals; reviewers reject; no write code path exists | Maintainer |

## Security & privacy

- **Threat surface is small** for our software (no runtime service, no data hosting, **no OSM write
  credentials**). Main surfaces: CI, the candidate/challenge files we emit, and the upstream edits
  humans make from them.
- **No write credentials, by construction.** The toolkit has no code path to the OSM write/upload
  API; CI holds no OSM secrets. This structurally prevents an accidental or malicious headless edit.
- **Secrets handling:** read-only API/Overpass/Osmose access needs no secret by default. If a human
  uses an OSM account to map/import, those credentials live only with that human's editor and are
  never written into our logs, receipts, or committed files (per Hee-Lee Oss rules).
- **PII / people-level data:** dominant concern is *upstream* — that a source dataset or field
  collection contains people-level data. Handled by the people-are-not-features rule, the do-no-harm
  screen, and partner-side de-identification + consent. We inspect candidate data only enough to map
  public infrastructure and exclude on any people-level signal.
- **Do-no-harm as a security control:** in sensitive regions, *publishing* correct data can be the
  threat. The do-no-harm gate can restrict feature sets, reduce geo-precision, or exclude an area.
- **Abuse/misuse prevention:** refuse and flag any task steering mapping toward surveillance,
  targeting a group, de-anonymization, partisan boundary edits, or laundering proprietary/Google-
  derived data into OSM. Per CLAUDE.md, when in doubt, stop and surface the concern.

## Sustainability & maintenance

- **Local ownership is the exit.** The durable outcome is a local OSM community that owns its map.
  M3 hands packs/validation to local mappers and trains a validator, so the work continues without
  Hee-Lee Oss. Remote contribution is scaffolding, not the destination.
- **Maintenance:** OSM data drifts and decays; the staleness/re-map process flags areas needing
  refresh (e.g., post-disaster change), tracked as `type: maintenance` tasks. Tooling tracks pinned
  interface versions and is bumped via deliberate tasks.
- **Outcome tracking:** the steward records validated-feature counts, pass rate, reverts, and
  verified beneficiary use against the success metrics, reviewed each milestone. Reverts for cause
  trigger a root-cause review and a gate/tooling fix.

## Open questions

- Which HOT hub / Missing Maps partner / local community will be the first confirmed partner, and
  for which specific area and use?
- How do we handle **CC-BY** source data given OSM's attribution-into-the-database constraints —
  blanket exclude, or per-source via the import-acceptance policy? (Default: exclude/escalate.)
- For genuine bulk imports, what is the threshold at which we invoke the full Import Guidelines
  process vs. staying with human MapRoulette/TM micro-tasking? (Default: prefer micro-tasking.)
- What is the canonical do-no-harm rubric and who is qualified to sign it for a given region (esp.
  conflict/displacement contexts)?
- ~~Where is the license/imagery-permission snapshot stored?~~ **Decided (must precede the gate
  task):** committed local copy of the permission/license page + SHA-256 + Wayback URL, referenced
  by `snapshotRef`; a bare URL is insufficient.
- What counts as a sufficiently "verifiable beneficiary use event" for the outcome metric?
- RapiD/AI-suggestion editors: do we endorse AI-assisted *human* tracing in instructions, and with
  what confirmation discipline, to keep within community norms?

## References

- Hee-Lee Oss work rules — `C:\code\hee-lee-oss\CLAUDE.md`
- Good Deed Definition + risk tiers — `C:\code\hee-lee-oss\docs\good-deed-definition.md`
- Task JSON schema — `C:\code\hee-lee-oss\packages\schema\src\schemas.ts`
- Portfolio roadmap — `C:\code\hee-lee-oss\planning\ROADMAP.md`
- Proposal — TO BE WRITTEN (`C:\code\hee-lee-oss\governance\proposals\open-map-gaps.md`)
- OpenStreetMap & ODbL 1.0; OSM Copyright/License page
- OSM Automated Edits code of conduct; Import / Import Guidelines; OSMF Organised Editing Guidelines
- OSM imagery permissions (Bing, Esri, Maxar via OSM agreement, Mapbox); OSM "on-the-ground" rule
- Humanitarian OpenStreetMap Team (HOT), Missing Maps, HOT data-ethics / do-no-harm guidance
- Tasking Manager; MapRoulette (v2 challenge schema); OSM Notes; Osmose / OSM Inspector
- Microsoft Building Footprints (ODbL); Overpass API; OSM API v0.6

---

## Appendix A — Improvements applied

The following 25 specific improvements were identified during drafting and have each been **applied**
to the plan/tasks above (and to `TASKS.md`). They are listed so the reasoning is auditable.

1. **No-write-credentials made architectural, not policy.** Added the "Read-only OSM access protocol"
   and a Security control stating no code path or CI secret can write to the OSM API — turning "AI
   never edits" from a promise into a structural guarantee.
2. **Candidate-list-first pattern** named as a locked decision so the human-in-the-loop and Import
   Guidelines are enforced by the data flow, not by reviewer vigilance alone.
3. **Two-axis licensing gate** split into (A) imagery and (B) source-data, because they are different
   legal obligations and conflating them is a common, costly mistake.
4. **Explicit forbidden-imagery list** (Google et al.) with the "hard exclude" consequence, since
   tracing from Google is the single most common cause of OSM reverts/blocks.
5. **ODbL share-alike propagated to our derived datasets** (candidate sets / MapRoulette challenges),
   not just to the upstream edits — preventing accidental re-licensing of OSM-derived content.
6. **Do-no-harm elevated to a blocking gate with its own reviewer role** and the power to restrict/
   exclude, reflecting HOT's humanitarian-protection practice rather than treating mapping as always-good.
7. **High-risk escalation rule** for conflict/vulnerable-population contexts, wiring the good-deed
   definition's high-tier expert-sign-off requirement into the gate.
8. **"People are not features" rule** added to scope, privacy, and risks — building footprints carry
   geometry + public function only, never occupant data.
9. **Organised Editing registration** (OSMF guideline) added as an exit criterion and steward duty,
   since a coordinated Hee-Lee Oss mapping campaign legally *is* organised editing.
10. **Local-first / local-community sign-off** made a gate and a sustainability exit, directly
    countering the well-known "armchair remote mapping" quality and legitimacy problem.
11. **Validator gate distinguishes "produced" from "delivered."** "Shipped" requires an experienced
    OSM validator to confirm merged, non-reverted edits — matching "delivered, not merged."
12. **Outcome metrics rewritten to count only validated, merged, non-reverted features**, with
    self-reported impact explicitly excluded and "0 reverts for cause" as a hard target.
13. **Verified-need honesty:** `verifiedNeed:false` until a named partner confirms area + use + local
    consent, with the rationale that remote mapping without a mandate can be worthless or harmful.
14. **Default-to-micro-tasking decision** (MapRoulette/Notes/TM over full imports) to minimize import-
    governance overhead and AI-bulk-edit risk while staying squarely within community norms.
15. **Import-acceptance policy task** added (M2) so CC-BY / share-alike / ambiguous sources are decided
    by a written rule before use, not ad hoc.
16. **License/permission snapshot format decided up front** (committed copy + SHA-256 + Wayback),
    resolving the open question before the gate task is built.
17. **Pinned external interface versions** (OSM API v0.6, Overpass QL, MapRoulette v2, TM fields,
    Osmose) recorded in `specVersions`, with drift handled by an isolated version-bump task.
18. **Golden fixtures + CI with no real protected data and no OSM creds**, so "CI green" is meaningful
    and safe for geospatial tooling.
19. **Disputed-territory / on-the-ground neutrality** added to satisfy the non-partisan civic guardrail
    for boundary and governance features.
20. **Reviewer roles made blocking and time-ordered** (must be filled before the pilot is mapped),
    mirroring the open-data-datasheets hard-gate pattern, with a ≥2-reviewer anti-bottleneck rule.
21. **Beneficiary use event** added as a top-line outcome (a partner actually using the completed map),
    pushing the project past "edits made" to real-world value.
22. **AI-assisted-human-tracing stance** (RapiD-style suggestions, human-confirmed) addressed in
    decisions + open questions, so we use AI's strength without crossing the bot-edit line.
23. **Deliverable-enum mapping spelled out** (pack→document, candidate set/challenge→dataset, tooling→
    pr, instructions→translation) since the schema has no "changeset" deliverable.
24. **Validator-time efficiency metric** added (with a no-pass-rate-drop guard) to make the "AI reduces
    drudgery" claim measurable without incentivizing speed over quality.
25. **Maintenance/re-map process** for OSM data decay (esp. post-disaster) added as a sustainability
    duty, with reverts-for-cause triggering root-cause + gate/tooling fixes.

## Review sign-off

**Reviewer:** drafting engineer (self-review pass) · **Date:** 2026-06-28 · **Scope:** completeness
against PLAN_SPEC's 17 sections + correctness against CLAUDE.md, the good-deed definition, the Task
schema, and OSM community norms.

**Completeness.** All 17 required H2 sections are present and in order. `TASKS.md` provides the
schema-mapped backlog, per-milestone tables aligned to the roadmap (M0–M3), acceptance criteria for
the key tasks, milestone DoDs, a future backlog, and one complete schema-valid example Task JSON.

**Correctness checks performed and resolved.**
- *Schema:* the example Task JSON includes every `required` field (id, title, project, type, lane,
  priority, domain, riskTier, urgent, deliverable, tokenEstimate, status, context, objective,
  acceptanceCriteria, output, verifiedNeed) with enum-valid values; `outputLicense` set; no
  `additionalProperties`; donated lane so no `fundedBudgetUsd` needed (funded conditional N/A).
- *Deliverable enums:* every task uses only `pr | dataset | document | translation`; ODbL applied to
  `dataset` outputs derived from OSM; no invented "changeset" deliverable.
- *Guardrails:* license/provenance (imagery + ODbL-compatibility gates, snapshotting, no proprietary/
  Google scraping); privacy/PII (people-are-not-features, partner-side de-identification); do-no-harm
  + high-risk expert escalation; non-partisan/on-the-ground rule — all present and cross-referenced.
- *Hee-Lee Oss rules:* no headless/automated edits (mirrors the donated-lane CLI rule); agent-neutral; no
  secrets in logs/CI; "delivered, not merged" encoded as the validator gate; honest `verifiedNeed`.
- *Honesty:* partner, requestor, and reviewer roles marked **TO BE SECURED**; `verifiedNeed:false`
  across the backlog until a partner is confirmed.

**Residual items for a human decision** (also in Open questions): naming the first partner/region;
the CC-BY source policy; the do-no-harm rubric/qualified signer for sensitive regions; the
import-vs-micro-tasking threshold. None block M0, which is foundation + gates + a single validated
pilot. **No correctness defects outstanding at sign-off.**
