# AISA 11.1 — MERGE LOG

# Full version history: BCW BOT  → ONT_SEARCH_BOT v11.1 → v12 → AISA 11.1 (internal build v84.24)

# Author: Oleksii Kartashov

# Period: March 9 – April 2026

# ================================================================================

## PRE-PHASE — BCW_BOT v1.0

Date: 2026-03-09 to 2026-03-10

ORIGIN PROJECT: BCW BOT — Behaviour Change Wheel design assistant  
Architecture: Markdown-file-based bot, Code Interpreter mandatory  
Instruction layer: PROMPT_BCW.md (System Prompt v1.0), CONTROLLER_BCW.md (absolute authority)  
Mode machine: single mode `bcw [input]` + `0` return to menu  
Knowledge layer: BCW_MAIN.md, RXT_LIB.md, LANGUAGE_BCW.md, WELCOME_BCW.md  
Data sources: bcio_index.json, bcio_clean.json, bcio_by_module.json, term_combo_index.json,
bcio_hierarchy_index.json, bcio_modules.json  
File count: 6 Markdown files + 5 JSON data files  
CI dependency: mandatory — hard stop if unavailable, no fallback

Core pipeline: 8-step Behaviour Change Wheel design process:

- Step 1: Define problem in behavioural terms (COM-B + TDF decomposition, BCIO mapping)
- Step 2: Select target behaviour (Behaviour module, priority weighting)
- Step 3: Specify target behaviour (5W × BCIO modules: Setting, Population, Source)
- Step 4: Identify what needs to change → COM-B deficit analysis (MoA module, APEASE soft check)
- Step 5: Identify intervention functions (BCT, Style of Delivery, APEASE official)
- Step 6: Identify policy categories (Mode of Delivery, Source modules)
- Step 7: Identify BCTs (BCT + MoA + Style of Delivery, λ cutoff, Engagement ABC)
- Step 8: Identify mode of delivery (full BCIO module binding, APEASE final)

Conditional module overlays (proxy-based, not yet full modules):

- Dose: λ (Low/Medium/High), frequency/intensity axes
- Reach: Bronfenbrenner levels + Shepard distance
- Fidelity: Design/Delivery/Receipt axes, Shepard proxy
- Engagement: ABC + 6-level frame
- Schedule: Temporal Discounting proxy, interval/horizon axes

Hidden external theories: Shepard generalization, Temporal Discounting,
Bronfenbrenner ecology, ABC model, 6-level thinking frame

RXT flags per step: SR_perceptual, SR_affective, SR_context, Trust_t, λ ceiling

Language policy: session language from first message, BCIO IDs and module names in English,
trigger prefixes untranslated, term labels with parenthetical translation

Output format: step-by-step tables per BCW step, navigation bar at every checkpoint,
one step per response, confirmation-gated progression

APEASE application: official at Steps 5 and 8, soft check at Steps 4, 6, 7;
rule: 2+ No → option eliminated

Note: This version was a standalone BCW design tool. No q/b/id/t search modes.
BCIO ontology was used for intervention design only, not as a searchable database.

-----

## PHASE −1 — FIRST ONT_SEARCH_BOT (v11.1)

Date: 2026-03-09 to 2026-03-10

TRANSITION: BCW design assistant refactored into unified ontology search + design bot.
BCW mode absorbed as Mode d (Intervention Design). Search modes added as primary interface.

Architecture: Markdown-file-based, Code Interpreter mandatory  
Instruction layer: Promt_works.txt (System Prompt v9.0), REFERENCE_Controller.md (absolute authority)  
File count: ~7 Markdown files + 4 JSON data files + 3 legacy .md1 files (REFERENCE_Controller.md1,
REFERENCE_Mode2_Dialog.md1, WELCOME_MESSAGES.md1, LANGUAGE_RULES.md1)  
Note: .md1 files = versioned snapshots of earlier drafts, not active runtime files

Mode machine (q / b / id / t / d / 0):

|Trigger        |Mode                                       |
|---------------|-------------------------------------------|
|`q [query]`    |Search — Unified Ontology Search           |
|`b N` / `b N,M`|Browse combo                               |
|`id [query]`   |ID lookup (digits/text/mixed)              |
|`t`            |Text analysis (file upload required)       |
|`d`            |Intervention Design (absorbed BCW pipeline)|
|`0`            |Return to main menu                        |

