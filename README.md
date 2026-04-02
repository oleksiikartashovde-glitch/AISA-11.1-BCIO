# AISA 11.1

*Ontology-driven case analysis across all 13 modules*

**The quality of the response depends on the quality of the request.**

## What AISA 11.1 is

AISA 11.1 is a controlled analysis assistant for working with cases, behavioural material, intervention logic, and ontology-backed mapping.

It works with any domain where there is behaviour: healthcare, retail, HR and workplace programmes, public policy, digital products, education, safety systems, technical deployment studies, and others.

**The entry threshold is low.** You do not need to know BCIO or any ontology to start working. You can describe a problem in plain language, paste a case in everyday words, or submit a technical paper — AISA normalizes the input into the ontology internally. The more structured your request, the more precise the output. But unstructured input works too.

It is designed to:

- extract explicit problems
- detect hidden problems
- break problems into behavioural patterns
- normalize patterns
- map patterns to ontology IDs
- preserve the logic of the analysis across intermediate steps
- move from problem structure to behavioural diagnosis and intervention design

AISA is not intended for free-form improvisation.
It is intended for controlled analytical work.

-----

## What it works with

AISA works with:

- **uploaded files**
- **pasted text**

**Always include the full case or material in the body of the request.** Do not refer to a previous message or say “the same paper as above.” Each request that starts a new analysis must contain the full source material.

Primary workflow input should come from those two sources.

-----

## File structure

### User-facing

- `README.md` — top-level overview of AISA 11.1
- `REFERENCE_UserGuide.md` — practical guide for step-by-step work with the bot

### Runtime and control

- `REQUEST_SIGNAL_GATEWAY.json` — input signal handling and routing constraints
- `REQUEST_NORMALIZER.json` — normalization and hidden request structuring rules
- `REQUEST_OUTPUT_SPEC.json` — output-form rules
- `EXECUTION_CORE.json` — execution order and control logic
- `RUNTIME_CONTRACTS.json` — runtime rendering and normalization contracts
- `OPERATIONAL_CONTROL_LAYER.json` — orchestration and owner-precedence layer

### Ontology, theory, and planning

- `BCIO_NORMALIZATION_RULES.json` — ontology normalization rules
- `BCIO_CANDIDATE_RANKER.json` — ontology candidate ranking rules
- `bcio_clean.json` — loaded ontology term base
- `bcio_by_module.json` — ontology terms grouped by module (slim format, hierarchy_nav)
- `bcio_index.json` — primary ontology index with module_position and hierarchy
- `BCIO_SEMANTICS_all.json` — semantic support layer for ontology matching
- `THEORY_GRAPH_v5.json` — coded theory, bias, and effect layer
- `MODULE_DEPENDENCY_GRAPH.json` — inter-module dependency structure
- `APEASE_REFERENCE.json` — APEASE entry layer
- `APEASE_FULL_SPEC.json` — full APEASE operational logic

This file map is intentionally selective.
It shows the main files needed to understand how AISA is used, controlled, and grounded.

-----

## Main use cases

AISA can be used for:

- explicit problem extraction
- hidden problem extraction
- behavioural patterning
- normalization
- ontology ID mapping
- target behaviour identification
- gap analysis
- DREFS analysis
- COM-B mapping
- TDF mapping
- APEASE assessment
- intervention-function selection
- policy-category selection
- theory / bias / effect mapping
- final solution design across all 13 modules

-----

## Core operating principles

### 1. One request = one task

Each request should contain one main task.

The more tasks are packed into one request:

- the higher the risk of semantic drift
- the higher the chance of correction loops
- the higher the chance of hallucinated structure
- the harder it becomes to validate intermediate outputs

### 2. Each next step grows out of the previous one

AISA is designed for chained analytical work.

Typical logic:

- extract
- normalize
- map
- diagnose
- redesign

### 3. The parent request defines the analysis frame

Follow-up requests are valid when they remain grounded in:

- the parent request
- the accepted source material
- the intermediate outputs already produced

### 4. Controlled combinatorics is allowed

The user may:

- refine intermediate outputs
- narrow or broaden a specific result layer
- request re-mapping, re-normalization, or stricter semantic matching
- change formatting or table structure
- ask for alternative cuts of the same already-produced data
- return to an earlier intermediate result and rebuild it
- reformat any table without restarting the analysis
- change column structure, definition language, or label format at any point

This is allowed only inside the analysis frame already defined by the parent request. AISA does not restart from zero on each follow-up. It carries forward the validated analysis space.

### 5. Ontology mapping is not free-floating

From the ontology-mapping step onward, the stable working unit is:

**No. + source text + normalization + ID**

This anchor should persist across downstream analysis.

-----

## How to control the output and prevent drift

The more precisely the output format is specified, the less AISA drifts beyond what was requested.

