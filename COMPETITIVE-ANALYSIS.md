# Competitive & Improvement Analysis — open-map-gaps

Project: identify and help fill gaps in OpenStreetMap (missing roads/buildings/amenities),
especially for underserved/humanitarian-mapping areas, producing prioritized task packs + tooling.
Lane: donated. Risk tier: medium (escalates to high for conflict/vulnerable contexts).
Reviewed artifacts: `PLAN.md` v0.1.0, `TASKS.md` v0.1.0.

All competitor claims below are grounded in cited web sources (URLs inline). Research conducted
2026-06-28/29.

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually strong on community-norms compliance — in most respects it is *more*
conservative than it strictly needs to be, which is the right posture for a newcomer entering a
community that reverts and blocks outsiders. Findings, roughly in order of importance:

**1.1 — The "AI never edits / candidate-list-first / no write credentials" stance is correct and
is the single most important design decision.** OSM's own governance is explicit that "imports and
automated edits should only be carried out by those with experience... and only with careful
planning and consultation with the local community. Imports/automated edits which do not follow
these guidelines might be reverted"
([Automated Edits CoC](https://wiki.openstreetmap.org/wiki/Automated_Edits_code_of_conduct);
[Import/Guidelines](https://wiki.openstreetmap.org/wiki/Import/Guidelines)). Undiscussed mechanical
edits "may be reverted without any discussion." The plan's structural guarantee (no OSM write
credential exists in any code path or CI; detection only ever emits a reviewed candidate list) is
the correct way to make "AI assists, humans edit" enforceable rather than aspirational. **This is
correct and should be defended as non-negotiable.**

**1.2 — Correctly internalizes the armchair-mapping / quality-degradation risk, but should sharpen
one nuance.** OSM documents that armchair mapping "favours quantity and speed over quality" and
that a "sudden rapid arrival" of low-quality data actively *discourages* better survey-based mapping
and makes an area "feel like a dumping ground"
([Armchair mapping](https://wiki.openstreetmap.org/wiki/Armchair_mapping);
[Import/Past Problems](https://wiki.openstreetmap.org/wiki/Import/Past_Problems)). The plan's
local-first gate and validator gate address this. **Gap to sharpen:** the plan treats "feature
mapped + validated + merged" as success, but OSM's own literature warns that even *correct* bulk
candidate data can crowd out local surveying. The do-no-harm and local-sign-off gates should
explicitly include a "community-saturation / displacement-of-local-effort" check, not only a
people-safety check. Right now that risk lives only implicitly in "local-first."

**1.3 — Two-axis licensing gate (imagery vs. source data) is correct and reflects a real, costly
distinction.** OSM separates (A) imagery tracing permission from (B) database-license compatibility;
both can independently cause reverts. The plan's forbidden-imagery hard-exclude (Google et al.) is
correct — tracing from Google is the canonical revert/block trigger. One subtlety the plan gets
right and should keep loud: license *laundering* risk. OSM warns that data "available under a
compatible license may ultimately be derived from... Google Maps... and therefore not actually
available under the stated license"
([Import/Past Problems](https://wiki.openstreetmap.org/wiki/Import/Past_Problems)). The plan's
provenance/snapshot requirement covers this; keep CC-BY default-exclude.

**1.4 — ODbL share-alike propagation to derived candidate datasets is correct.** Candidate sets and
MapRoulette challenges derived from OSM data inherit ODbL-1.0; the plan states this. Good. One
completeness gap: the plan should state how it handles *mixing* an ODbL-compatible external source
(e.g., Microsoft Footprints, itself ODbL) with OSM-derived geometry in a single candidate file —
the combined artifact is cleanly ODbL, but provenance per-feature should be retained so a human can
later attribute the import correctly.

**1.5 — Organised Editing registration is correctly required.** A coordinated Elyos campaign legally
*is* organised editing under the OSMF guideline; registering a wiki activity page before mapping is
the right move and mirrors how HOT and Meta register their activities
([Organised Editing/Activities](https://wiki.openstreetmap.org/wiki/Organised_Editing/Activities/Humanitarian_OpenStreetMap_Team)).

**1.6 — Ground-truth limits of remote mapping are acknowledged but under-operationalized.** The plan
correctly says remote mapping without local mandate "can be worthless or actively harmful," and
prefers local validation. But OSM notes some features are simply *not surveyable from imagery*
("you can't go look for the parcel lines on the ground"). The tagging/validation schema should
encode a **"remotely-traceable vs. requires-ground-survey"** distinction per feature class — e.g.,
building *footprints* are imagery-traceable, but a building's *function* (school/clinic) generally is
not and must come from an authorized source or local survey. The plan gestures at this ("footprints
carry geometry + public function only") but does not make the trace-vs-survey boundary an explicit,
gated field. Recommend adding it to the canonical pack model.

**1.7 — Privacy / "people are not features" is correct and well-placed.** Refusing occupant/
household/individual data and deferring de-identification to the partner is the right division of
responsibility. Do-no-harm gate's power to restrict feature set, lower geo-precision, or exclude is
consistent with HOT data-ethics practice. No defect; keep the high-tier expert escalation.

**1.8 — Minor completeness gaps:** (a) No explicit treatment of **changeset hygiene** (changeset
comments, `source=` and `hashtag=` tags, source imagery tagging) which is what makes an organised
edit auditable and revert-friendly — should be in the mapping instructions template. (b) No mention
of **OSM's data-protection/DWG channels** as the escalation path if a revert *does* happen — the
plan should pre-stage a "what we do if reverted" runbook. (c) The success metric "0 reverts for
cause" is good but a single bad actor on a public TM project could breach it through no fault of the
pack; metric should scope to *edits attributable to our pack/instructions*.

Overall: **no correctness defects that would cause community harm; the gating philosophy is right.**
The improvements are about operationalizing nuances (trace-vs-survey, saturation, changeset hygiene,
revert runbook).

---

## 2. Competitive landscape

This is a crowded, mature ecosystem. Critically, **the closest competitor is HOT itself**, which now
ships its own AI tool. open-map-gaps must position as a *scoping/governance/QA layer that feeds*
these tools, not a rival detector.

**Humanitarian OpenStreetMap Team (HOT) + Tasking Manager.**
- *What it is:* The dominant coordination platform; divides a region into small "tasks" mapped
  remotely and then validated by experienced mappers
  ([tasks.hotosm.org](https://tasks.hotosm.org/);
  [HOT TM](https://www.hotosm.org/tech-suite/tasking-manager/)).
- *Strengths:* De-facto standard, huge volunteer base, built-in validation workflow, trusted by NGOs.
- *Weaknesses:* TM assumes someone has *already scoped* the project (AoI, instructions, imagery,
  priority); scoping quality is uneven, and "armchair" quality problems are well documented. This
  scoping/QA gap is exactly open-map-gaps's target.

**HOT fAIr (AI-assisted mapping) — the most direct competitor.**
- *What it is:* HOT's own open-source AI service (production-released 31 May 2024) that detects
  buildings/roads from satellite/UAV imagery and lets *local* communities train their own models
  ([fair.hotosm.org](https://fair.hotosm.org/);
  [HOT tech blog](https://www.hotosm.org/tech-blog/hot-tech-talks-fair/)). Pilot roughly *doubled*
  mapping throughput to ~2,500–3,000 buildings/day/mapper.
- *Strengths:* Same humanitarian mission, human-in-the-loop by design, local-model training, backed
  by the incumbent, already integrated into the HOT workflow.
- *Weaknesses:* It is a *detection/assist* tool inside the editor, not a licensing/do-no-harm/
  task-scoping governance layer; it does not produce the cross-source conflated, license-gated,
  ethics-screened **task pack**. open-map-gaps is complementary — it can *feed* fAIr-enabled mappers.
- *Implication:* Do **not** compete on detection. Position as the upstream scoping + downstream QA
  wrapper. Partnering with HOT (a named candidate partner already) beats competing.

**Missing Maps.**
- *What it is:* Coalition (HOT, MSF, Red Cross) that runs mapathons on the TM
  ([missingmaps.org](https://missingmaps.org/hot-tasking-manager/)).
- *Strengths:* Brand, volunteer pipeline, real humanitarian demand signal.
- *Weaknesses:* Same scoping/quality gap; relies on whoever sets up the project.

**MapRoulette.**
- *What it is:* Microtasking platform; "snack-sized challenges" sourced from Overpass queries or
  GeoJSON; users fix one issue at a time; "Quick Fix" can apply a suggested change to OSM directly
  ([wiki](https://wiki.openstreetmap.org/wiki/MapRoulette);
  [OSM US](https://openstreetmap.us/our-work/maproulette/)).
- *Strengths:* Perfect human-in-the-loop delivery channel for candidate gaps; open challenge API
  (the plan already targets v2 challenge GeoJSON as an output — correct choice).
- *Weaknesses:* It's a *delivery surface*, not a gap-detector or scoper. open-map-gaps generating
  well-formed, license-clean challenges is a genuine value-add; note "Quick Fix" can write to OSM, so
  challenge design must keep humans deciding (aligns with plan).

**OpenStreetMap itself (+ Overpass/Osmose).**
- *Strengths:* The commons; Osmose/OSM Inspector already flag many errors for free.
- *Weaknesses:* No built-in "where is the map *missing*" prioritization across external open data;
  that completeness-gap analysis is the open space.

**Mapillary (Meta-owned).**
- *What it is:* Crowd street-level imagery + computer-vision object detection (>191M map features),
  integrated into iD/RapiD/JOSM ([mapillary.com/osm](https://www.mapillary.com/osm);
  [wiki](https://wiki.openstreetmap.org/wiki/Mapillary)).
- *Strengths:* Ground-truth-ish imagery for verifying amenity attributes remote tracing can't.
- *Weaknesses:* Coverage sparse in exactly the underserved rural/crisis areas this project targets;
  Meta-owned (licensing/attribution care needed).

**Meta / Daylight Map Distribution — note it is SUNSET.**
- *What it was:* Validated OSM snapshot + ML roads, 2020–Nov 2024; **discontinued**, Meta migrated
  to Overture ([daylightmap.org sunset](https://daylightmap.org/2024/05/03/sunsetting-daylight.html);
  [registry](https://registry.opendata.aws/daylight-osm/)).
- *Implication:* A validated-snapshot niche just *opened up*; relevant to differentiation.

**Microsoft Building Footprints.**
- *What it is:* ~1.2B+ AI building polygons, **ODbL-licensed**, importable via RapiD/JOSM mapwithai
  plugin ([wiki](https://wiki.openstreetmap.org/wiki/Microsoft_Building_Footprint_Data);
  [GlobalMLBuildingFootprints](https://github.com/microsoft/GlobalMLBuildingFootprints/)).
- *Strengths:* Free, ODbL-compatible (clean conflation input — the plan correctly names this),
  good rural recall.
- *Weaknesses:* Raw footprints; quality variable in dense urban areas; no function/attribute data;
  needs conflation + human review before OSM entry. **This is a primary *input*, not a rival.**

**Overture Maps Foundation.**
- *What it is:* Linux Foundation project (Meta/Microsoft/AWS/TomTom); GA July 2024; 2.3B building
  footprints, ~54M POIs, divisions, base layers
  ([TechCrunch](https://techcrunch.com/2024/07/24/backed-by-microsoft-aws-and-meta-the-overture-maps-foundation-launches-first-open-map-datasets/);
  [Wikipedia](https://en.wikipedia.org/wiki/Overture_Maps_Foundation)).
- *Strengths:* Massive, well-funded, interoperable schema (GERS IDs); a major open-data source.
- *Weaknesses:* Mixed licensing (some layers CDLA/proprietary-derived, *not* uniformly ODbL) — so
  Overture is **not blanket-safe as an OSM import source**; license gate must check per-layer. It is
  also a *map product*, not a humanitarian-gap-filling workflow, and serves corporate consumers.

**Ohsome / HeiGIT analytics + OSMnx.**
- *What it is:* ohsome API/dashboard/ohsomeHeX measure OSM completeness, currency, and quality
  globally and near-real-time using full OSM history; ISO 19113-based quality metrics
  ([HeiGIT](https://heigit.org/openstreetmap-quality/);
  [ohsome](https://heigit.org/big-spatial-data-analytics-en/ohsome/)). OSMnx analyzes street networks.
- *Strengths:* Best-in-class *measurement* of where/how complete OSM is — directly usable as a
  gap-prioritization input.
- *Weaknesses:* Analytics only; they tell you *where* it's incomplete, not produce a scoped,
  license-gated, ethics-screened *task pack* to fix it. open-map-gaps can sit on top of ohsome.

**RapiD / MapWithAI (Meta).**
- *What it is:* AI-supercharged iD fork; surfaces Meta ML roads + Microsoft buildings for
  human-confirmed one-click adding; used in disaster response (e.g., Kerala 2018, 21,500 roads)
  ([github.com/facebook/Rapid](https://github.com/facebook/Rapid);
  [wiki](https://wiki.openstreetmap.org/wiki/RapiD)).
- *Strengths:* The canonical human-confirmed AI-tracing editor; exactly the endorsed last-mile tool.
- *Weaknesses:* An *editor*, not a scoper/QA/governance layer. The plan correctly names RapiD as a
  human-driven delivery tool, not a thing to rebuild.

**Landscape takeaway:** every major player is either (a) a *detection* engine (fAIr, Microsoft,
RapiD/MapWithAI, Mapillary), (b) a *delivery surface* (TM, MapRoulette), (c) a *data source*
(Overture, MS Footprints, Daylight[sunset]), or (d) an *analytics* layer (ohsome, OSMnx). **No one
owns the glue: a license-gated, do-no-harm-screened, partner-validated, multi-source *task-pack
assembly + QA* layer.** That is open-map-gaps's lane.

---

## 3. Gaps we can fill

1. **The scoping/assembly gap.** TM/MapRoulette assume a well-scoped project already exists. We
   produce the AoI + tagging schema + imagery allowlist + instructions + validation rules as a
   single reusable, auditable artifact.
2. **The license-gating gap.** No existing tool *forces* a per-pack, two-axis (imagery + data)
   ODbL/permission check with committed snapshot evidence before mapping. We make it a blocking gate.
3. **The do-no-harm gap.** Detection tools (fAIr, MS, RapiD) have no built-in protection screen for
   vulnerable populations; we add a signed, blocking ethics gate with high-tier escalation.
4. **The cross-source conflation gap.** ohsome tells you completeness; MS/Overture give footprints;
   nobody assembles "OSM vs. *authorized* open data → reviewed candidate gap list with provenance."
5. **The prioritization gap.** Turning ohsome-style completeness metrics + partner demand into a
   *prioritized, verified-need* task list (not just "everything is incomplete").
6. **The validated-snapshot gap (newly opened).** Daylight's sunset (Nov 2024) leaves a niche for
   curated, validated, license-clean derived subsets for a beneficiary's specific use.
7. **The "remote-trace vs. ground-survey" honesty gap.** Encoding which attributes can be remotely
   traced vs. which require local survey — a quality discipline no detector enforces.
8. **The provenance/auditability gap.** A machine-readable pack model recording requestor, use,
   consent, license evidence, snapshots, organised-editing URL, and outcome — auditable end-to-end.

---

## 4. Differentiators to win

1. **Governance-as-a-product.** The defensible moat is not detection (the incumbents win that) but
   the *trust layer*: a license-gated, ethics-screened, provenance-complete, organised-editing-
   registered task pack that a HOT hub or local community can adopt with confidence. Sell trust and
   auditability, not pixels.
2. **Complement, don't compete — feed the incumbents.** Position packs/candidates as inputs to TM,
   MapRoulette, RapiD, and fAIr. Partnering with HOT (already a named candidate partner) converts the
   closest competitor into the primary distribution channel.
3. **Outcome-not-output accounting.** "Validated + merged + not reverted + actually used by a
   beneficiary," with self-reported impact excluded, is a sharper success bar than any competitor's
   "buildings added" vanity metric — and directly answers OSM's armchair critique.
4. **Local-first as legitimacy.** A pack that ships only with a local validation plan answers the
   single loudest community objection to outside mappers; this is both ethical and strategic.
5. **The reusable, agent-neutral gap-analysis toolkit + MCP server** (see §7) — turning the scoping
   discipline into infrastructure others reuse, which is how you become the standard.

---

## 5. Claude API leverage — and the hard boundaries

**Where Claude adds clear value:**
1. **Gap detection / triage from open data (assistive).** Use Claude to *reason over and explain*
   conflation outputs (OSM vs. ohsome completeness vs. MS/Overture footprints) — clustering likely
   gaps, drafting human-readable rationales ("this 2km² has 0 mapped buildings but 400 MS footprints
   + a clinic in the partner's list"), and prioritizing by partner-stated need. Claude ranks and
   narrates; the geometry comes from deterministic tools.
2. **Task-pack & instruction generation.** Draft HOT-style mapping guides, tagging-schema docs,
   validator briefs, changeset-comment templates, and MapRoulette challenge descriptions from the
   canonical pack model — the highest-leverage, lowest-risk use (pure documentation).
3. **QA assist.** Summarize/triage Osmose/iD validation flags, draft do-no-harm screening checklists,
   and surface "this candidate looks like it may be Google-derived / may be a person's home / may need
   ground survey" *flags for a human* — accelerating reviewers without deciding.
4. **Translation.** Localize mapping instructions for local mapper communities (a `translation`
   deliverable in the schema).
5. **Partner/outreach drafting & research synthesis.** Draft outreach, summarize a region's existing
   OSM activity, surface the active local community to coordinate with.

**Where Claude must NOT decide (hard lines, consistent with PLAN.md and CLAUDE.md):**
- **No automated/headless edits to OSM, ever.** Claude output is always a reviewed candidate,
  instruction, or flag — never an upload. There must be **no OSM write code path / credential**.
- **No bulk import without the full community process.** Any genuine import follows Import Guidelines
  + Automated Edits CoC + Organised Editing — a human-run, community-consulted process, not a model
  decision.
- **Ground-truth and privacy are human-verified.** Claude may *flag* a possible vulnerable-population
  or non-surveyable feature, but the do-no-harm decision and any field/consent judgment are made and
  signed by the human reviewer/partner. People are never features.
- **License/attribution are human-verified.** Claude may *summarize* a license clause, but
  `odblCompatible: true` and imagery-permission PASS must be confirmed by the licensing reviewer with
  cited evidence + snapshot. Model "looks compatible" is never sufficient.
- **No fabricated features.** Claude must never invent geometry/attributes "to fill the blank." Every
  candidate traces to an authorized source or imagery a human can verify; unverifiable = excluded.
- **No partisan/disputed-boundary calls.** Follow OSM's on-the-ground rule; Claude does not adjudicate.

---

## 6. Ten concrete optimizations

1. **Add a `surveyability` field per feature class** in the canonical pack model
   (`remote-traceable | requires-ground-survey | source-attested`) so packs never ask remote mappers
   to invent attributes they can't verify (addresses §1.6).
2. **Build the conflation tool directly on ohsome + Microsoft/Overture**, not bespoke detection —
   ohsome gives completeness baselines, MS Footprints (ODbL) gives clean candidate geometry. Reuse,
   don't rebuild detection.
3. **Ship a per-layer Overture/Source license matrix** in the imagery-gate task — Overture layers are
   *not* uniformly ODbL; encode per-layer compatibility so the gate can't wave it through.
4. **Add a "community-saturation" check to the do-no-harm/local-sign-off gate** — verify the pack
   won't dump candidates into an area with active local surveyors, per OSM's armchair-mapping warning.
5. **Pre-stage a "if reverted" runbook + DWG contact path** as an M0 doc, and add changeset-hygiene
   requirements (changeset comment, `source=`, hashtag, imagery tag) to the instruction template.
6. **Make HOT a first-call partner and design packs as fAIr/TM/RapiD inputs** — explicitly format
   candidate outputs to drop into fAIr-enabled or RapiD workflows; turn the competitor into a channel.
7. **Scope the "0 reverts for cause" metric to pack-attributable edits** (changeset hashtag), so a
   third-party mapper on a public TM project can't falsely fail the metric.
8. **Provide a small "validated-subset export" capability** to fill the Daylight gap — a license-clean,
   validated ODbL slice for a partner's specific decision/use.
9. **Add a reproducible-provenance bundle per pack** (license snapshot SHA-256 + Wayback, source
   version, retrieval date, conflation tool version) so any reviewer can re-derive the candidate list.
10. **Build a golden-fixture corpus from real public OSM/Osmose/MS samples** so CI exercises real
    edge cases (dense-urban MS-footprint noise, license-ambiguous source) — not just synthetic happy
    paths — making "CI green" actually meaningful for quality.

---

## 7. Parallel & perpendicular spin-offs

- **Reusable gap-analysis toolkit (perpendicular, high value).** The conflation/QA/license-gate/
  provenance engine is domain-agnostic: "compare an authoritative open dataset against a community
  dataset, find gaps, emit a license-gated, reviewed candidate list." Extract it as a standalone
  Elyos package usable beyond OSM.
- **MCP server (`gap-analysis-mcp`).** Expose read-only Overpass/ohsome/Osmose queries + conflation +
  license-check as MCP tools so *any* Claude agent (or other Elyos project) can ask "where is X
  under-mapped and what's the authorized source to fix it" — without ever holding write credentials.
  This is the agent-neutral-core-friendly way to ship the capability.
- **Tie to `citizen-science-pipelines`.** Citizen-collected field observations (consented,
  de-identified) become the *ground-truth/local-survey* input that closes open-map-gaps's surveyability
  gap (§1.6) — and conversely, gap analysis tells citizen-science projects where field collection is
  most needed. Natural two-way feed.
- **Tie to `community-resource-maps`.** Validated public-infrastructure layers (clinics, water points,
  markets, schools) produced here are exactly the substrate community-resource-maps would render for a
  beneficiary audience — share the pack model and the do-no-harm screen.
- **Tie to `urban-tree-inventory`.** Tree/green-feature mapping reuses the same conflation +
  candidate-list + human-validation discipline; an urban-tree pack is a specialization of the generic
  pack model, and could share the MCP server's detection-vs-survey logic.
- **Shared "do-no-harm + license gate" library.** The two blocking gates are reusable governance
  components for *any* Elyos open-data project that touches people or licensed sources — promote them
  to shared infrastructure.

---

## 8. Open questions

1. **HOT relationship: partner or compete?** The analysis strongly favors partnering (fAIr is the
   closest competitor and the obvious channel). Is HOT willing to consume external task packs, and at
   what governance bar? This determines distribution.
2. **Which first region/partner/use** (the plan's own top open question) — and does a local community
   already exist there so the local-first gate can pass?
3. **Overture as a source:** which specific Overture layers are ODbL-importable vs. excluded, and who
   maintains that matrix as Overture's licensing evolves?
4. **CC-BY policy** (plan's open question): blanket-exclude vs. per-source via import-acceptance
   policy — needs the licensing reviewer's written rule before any CC-BY source is touched.
5. **Do-no-harm rubric ownership:** who is qualified to sign it for a given conflict/displacement
   context, and what is the canonical rubric? (Plan flags this; it's the highest-stakes unfilled role.)
6. **Verifiable "beneficiary use event":** what evidence counts (a partner's written decision record?
   a TM project completion tied to a real response?) — needed to make the headline outcome metric real.
7. **Saturation threshold:** at what level of existing local mapping activity does a pack become a
   net *harm* (crowding out surveyors) rather than a help, and how is that measured (ohsome activity)?
8. **MCP/toolkit scope:** is the reusable gap-analysis toolkit in-scope for this project's milestones,
   or a separate Elyos deliverable? (Affects M1/M2 sizing.)

---

### Sources (key)
- OSM Automated Edits CoC / Import Guidelines / Past Problems / Armchair mapping (wiki.openstreetmap.org)
- HOT Tasking Manager (tasks.hotosm.org, hotosm.org); Missing Maps (missingmaps.org)
- HOT fAIr (fair.hotosm.org, hotosm.org tech blog, State of the Map EU 2024)
- MapRoulette (wiki.openstreetmap.org, openstreetmap.us)
- RapiD / MapWithAI (github.com/facebook/Rapid, wiki); Facebook AI-Assisted Road Tracing (wiki)
- Microsoft Building Footprints (wiki, github.com/microsoft/GlobalMLBuildingFootprints)
- Overture Maps Foundation (techcrunch.com 2024-07-24, en.wikipedia.org)
- Meta Daylight Map Distribution + sunset (daylightmap.org, registry.opendata.aws)
- Mapillary (mapillary.com/osm, wiki)
- ohsome / HeiGIT OSM quality (heigit.org)