Data layer: bcio_index.json (2607 terms), bcio_clean.json, bcio_by_module.json
(7534 entries, 8 core modules), 5modules_registry.json (65 extended entries:
Dose · Engagement · Fidelity · Reach · Schedule as overlay registries),
bcio_hierarchy_index.json, term_combo_index.json

Key architectural features established at this version:

- REFERENCE_Controller.md as master orchestrator with absolute authority
- GATE CHECK before any output in q/b/id/t modes (CI + JSON loaded)
- Session language detection from first message, fixed for session
- BCIO ID always paired with label, never output ID without label
- Hidden pre-processing pipeline (Q·P): referent split + 6 process type checks
- Block A → Block B (17 columns incl. extended module cols) → Block C → Block D → Block E pipeline
- Extended module columns in every Block B and Block C: Dose · Engagement · Fidelity · Reach · Schedule
- SEMANTIC_EXPANSION_TEMPLATE.md triggered for affective/abstract queries (Q·P proxy bridge)
- Mode T text analysis with T·1→T·9 pipeline, DESIGN HANDOFF 12-section output
- System protection rules: no pipeline revelation, warm deflection on meta-questions
- REFERENCE_Controller.md v3.4; Prompt v9.0 → v9.0 (with Rule 7 PDCA Block K addition)

Regression test material: Анна case (Анна 2 ответ.txt, Анализ текста Анна.txt, Отчет Анна.txt),
q-word requests (Запрос q слова.txt, Запрос q.txt), id lookup (Запрос id.txt),
T mode (Запрос T.txt, Режим T.txt), Кейс.txt, Кейс 2.txt

Issues identified and resolved in this period:

- Block D outputting prose instead of table → mandatory table-only enforcement added
- MFOEM:000033 missing from Block D → external prefix handling rule added
  (MFOEM/GO/BFO/IAO: Level=—, Parent=—, Children=— when OWL unavailable)
- Semantic analysis depth for affective/social queries → proxy bridge pass introduced

-----

## PHASE 0 — ONT_SEARCH_BOT v12 (v12.2 → internal builds v12–v27)

Date: 2026-03-12

RELEASE: ONT_SEARCH_BOT v12.2 (Prompt v10.0, internal builds v12–v27)  
Architecture: Markdown-file-based search bot  
Instruction layer: Promt.txt (System Prompt v10.0), REFERENCE_Controller.md (absolute authority)  
File count: 15 Markdown files + 1 instruction (Promt.txt)

File inventory at v12.2:
REFERENCE_Controller.md · REFERENCE_Search.md · SEMANTIC_EXPANSION_TEMPLATE.md ·
ROLES.md · WELCOME_MESSAGES.md · LANGUAGE_RULES.md · T_CORE_REFERENCE.md ·
T_OPERATIONAL_REFERENCE.md · T_ADHOC_REFERENCE.md · T_REVERSE_REFERENCE.md ·
TABLE_CONTRACTS.md · REFERENCE_ANTI_PATTERNS.md · REFERENCE_PUNCHLINE_CONSTRAINT_MAPPING.md ·
REFERENCE_OUTPUT_T_FULL.md · ADHOC_BOUNDARY_RULES.md

Mode machine: q / b / id / t triggers via text router (Mode d absent in this build)  
Dependency: Code Interpreter required for all non-welcome modes  
Pipeline: visible step-by-step execution (Q0→Q4, T1→T9)  
Output: 17-column Block B tables, Block C module columns, Block D hierarchy, Block E extensions  
Ontology source: bcio_index.json, bcio_clean.json, bcio_by_module.json (loaded via CI at runtime)

Key changes vs v11.1: T-mode split into 4 stability files (T_CORE / T_OPERATIONAL / T_ADHOC /
T_REVERSE); TABLE_CONTRACTS.md and REFERENCE_ANTI_PATTERNS.md added; ADHOC_BOUNDARY_RULES.md
and PUNCHLINE_CONSTRAINT_MAPPING.md added; ROLES.md added; Mode d removed from build;
QUALITY OVER SPEED declared non-negotiable; S8 silent self-check (12 points) mandatory.

Internal builds v12–v27 (2026-03-12 to 2026-03-16, logs not preserved):  
Iterative stabilization of T-mode output contracts, Block structure enforcement,
semantic expansion pipeline, and multi-language output rules.

### v19 CHECKPOINT — T-Only Bot (2026-03-16 to 2026-03-17)

Intermediate architecture snapshot: T-mode isolated as standalone bot for testing.
First migration of control layer to JSON format.