### Specifying table columns

State the exact column names in the request:

> `Format: Table / No. / Pattern / ID / Label / Definition`

### Requesting ontology IDs

**Always write “ontology ID” or “ontology across all 13 modules available to the bot” — not “BCIO ID.”**

Writing “BCIO ID” restricts the bot to the BCIO prefix only and prevents it from returning GO, MeSH, APA, iSci, ADDICTO, or other loaded ontology terms that may be a better fit for the pattern.

### Specifying the label format

When you want bilingual labels:

> `Label format: English label (target language label). Definition in [language].`

### Fixing APEASE output order

> `Apply APEASE in fixed order: S, E, Eq, P, A, Af.`

### Restricting to specific modules

> `Match using Source, Mode of Delivery, and Setting modules only.`

-----

## Ontology logic

AISA works with **the ontology across all 13 modules available to the bot**.

It does not rely only on terms with the `BCIO:` prefix.
Imported ontology IDs are also present in the loaded ontology space.

### Verified ontology counts (post-DB31)

- **total ontology IDs loaded:** 2548
- **BCIO-prefix IDs:** 2319
- **imported non-BCIO IDs:** 229

### Imported ontology sources include

- ADDICTO
- APA
- GO
- MeSH
- MF
- MFOEM
- iSci
- ENVO
- OMRSE
- and others

### 13-module flat architecture (post-DB31)

All 13 modules are equal peers. There are no star suffixes or auxiliary overlays.

**Core 8:** Mechanisms of Action · BCT · Behaviour · Population · Style of Delivery · Source · Mode of Delivery · Setting

**DREFS 5:** Dose · Engagement · Fidelity · Reach · Schedule

### Hierarchy scale by module

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

## Module logic and small-letter suffixes

Many IDs contain small-letter suffixes that indicate module-level routing or linked layers. One ID can carry multiple suffixes when it belongs to more than one module.

Common suffixes:

- **.be** = Behaviour
- **.p** = Population
- **.ma** = Mechanisms of Action
- **.md** = Mode of Delivery
- **.se** = Setting
- **.so** = Source
- **.sd** = Style of Delivery
- **.bc** = Behaviour Change / intervention-layer anchor
- **.d** = Dose
- **.r** = Reach
- **.e** = Engagement
- **.f** = Fidelity
- **.sh** = Schedule

AISA does not only read labels.
It also checks whether the selected ID is structurally and semantically consistent with the intended module.

### Example IDs

- `BCIO:006119.07.04.ma.d.e.sh` → MoA + Dose + Engagement + Schedule
- `BCIO:050887.00.01.be.p.ma.sd.r.f` → Behaviour + Population + MoA + Style of Delivery + Reach + Fidelity
- `BCIO:050950.00.04.ma.r.e` → MoA + Reach + Engagement
- `iSci:REAIM.00.00.bc.p.r.f` → BCT + Population + Reach + Fidelity
- `MeSH:D001143.00.00.ma.d.sh` → MoA + Dose + Schedule

-----

## Parent, children, and siblings

AISA checks ontology neighbourhood before confirming a match.

### Terms

- **Parent** = the broader class above the candidate term
- **Children** = narrower terms under the candidate term
- **Siblings** = terms that share the same parent

This matters because a pattern may fit:

- the exact term
- a broader parent zone
- a narrower child zone
- or a neighbouring sibling zone

AISA uses these checks to avoid shallow word-overlap mapping.

-----

## BPMS and DREFS

### BPMS

In AISA, BPMS is a non-standard term specific to this runtime. It refers to the intervention-design layer across the main modules used in the runtime package.

In practical work, final solution breakdown is performed across **all 13 modules**.

### DREFS

DREFS is a non-standard term specific to this bot. It is the gap-analysis layer used to explain why target behaviour was or was not achieved.

DREFS is not a published framework. It is an author-built codification assembled from the internal ontology. Every term in DREFS has a real ontology ID. There are no free labels.

It consists of 5 modules:

- **D** = Dose
- **R** = Reach
- **E** = Engagement
- **F** = Fidelity
- **S** = Schedule

### Verified DREFS counts (post-DB31, from bcio_index.json)

IDs with multiple module suffixes appear in more than one module count.

|Module                           |Terms in index|
|---------------------------------|--------------|
|D — Dose                         |35            |
|R — Reach                        |37            |
|E — Engagement                   |68            |
|F — Fidelity                     |31            |
|S — Schedule                     |35            |
|**Total entries (with overlaps)**|**206**       |
|**Unique IDs across all DREFS**  |**189**       |

### Example DREFS IDs

#### Dose

- `BCIO:006109.00.16.ma.d` — feeling an urge
- `BCIO:006110.00.05.ma.d` — comprehension
- `BCIO:006111.00.05.ma.d` — decision simplification
- `BCIO:006119.07.04.ma.d.e.sh` — associative learning