Control files: PROMPT_ROUTER.json · RUNTIME_ENFORCEMENT_CORE.json (v: module_first_full_v20) ·
T_RUNTIME_CONTRACT.json · MENU_CANONICAL.json · TABLE_SCHEMA_CONTRACT.json ·
ATOMICITY_CONTRACT.json · FILE_REGISTRY.json (REG-T-RESTORED-003)

Data layer: bcio_index.json · bcio_by_module.json · bcio_hierarchy_index.json ·
term_combo_index.json · BCIO_SEMANTICS_all.json · BCIO_SEMANTIC_LINK_RULES.json ·
BCIO_SEMANTIC_LINKS_manifest.json

New files introduced at v19:
T_SEMANTIC_MATCHING_CORE_with_neighborhoods.json — first appearance (semantic neighborhood graph)  
BCIO_SEMANTICS_all.json — semantic metadata layer added to data layer

Authority order (first JSON-embedded): Instructions → PROMPT_ROUTER →
RUNTIME_ENFORCEMENT_CORE → T_RUNTIME_CONTRACT → MENU_CANONICAL → BCIO data files

Architectural principles established at v19:

- JSON replaces Markdown as control-layer format (first time)
- Memory isolation contract: previous session memory forbidden
- Quality-over-speed priority order embedded in enforcement JSON
- Exact output enforcement: pattern literal fragment compliance, paraphrase forbidden
- legacy_fallback_forbidden · mode_auto_switch_forbidden · project_contracts_override_style
- Test suite present: Test q 1–3, Case bot 1–2, Режим t тест, Семантика модули

-----

## PHASE 1 — RUNTIME HARDENING (v28–v56)

**v28** Packaging cleanup: duplicate subtree removed, 55 unique files merged,
archive flattened to single root.

**v29** Instruction placement corrected: system_files layer vs. backlog layer
separation established.

**v30** DREFS templates approved in English. Runtime wording aligned across files.

**v31** Deep runtime-contract embedding: PROMPT, ALGORITHM, TABLE_SCHEMA_CONTRACT,
helper notes, manifest, inventory, and logs synchronized.

**v32** DREFS pre-table schema embedded. Table-specific meaning columns added
to output templates and runtime contracts.

**v33** First-gate scope guard added before task entry, before DREFS rendering,
and before table rendering.

**v34** Package rebuild: nested subtree removed, archive flattened,
instruction files assigned to correct layers.

**v35** Menu-only start behavior enforced. MANIFEST, PROMPT, runtime_enforcement
updated.

**v36** T1–T16 start menu introduced. Runtime files synchronized.

**v38** Full runtime synchronization: MANIFEST, PROMPT, ALGORITHM,
runtime_enforcement, helpers, schema, inventory, and logs.

**v40** Per-file root-path normalization. project_root compatibility enforced
across all runtime files.

**v41** Output formatting and security updates applied across runtime stack.

**v42** Strict visible-menu lock introduced.

**v43** Full enforcement lock: runtime_enforcement, ALGORITHM, PROMPT, schema,
helpers, manifest, and inventory locked.

**v50** MASTER_INSTRUCTION_ROOT_LOCK v50: 6-layer authority order established,
session state model formalized, GATE CHECK added as mandatory pre-step.

**v52** Optional E (Expand Candidate Search) action added after valid Report 1.
Repass rules: once per module, may increase/preserve/reduce candidate set.

**v56** OPERATIONAL_CONTROL_LAYER.json introduced (new file).
Pipeline principle: extraction-first / coverage-first / mapping-first /
patching-second. Coverage passport and stage checklists added.
Empty child report required when child search returns zero hits.
Sibling pass rendered as option space, not drop-only.
Gap rendered as bottleneck analysis with hypotheses and remediation.
Internal validation tables forbidden in visible output.

-----

## PHASE 2 — CONTRACT LAYER (v58–v64)

**v58** Minor runtime patch.

**v59** Minor runtime patch.

**v60** Minor runtime patch.

**v61** Noise filtering and signal extraction added before normalization (Wave 0).

**v62** Wave 6: Runtime builder/validator contracts added for:
coverage passport, pattern ledger, mapping ledger, empty child report,
sibling option space, gap bottleneck block, stage footer.
Pre-send checklist hard block and required-container validation introduced.

**v63** Minor patch.

**v64** Regression and acceptance wave. Package inherited as stable base for v65.

-----

## PHASE 3 — GRAPH SYSTEM (v65)

**v65** Wave 9: Predictive graph-analysis system introduced.
Three new data files built from THEORY_GRAPH_v5.json (339 entries):

```
MODULE_DEPENDENCY_GRAPH.json
  - 11 modules, 41 co-activation edges
  - Bottleneck weight formula: (failure_cluster_hits×2 + theory_count×0.5)
    / theory_count
  - Top success drivers: Engagement(223), Reach(120), Fidelity(110)
  - Top bottleneck modules: Mode(5.0), Population(3.25), Source(2.75)

BOTTLENECK_FORECAST_RULES.json
  - 101 bottleneck rules, 46 displacement pairs, 80 intervention rules
  - Extracted from failure_cluster and paradox-type theory entries

NODE_INTERVENTION_TEMPLATES.json
  - 13 per-module templates
  - predicted_effects: primary_beneficiary, cascade_risk,
    new_bottleneck_candidates

Graph mode activated by command G after valid Report 5.
Theory layer status: diagnostic_only (standard) → analytic (graph mode).
Build scripts: build_module_graph.py, build_bottleneck_rules.py,
build_node_intervention.py — all derive from THEORY_GRAPH_v5.json.

File count: 21 → 24 system files.
```

-----

## PHASE 4 — WEIGHT SYSTEM (v70–v72)

**v70** Wave 14: Route Weight System introduced.
Node weights for standard route (target = 100):
R1=20, R2=20, R3=15, R4=15, R5=15, R6=15  
Optional expanded repass (E): +10, target becomes 110.  
Skip compensation (legitimate skips transfer weight):

- R3 skipped (children_count=0) → +15 to R5
- R4 skipped (siblings_count=0) → +15 to R5
- Loop break R2→R6 (no candidates) → R3+R4+R5 (45) transferred to R6

Penalty rules:  
Non-contract table: −15 | Missing footer: −10  
Out-of-scope text: −10 | Missing coverage passport: −10  
Hypothesis without theory_id when coverage exists: −5 each  
Pre-send gate: accumulated_weight == target_sum → render.
Otherwise rebuild. No exceptions.  
Weight display: every stage footer shows `WEIGHT: {accumulated}/{target}`  
Files updated: RUNTIME_CONTRACTS.json, OPERATIONAL_CONTROL_LAYER.json,
MASTER_INSTRUCTION_ROOT_LOCK_v70.txt

**v71** Wave 15 (Dialog 54):  
PRE-RUN ROUTE CONFIRMATION: mandatory confirmation stage before Report 1.  
DREFS co-location: DREFS block rendered with each REPORT N.  
DREFS format standardized: metric|meaning|formula|threshold|example.  
DREFS 1 coverage coefficients added.  
Table header-lock discipline added to MASTER INSTRUCTION.  
dialog_54_pack regression checklist added to OPERATIONAL_CONTROL_LAYER.

**v72** Wave 16: Mass runtime alignment.  
Wave A–E decisions integrated into live runtime files:

- Wave A — route state, stage transitions, preflight validators, block mode discipline, route footer
- Wave B — module gate registry, recognition signal capture, candidate object schema, narrow-pass rules
- Wave C — Report 2 ID validation, three-state split (ontology/source/selection), children wording guard, siblings wording guard, anti-overmatch
- Wave D — theory layer constraints, theory overlay pass, theory gap citations and displacement forecast per module
- Wave E — acceptance criteria, session failure regression packs, Mercadona and Castro regression packs
- Wave F — graph system regression

-----

## PHASE 5 — ARCHITECTURE MIGRATION TO RAG (v80–v81)

Date: 2026-03-22 to 2026-03-25

MAJOR TRANSITION: Markdown-file architecture replaced by pure JSON
knowledge architecture. Code Interpreter dependency removed.
Visible pipeline hidden. Dual-mode operation introduced.

**v80** Base RAG architecture established.  
Knowledge layer migrated to structured JSON files.  
16 knowledge files + 1 instruction = 17 total runtime files.  
Inventory counts corrected in OPERATIONAL_CONTROL_LAYER.json.

**v80.2** Corrected runtime inventory counts to 16 knowledge files + 1 instruction = 17 total runtime files.
Updated embedded manifest and current_state_version in OPERATIONAL_CONTROL_LAYER.json.

**v80.4** Inventory cleanup:

- embedded_manifest current_package updated
- ambiguous file_count_system_files removed
- knowledge/instruction/runtime counts normalized
- merge-note fields removed from OPERATIONAL_CONTROL_LAYER.json

**v81 W0** Package-fit adaptation for upcoming normalizer.  
No analytical pipeline changes.  
Instruction title line removed to fit size limit.  
Reserved: REQUEST_NORMALIZER.json, REFERENCE_UserGuide.md,
REFERENCE_RequestPatterns.json.