#### Reach

- `BCIO:006088.00.04.ma.r` — healthcare access
- `BCIO:006089.03.04.ma.r` — physical behavioural opportunity
- `BCIO:006090.00.04.ma.r` — social behavioural opportunity
- `BCIO:006142.00.02.ma.r.f` — belief about the credibility of a message’s source

#### Engagement

- `BCIO:006023.00.31.ma.e` — belief about gain
- `BCIO:006027.00.31.ma.e` — belief about reduction
- `BCIO:006059.00.08.ma.e` — motivation to comply
- `BCIO:006060.02.03.ma.e` — motivational orientation

#### Fidelity

- `BCIO:006102.00.14.ma.f` — plan enactment
- `BCIO:006142.00.02.ma.r.f` — belief about the credibility of a message’s source
- `BCIO:007010.00.17.bc.f` — action planning BCT
- `iSci:FidelityDrift.00.00.ma.f` — fidelity drift

#### Schedule

- `BCIO:006119.07.04.ma.d.e.sh` — associative learning
- `BCIO:006120.00.06.ma.sh` — classical conditioning
- `BCIO:009000.00.03.bc.be.p.ma.md.se.so.sd.d.sh` — BCI schedule of delivery
- `MeSH:D008568.00.00.ma.sh` — memory

-----

## Coded theory layer

AISA can also work with:

- theories
- biases
- effects
- paradoxes
- decision models
- stage models

These are not treated as loose narrative tags.
They are handled through ontology-linked coded resources.

### Loaded theory graph

- **total coded entries:** 339

Theory-layer work should start only after explicit problems, hidden problems, patterns, normalization, and ontology ID mapping are already stable.

-----

## Behavioural frameworks available in AISA

AISA can move from ontology-backed problem mapping into:

### COM-B

- Capability
- Opportunity
- Motivation
- Behaviour

### TDF

- 14-domain Theoretical Domains Framework layer

### APEASE

- Acceptability
- Practicability
- Effectiveness
- Affordability
- Side-effects
- Equity

Fixed output order: **S, E, Eq, P, A, Af**

### Intervention functions

Examples:

- education
- training
- persuasion
- enablement
- environmental restructuring
- modelling
- incentivisation

### Policy categories

Examples:

- guidelines
- service provision
- communication / marketing
- environmental / social planning

-----

## Recommended workflow

The exact route depends on the user’s goal, but a common deep-analysis route is:

### Step 1

`Identify the explicit problems in the case. Format: Table / No. / page no. / problem text.`

### Step 2

`Now identify the hidden problems. Continue the numbering.`

### Step 3

`Break all identified problems into behavioural patterns and normalize them in the same step. Format: Table / No. / problem text / pattern / normalization.`

### Step 4

`Match each pattern to the best ontology ID using the ontology across all 13 modules available to the bot. Format: Table / No. / text / normalization / ID / label / definition.`

### Step 5

`Identify the target behaviour. Say whether it was achieved. Show the gap.`

### Step 6

`Break the gap down using DREFS ontology IDs. Format: Table / DREFS module / ID / label / gap in this case.`

### Step 7

`Map the problems to COM-B. Table / No. / Capability / Opportunity / Motivation / Behavior.`

### Step 8

`Map the problems to TDF. Table / No. / TDF 14 Domain.`

### Step 9

`Apply APEASE in fixed order S, E, Eq, P, A, Af. Format: Table / criterion / assessment / verdict / evidence.`

### Step 10

`Now break the solution down across all 13 modules and their ontology IDs. Format: Table / module / ID / label / case instantiation.`

-----

## Query design rules

### One request = one task

This is the main operational rule.

### Each next request should grow out of the previous result

Do not jump ahead if the previous layer is still unstable.

### The order depends on the goal

The starting point can vary:

- extraction
- problem finding
- normalization
- ontology mapping
- target behaviour analysis
- DREFS
- COM-B / TDF
- APEASE
- intervention design
- 13-module breakdown

The route is not fixed by a rigid script.
But it must still remain logically chained.

-----

## Useful refinement commands

At any point you can say:

- `refine this`
- `make it stricter`
- `narrow the analysis`
- `expand the analysis`
- `rebuild semantically`
- `do not choose the nearest term too early`
- `show exact / broad / surrogate separately`
- `do not move on until we validate this step`

-----

## Document links

- **User Guide:** <REFERENCE_UserGuide.md>

-----

## Title-page wording

Recommended title:

**AISA 11.1**
*Reference & User Guide*
*Ontology-driven case analysis across all 13 modules*

Recommended core statement:

**The quality of the response depends on the quality of the request.**

Optional harder second line:

*Garbage in, garbage out.*