**v81 W1** Normalizer architecture locked:

- Normalizer role: pre-analysis only, never default
- User consent required before normalizer activation
- No analysis launch before final user confirmation
- Unsupported route invention forbidden

Ownership prepared across OPERATIONAL_CONTROL_LAYER, EXECUTION_CORE, RUNTIME_CONTRACTS.

**v81 W2** REQUEST_NORMALIZER.json added (17th knowledge file).  
Components: task taxonomy, defect taxonomy, normalization rules,
reconstruction templates, sub-request chain builder,
activation and gate logic.  
Not yet built in this wave: REFERENCE_RequestPatterns.json,
REFERENCE_UserGuide.md, full route integration, final acceptance
and regression, wave tail cleanup.

**v81 W3** REFERENCE_RequestPatterns.json built and added.  
Inventory and anchors synced for pattern-library support.

**v81 W4** REFERENCE_UserGuide.md added.  
Package inventory updated to 19 knowledge files + 1 instruction.  
Route integration not yet applied.

**v81 W5** Route integration wave:
request_normalizer, request_normalizer_iteration, and user_guide
route nodes added. Pre-analysis offer gate and open-ended final
launch gate integrated. No analytical core changes. Wave tails not cleaned.

**v81 W6** Normalizer acceptance and regression packs added.  
Roadmap corpora used as primary regression sources.

**v81 W7** Final cleanup wave.  
Removed normalizer wave-tail blocks from runtime files and consolidated
into current runtime sections: request_normalizer_runtime,
request_normalizer_runtime_contract.  
No analytical core changes. No backlog participation in runtime.

**v81 W7 (guide/pdf update)** REFERENCE_UserGuide.md replaced by REFERENCE_UserGuide.pdf.
Guide access now returns PDF link only via starter phrase `Reference & guideMap`
or explicit guide/reference/help intent.
Inline guide rendering and i-based guide mode removed.

**v81 W7 (i-guide binding fix)** Updated files: OPERATIONAL_CONTROL_LAYER.json,
EXECUTION_CORE.json, RUNTIME_CONTRACTS.json,
MASTER_INSTRUCTION_ROOT_LOCK_v81_w7_final.txt.  
Purpose: bind `i` to full REFERENCE_UserGuide.md output and forbid
fallback to menu/reference stubs.

**v81 W8** Guide access refactored to single raw GitHub link.  
Canonical URL: `https://raw.githubusercontent.com/oleksiikartashovde-glitch/AISA-guide/main/REFERENCE_UserGuide.pdf`  
Local guide file removed from runtime package.  
Inline guide rendering forbidden. Analysis launch from guide request forbidden.

**v81 W8.1 (mini-wave)** Guide binding repair. Guide requests now return the exact literal URL only.
No labels, no citations, no placeholders, no inline guide text.

**v81 W9** Gate-before-route hard lock added.  
Complex requests must emit only the normalizer gate first.  
No route selection before gate completion.

-----

## PHASE 6 — SIGNAL GATEWAY AND OUTPUT SPEC (v84)

Date: 2026-03-29 to 2026-03-30

**v83 W0** Wave 0 added: noise filtering and signal extraction before normalization.

**v84** Three-layer normalization architecture introduced:

- REQUEST_SIGNAL_GATEWAY.json — Wave 0 owner, input classification
- REQUEST_NORMALIZER.json — request specification, gap expansion, deduplication/compression, single normalized request assembly
- REQUEST_OUTPUT_SPEC.json — Wave 2 owner, output form specification

APEASE entry gate added (Problem / Goal / Result).  
Single normalized request invariant enforced.  
Single output specification invariant enforced.  
Normalization regression fit checks added.

**v84.17** Full package harmonization.  
Unified hidden request JSON across all pipeline stages.  
Normalization ending at assigned BCIO ID terms only.  
Dense normalization coverage including population/programme entities.  
Optional pre-analysis theory table added.  
Mandatory output form and Analyze? gate introduced.  
Visible flow:

1. Normalize request? + empty normalization table header
1. Filled normalization table
1. Optional theory table
1. Output form
1. Analyze?
1. Analysis or stop

Updated system files: MASTER_INSTRUCTION_ROOT_LOCK_v84.txt,
MASTER_INSTRUCTION_ROOT_LOCK_v84_16.txt, REQUEST_SIGNAL_GATEWAY.json,
REQUEST_NORMALIZER.json, REQUEST_OUTPUT_SPEC.json, EXECUTION_CORE.json,
RUNTIME_CONTRACTS.json, OPERATIONAL_CONTROL_LAYER.json.

**v84.22** Free analytic orchestration mode introduced.  
Visible mode: free analytical (no visible pipeline).  
Hidden mode: strict structured processing.  
Dual-mode architecture finalized.

**v84.23** Language policy layer added.  
Default response language: user language.  
Ontology ID language: original (English).  
Ontology label: English + (user language) parenthetical.  
Ontology definition: user language.  
Semantic query bridge language: English internally.  
Translation override by user: allowed.

-----

## RELEASE: v84.24 — FINAL PACKAGE

Date: 2026-03-30

ARCHITECTURAL COMPRESSION:  
Before merge: 21 files (20 JSON + 1 instruction)  
After merge:  13 files (12 JSON + 1 instruction)  
Freed: 8 file slots

Merge operations performed:

- runtime_enforcement.json → merged into OPERATIONAL_CONTROL_LAYER.json
- TABLE_SCHEMA_CONTRACT.json → merged into RUNTIME_CONTRACTS.json
- BCIO_ID_GATE_RULES.json → merged into BCIO_CANDIDATE_RANKER.json
- BCIO_PATTERN_TO_ID_FORECAST.json → merged into BCIO_CANDIDATE_RANKER.json
- bcio_hierarchy_index.json → merged into bcio_index.json (as hierarchy_index)
- term_combo_index.json → merged into bcio_index.json (as term_combo_index)
- BOTTLENECK_FORECAST_RULES.json → merged into MODULE_DEPENDENCY_GRAPH.json
- NODE_INTERVENTION_TEMPLATES.json → merged into MODULE_DEPENDENCY_GRAPH.json

FINAL SYSTEM FILES (13):

**Knowledge layer (9 files):**

|File                                            |Role                                                                     |
|------------------------------------------------|-------------------------------------------------------------------------|
|bcio_index.json                                 |primary ontology index (merged, ~20.7MB)                                 |
|bcio_clean.json                                 |curated term set                                                         |
|bcio_by_module.json                             |module-partitioned ontology                                              |
|BCIO_SEMANTICS_all.json                         |semantic definitions                                                     |
|THEORY_GRAPH_v5.json                            |339 theory/paradox/bias entries                                          |
|MODULE_DEPENDENCY_GRAPH.json                    |co-activation matrix + bottleneck rules + intervention templates (merged)|
|T_SEMANTIC_MATCHING_CORE_with_neighborhoods.json|semantic neighborhoods                                                   |
|BCIO_NORMALIZATION_RULES.json                   |normalization rule set                                                   |
|BCIO_CANDIDATE_RANKER.json                      |candidate ranking + ID gate rules + pattern-to-ID forecast (merged)      |

**Control layer (3 files):**

|File                          |Role                                                                     |
|------------------------------|-------------------------------------------------------------------------|
|EXECUTION_CORE.json           |13-step execution pipeline, owner precedence                             |
|OPERATIONAL_CONTROL_LAYER.json|route nodes, pre-send checklist, weight system, regression packs (merged)|
|RUNTIME_CONTRACTS.json        |table schemas, DREFS contracts, route weight contracts (merged)          |

**Gateway layer (3 files):**

|File                       |Role                              |
|---------------------------|----------------------------------|
|REQUEST_SIGNAL_GATEWAY.json|input classification, Wave 0 owner|
|REQUEST_NORMALIZER.json    |normalization engine              |
|REQUEST_OUTPUT_SPEC.json   |output form specification         |

**Supplementary (2 files):**

|File                          |Role                        |
|------------------------------|----------------------------|
|REFERENCE_RequestPatterns.json|pattern library             |
|APEASE_FULL_SPEC.json         |APEASE framework integration|

**Instruction (1 file):**

|File                                   |Role                                                                               |
|---------------------------------------|-----------------------------------------------------------------------------------|
|MASTER_INSTRUCTION_ROOT_LOCK_v84_23.txt|conductor document, global behaviour line, hard bans, root anchors, language policy|

FINAL ARCHITECTURE PROPERTIES:

- Dual-mode: free analytical (visible) + strict structured (hidden)
- Hidden request JSON: mandatory before any visible answer
- Mixed input repartition: source_material / user_questions / output_form_request
- Normalization source: BCIO-only by default, user override allowed
- Theory jump: forbidden without explicit user instruction
- Framework jump: forbidden without explicit user instruction
- Visible internal pipeline: forbidden
- Ontology render: ID (original) + Label (English + user language) + Definition (user language)
- Language policy: user language default, English bridge for semantics
- Published: GPT Store as AISA 11.1 by Oleksii Kartashov

-----

## PHASE 7 — BCIO DATABASE UPDATE (DB31 / DREFS-v1)

Date: 2026-03-31 to 2026-04

### Overview

Post-release ontology update expanding DREFS module coverage and migrating to a
full 13-module flat architecture. Applied to all files in bcio_updated.zip.

### Module structure change — 13 modules, no stars

Previously: 8 core modules + 5 DREFS modules suffixed with `*` (auxiliary overlays).  
Now: exactly 13 modules, all equal peers. The `modules` field on every term
contains only clean module names without star suffixes.

**Core 8:** Mechanisms of Action · BCT · Behaviour · Population ·
Style of Delivery · Source · Mode of Delivery · Setting

**DREFS 5 (now full modules):** Dose · Engagement · Fidelity · Reach · Schedule

Algorithm change: remove any logic that checks for `*` suffix, merges star/non-star
modules, or treats DREFS modules as secondary. All 13 modules are now peers.

### DREFS module expansion — approved additions applied (2026-03-31)

50 approved additions applied across 5 DREFS modules:

|Module    |Before|After|Added|
|----------|------|-----|-----|
|Dose      |32    |42   |+10  |
|Engagement|58    |79   |+21  |
|Fidelity  |27    |44   |+17  |
|Reach     |22    |37   |+15  |
|Schedule  |33    |35   |+2   |

All additions are cross-references to existing terms — no new terms were created.  
Unique source term IDs affected: 48. ID strings changed after suffix synchronization: 45.

### New cross-reference IDs — 59 terms

When a term was added to a new module, its ID was updated by appending the module
suffix code to the chain segment.

Suffix codes:
`bc`=BCT · `be`=Behaviour · `d`=Dose · `e`=Engagement · `f`=Fidelity ·
`ma`=Mechanisms of Action · `md`=Mode of Delivery · `p`=Population ·
`r`=Reach · `sd`=Style of Delivery · `se`=Setting · `sh`=Schedule · `so`=Source

Suffix order in chain: alphabetical by code.

Example:

```
awareness (was only in MoA):  BCIO:006015.03.27.ma
After adding to Reach:        BCIO:006015.03.27.ma.r
```

### New field: `module_position` in bcio_index

Every term now has a `module_position` field mapping each module the term belongs
to → its position in that module’s 3-level hierarchy (category / group / subgroup).
Used as the primary semantic matching path for case decomposition.

### New field: `hierarchy_nav` in bcio_by_module

Every module entry now contains `hierarchy_nav` — a complete navigational map of
the module’s category→group→subgroup structure with term counts and descriptions.
Use for top-down navigation; do not scan all 2548 terms.

### Decomposition algorithm update

Old flow (flat): `case text → normalize → search all terms → rank → return`

New flow (hierarchical):

1. Identify relevant modules from case text via module descriptions
1. For each module: match case text against `category_desc` → select 1–3 categories
1. Within categories: match against `group_desc` → select relevant groups
1. Within groups: retrieve terms via `bcio_index module_position` → run BCIO_CANDIDATE_RANKER
1. Return ranked terms with their position context

Reduces candidate space from ~2548 to ~10–50 terms per module before ranking.

### bcio_by_module.json — slim format

Reduced from 36MB to 1.1MB. Terms inside each module now contain only `id` and `label`.
Use it only for navigation via `hierarchy_nav` and getting the list of IDs in a
module (`terms[].id`). Fetch full term data from `bcio_index.json` by ID.

### Encoding fix

One term label corrected:

- Before: `sp?cification d?intervalle de dose`
- After: `spécification d'intervalle de dose`

Updated in: bcio_clean, bcio_index, bcio_by_module, BCIO_NORMALIZATION_RULES.

### Files changed in DB31 update

|File                                            |Role                                 |Changed                          |
|------------------------------------------------|-------------------------------------|---------------------------------|
|bcio_clean.json                                 |Master term list (2548 terms)        |✓ IDs, modules, encoding         |
|bcio_index.json                                 |Main lookup with hierarchy           |✓ IDs, modules, `module_position`|
|bcio_by_module.json                             |Terms grouped by module              |✓ 13 modules, `hierarchy_nav`    |
|bcio_hierarchy_index.json                       |Tree structure with children/siblings|✓ IDs, stars removed             |
|BCIO_NORMALIZATION_RULES.json                   |Normalized label lookup              |✓ IDs, stars removed             |
|term_combo_index.json                           |Combo terms (2726 entries)           |✓ IDs, stars removed             |
|BCIO_SEMANTICS_all.json                         |Semantic metadata                    |✓ modules, stars removed         |
|T_SEMANTIC_MATCHING_CORE_with_neighborhoods.json|Semantic neighborhoods               |✓ IDs, stars removed             |
|bcio_hierarchy_detailed.md                      |Full hierarchy reference             |NEW                              |
|bcio_hierarchy_summary.md                       |Module counts table                  |NEW                              |

Not changed: THEORY_GRAPH_v5.json, BCIO_CANDIDATE_RANKER.json,
MODULE_DEPENDENCY_GRAPH.json, APEASE_FULL_SPEC.json, APEASE_REFERENCE.json,
operational control files.

### DB31 algorithm patch

Active control files updated to use index-first routing:

- EXECUTION_CORE.json
- OPERATIONAL_CONTROL_LAYER.json
- REQUEST_NORMALIZER.json
- BCIO_CANDIDATE_RANKER.json
- T_SEMANTIC_MATCHING_CORE_with_neighborhoods.json
- RUNTIME_CONTRACTS.json

Reference docs moved to `backlog/db31_update_docs/` (non-executable).

### Hierarchy scale by module (post-DB31)

|Module              |Terms|Categories|Groups|Subgroups|
|--------------------|-----|----------|------|---------|
|Mechanisms of Action|1874 |28        |112   |63       |
|BCT                 |546  |14        |65    |35       |
|Behaviour           |1329 |21        |79    |38       |
|Population          |1113 |19        |63    |22       |
|Style of Delivery   |1463 |22        |86    |38       |
|Source              |396  |16        |55    |19       |
|Mode of Delivery    |324  |14        |46    |14       |
|Setting             |315  |16        |52    |18       |
|Dose                |42   |5         |11    |0        |
|Engagement          |79   |7         |18    |2        |
|Fidelity            |44   |5         |10    |0        |
|Reach               |37   |4         |10    |0        |
|Schedule            |35   |5         |10    |0        |

-----

## EVOLUTION SUMMARY

|Date         |Version  |Milestone                                                                                                                 |
|-------------|---------|--------------------------------------------------------------------------------------------------------------------------|
|2026-03-09/10|BCW BOT 3|BCW design assistant — 8-step BCW pipeline, BCIO mapping, conditional DREFS overlays, Code Interpreter, 6 MD + 5 JSON     |
|2026-03-09/10|v11.1    |ONT_SEARCH_BOT v9.0 — search modes added (q/b/id/t/d), Controller authority, 7 MD + 4 JSON, Gate Check, Block A–E pipeline|
|2026-03-12   |v12      |ONT_SEARCH_BOT v10.0 — 15 MD files, T-mode split into 4 stability files, Mode d removed, anti-pattern enforcement         |
|2026-03-16/17|v19      |T-only checkpoint — first JSON control layer, T_SEMANTIC_MATCHING_CORE introduced, BCIO_SEMANTICS_all added               |
|2026-03-22   |v72      |Weight system, DREFS, graph analysis, JSON migration begins, 24 system files                                              |
|2026-03-25   |v80      |Full RAG architecture, 17 JSON files, CI removed                                                                          |
|2026-03-29   |v84      |Signal gateway, output spec, APEASE gate                                                                                  |
|2026-03-30   |v84.24   |Final release — 13 files, dual-mode, language policy, published to GPT Store as AISA 11.1                                 |
|2026-03-31   |DB31     |BCIO DB update — DREFS-v1: 13 flat modules, 50 additions, module_position, hierarchy_nav                                  |

Total development span: ~23 days (2026-03-09 to 2026-04)  
Versions tracked: BCW BOT 3 → v11.1 → v12–v27 → v28–v84.24 + DB31 (~75+ named releases)  
Architecture shifts: 3 major (Markdown→JSON control layer at v19; full JSON RAG at v80; single-mode→dual-mode at v84.22)  
Files at origin (BCW BOT 3): 11 (6 MD + 5 JSON)  
Files at v11.1: ~11 active + 3 legacy .md1 snapshots  
Files at v12.2: 15 MD + 1 instruction  
Files at release (v84.24): 13 JSON + 1 instruction  
Ontology terms at release: 2548 terms across 13 modules

# ================================================================================

END OF MERGE LOG
