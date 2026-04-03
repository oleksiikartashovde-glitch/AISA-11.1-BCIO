# REFERENCE_UserGuide
Bot: [Open AISA 11.1 in ChatGPT](https://chatgpt.com/g/g-69a6ab4b5adc81919a5be72eecbd2f81-aisa-11-1)  
## What this bot does

This bot supports structured analysis of any system where there is behaviour. It works with published research, intervention descriptions, policy documents, programme materials, case descriptions, technical deployment studies, corporate programmes, and any other material where behaviour and its drivers can be identified.

It can work with any domain: healthcare, retail, HR and workplace programmes, public policy, digital products, education, safety systems, and others.

**The entry threshold is low.** You do not need to know BCIO or any ontology to start working. You can describe a problem in plain language, paste a case in everyday words, or submit a technical paper — the bot normalizes the input into the ontology internally. The more structured your request, the more precise the output. But unstructured input works too.

Its strengths:

- identifying **explicit problems**
- detecting **hidden problems**
- breaking problems into **behavioural patterns**
- performing **normalization**
- matching patterns to **ontology IDs**
- extracting **module affiliation** from IDs
- identifying **target behaviour**
- showing the **gap**
- analyzing non-achievement through **DREFS**
- mapping to **COM-B** and **TDF**
- selecting **intervention functions** and **policy categories**
- identifying **theories / biases / effects**
- assessing solutions through **APEASE**
- producing a final breakdown across **all 13 modules**

-----

## Core abbreviations and ontology notes

### BPMS

In this bot, BPMS is a non-standard term specific to this runtime. It refers to the eight BCIO sub-ontology modules that form the intervention-description layer. The abbreviation follows the first letter of each module name:

- B = Behaviour + BCT (Behaviour Change Technique)
- P = Population
- M = Mechanisms of Action + Mode of Delivery
- S = Source + Setting + Style of Delivery

These eight modules are fully published on bciosearch.org. In practical work, final solution breakdown is carried out across all 13 modules — BPMS plus the five DREFS modules (Dose, Reach, Engagement, Fidelity, Schedule).

In practical work, final solution breakdown is carried out across **all 13 modules**, not reduced to only one or two end-stage boxes.

### DREFS

**DREFS** is a non-standard term specific to this bot. It is the gap-analysis layer used to explain why target behaviour was or was not achieved.

DREFS is not a published framework. It is an **author-built codification assembled from the internal ontology**. Every term in DREFS has a real ontology ID from BCIO, GO, MeSH, APA, iSci, ADDICTO, MF, MFOEM, or other loaded ontologies. There are no free labels.

DREFS consists of 5 modules:

- **D** = Dose
- **R** = Reach
- **E** = Engagement
- **F** = Fidelity
- **S** = Schedule

### DREFS term counts from the index

The following counts are taken directly from the runtime index (`bcio_index.json`). IDs with multiple module suffixes (e.g. `.ma.d.e.sh`) appear in more than one module count.

|Module                           |Terms in index|
|---------------------------------|--------------|
|D — Dose                         |43            |
|R — Reach                        |40            |
|E — Engagement                   |90            |
|F — Fidelity                     |50            |
|S — Schedule                     |37            |
|**Total entries (with overlaps)**|**260**       |
|**Unique IDs across all DREFS**  |**224**       |

### New codification principles

The bot uses a stricter codification logic when matching patterns to ontology IDs.

Main principles:

- normalization is done in **lower case**
- parenthetical text is stripped before matching
- dashes are normalized
- whitespace is collapsed
- matching is strengthened by checking **children**, **siblings**, and parent-neighbourhood consistency
- cross-module matching is allowed only when supported by the text fragment and module semantics
- IDs are not treated as free-floating labels

This means the bot does not only match terms by surface similarity. It also checks semantic neighbourhood and module consistency before assigning an ID.

-----

## Module suffixes in IDs and neighbourhood logic

### Small-letter module suffixes

In many IDs, the small-letter suffixes indicate module-level routing or attached module layers. One ID can carry multiple suffixes when it belongs to more than one module.

Common suffixes used in this runtime:

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

These suffixes matter because the bot does not only read the label. It also checks whether the ID is structurally consistent with the intended module.

### Examples of multi-suffix IDs

- `BCIO:006119.07.04.ma.d.e.sh` → MoA + Dose + Engagement + Schedule
- `BCIO:050887.00.01.be.p.ma.sd.r.f` → Behaviour + Population + MoA + Style of Delivery + Reach + Fidelity
- `BCIO:050950.00.04.ma.r.e` → MoA + Reach + Engagement
- `iSci:REAIM.00.00.bc.p.r.f` → BCT + Population + Reach + Fidelity
- `MeSH:D001143.00.00.ma.d.sh` → MoA + Dose + Schedule

This is why DREFS term counts show overlaps: the same ID contributes to multiple modules.

### Parent, children, and siblings

The bot also checks ontology neighbourhood before confirming a match.

- **Parent** = the broader class above the candidate term
- **Children** = narrower terms under the candidate term
- **Siblings** = terms that share the same parent

This is important because a pattern may not match the exact term directly, but may still fit the parent zone, a child zone, or a sibling zone. The bot uses this neighbourhood check to avoid shallow surface matching.

### Example 1

If the candidate term is:

- `BCIO:011000.15.03.bc.be.p.ma.md.se.so.sd`
- label: `behaviour change intervention mode of delivery`

then:

- its **parent** is a broader BCI attribute term
- its **children** include narrower delivery-mode terms
- its **siblings** include other intervention attribute terms under the same parent

### Example 2

If the candidate term is:

- `BCIO:001000.00.07.bc.be.p.ma.md.se.so.sd`
- label: `behaviour change intervention scenario`

then the bot checks whether the problem fragment really fits that term itself, or instead fits one of its neighbouring terms such as:

- `behaviour change intervention`
- `behaviour change intervention delivery`
- `behaviour change intervention content`
- `behaviour change technique`

This prevents drift into a nearby but wrong ontology anchor.

### Why this matters

The bot does not assign IDs only by word overlap. It also checks:

- semantic fit
- module fit
- parent fit
- child fit
- sibling fit

That is why two similar-looking IDs may not be equally valid for the same normalized pattern.

-----

## Ontology coverage in the loaded database

Verified counts from the loaded ontology files:

- **total ontology IDs loaded:** **2548**
- **BCIO-prefix IDs:** **2319**
- **imported non-BCIO IDs:** **229**

Imported IDs in the loaded ontology come from additional ontologies and vocabularies such as:
ADDICTO, APA, GO, MeSH, MF, MFOEM, iSci, ENVO, OMRSE, and others.

**This means the bot does not work only with `BCIO:` IDs.**
It works with the ontology across all 13 modules available to the bot.

-----

## DREFS ontology composition

### Verified DREFS counts

Core DREFS modules only:

- **Dose:** 43 IDs
- **Reach:** 40 IDs
- **Engagement:** 90 IDs
- **Fidelity:** 50 IDs
- **Schedule:** 37 IDs

Total entries across the 5 modules (with overlaps): **260**
Unique IDs across all DREFS: **224**

### Examples of IDs inside each DREFS module

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

These DREFS layers are ontology-backed. They are not free labels: they are made of ontology IDs, including imported IDs where needed.

-----

## Theories, biases, effects, and related coded layers

The bot can also work with:

- **theories**
- **biases**
- **effects**
- **paradoxes**
- **decision models**
- **stage models**

These are also coded through ontology-linked resources rather than free narrative labels.

In the loaded theory graph:

- **total coded entries:** **339**

The bot can assign these only after the problem-pattern-normalization layer is already stable.

-----

## APEASE, COM-B, TDF, interventions, and policies

The bot can move from ontology-backed problem mapping into the following frameworks:

- **APEASE**
  Acceptability, Practicability, Effectiveness, Affordability, Side-effects, Equity.
  Fixed output order: **S, E, Eq, P, A, Af**
- **COM-B**
  Capability, Opportunity, Motivation, Behaviour
- **TDF**
  the 14-domain Theoretical Domains Framework layer
- **Intervention functions**
  such as education, training, persuasion, enablement, environmental restructuring, modelling, incentivisation, and others where relevant
- **Policy categories**
  such as guidelines, service provision, communication/marketing, environmental/social planning, and others where relevant

The user may choose these steps in any controlled order, as long as the requests stay grounded in the parent request and its derived outputs.

-----

## How to provide the material

You can provide the material in these forms:

- **uploaded file**
- **pasted text**

**Always include the full case or material in the body of the request.** Do not refer to a previous message or say “the same paper as above.” Each request that starts a new analysis must contain the full source material. The bot works with what is submitted in the current request.

A well-formed request contains three parts:

1. **Source material** — the case, text, paper, or description to be analysed
1. **Task** — what you want the bot to do
1. **Output format** — how you want the result presented

-----

## How to control the output and prevent drift

The more precisely the output format is specified, the less the bot drifts beyond what was requested.

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

## Query design principles and main rule

**Do not try to ask for everything at once.**

Move step by step inside one parent analysis frame. The order of requests depends on the user’s goals — from simple extraction to full intervention design. The request trajectory is not fixed in advance by one rigid script, but it must follow controlled analytical logic.

### Core principles

- **one request = one task**
- each next step should follow from the previous step
- the deeper the workflow, the more important it is to preserve the chain from parent request → intermediate result → next request

### Why this matters

The more tasks are packed into one request:

- the greater the risk of correction loops
- the greater the risk of semantic drift
- the greater the risk of bot hallucination
- the harder it becomes to validate intermediate outputs before moving on

### Recommended starting points

It is acceptable to choose different starting points depending on the goal:

- simple extraction
- explicit problem finding
- hidden problem finding
- ontology ID mapping
- target behaviour analysis
- gap analysis
- DREFS
- COM-B
- TDF
- intervention planning
- APEASE
- full 13-module solution design

-----

## Controlled interaction rule and multi-combinatorics

The **parent request** defines the analysis frame. After that, the user may use **free combinatorics of follow-up requests** over the intermediate results produced from the parent request.

This means the user may:

- refine intermediate outputs
- narrow or broaden a specific result layer
- request re-mapping, re-normalization, or stricter semantic matching
- change formatting or table structure
- ask for alternative cuts of the same already-produced data
- return to an earlier intermediate result and rebuild it
- reformat any table without restarting the analysis
- change column structure, definition language, or label format at any point

This does **not** mean unconstrained free-step navigation. All follow-up requests must remain grounded in the parent request, the source material already accepted into analysis, and the intermediate outputs already derived from that parent request.

In practice, the user controls:

- formatting
- result selection
- granularity
- semantic strictness
- and recombination of already produced intermediate outputs

The bot does not restart from zero on each follow-up. It carries forward the validated analysis space.

-----

## Example workflow with the bot

The exact order depends on the user’s goal. A common deep-analysis route:

### Step 1

`Identify the explicit problems in the case. Format: Table / No. / page no. / problem text.`

**Result**

- **No.**
- **page no.**
- **problem text**

### Step 2

`Now identify the hidden problems. Continue the numbering.`

**Result**

- **No.**
- **page no.**
- **problem text**

### Step 3

`Break all identified problems into behavioural patterns and normalize them in the same step. Format: Table / No. / problem text / pattern / normalization.`

**Result**

- **No.**
- **problem text**
- **pattern**
- **normalization**

### Step 4

`Match each pattern to the best ontology ID using the ontology across all 13 modules available to the bot. Format: Table / No. / text / normalization / ID / label / definition.`

**Result**

- **No.**
- **text**
- **normalization**
- **ID**
- **label**
- **definition**

### Step 5

`Identify the target behaviour. Say whether it was achieved. Show the gap.`

**Result**

- **target behaviour**
- **was it achieved**
- **gap**

### Step 6

`Break the gap down using DREFS ontology IDs. Format: Table / DREFS module / ID / label / gap in this case.`

**Result**

- **DREFS module**
- **ID**
- **label**
- **gap in this case**

### Step 7

`Map the problems to COM-B. Table / No. / Capability / Opportunity / Motivation / Behavior.`

**Result**

- **No.**
- **Capability**
- **Opportunity**
- **Motivation**
- **Behavior**

### Step 8

`Map the problems to TDF. Table / No. / TDF 14 Domain.`

**Result**

- **No.**
- **TDF 14 Domain columns**

### Step 9

`Apply APEASE in fixed order S, E, Eq, P, A, Af. Format: Table / criterion / assessment / verdict / evidence.`

**Result**

- **criterion**
- **assessment**
- **verdict**
- **evidence / rationale**

### Step 10

`Now break the solution down across all 13 modules and their ontology IDs. Format: Table / module / ID / label / case instantiation.`

**Result**

- **module**
- **ID**
- **label**
- **case instantiation in this solution**

-----

## What you can ask at each stage

### 1. Explicit problems

`Identify the explicit problems in the case. Format: Table / No. / page no. / problem text.`

### 2. Hidden problems

`Now identify the hidden problems. Continue the numbering.`

### 3. Patterns and normalization

`Break all identified problems into behavioural patterns and normalize them in the same step. Format: Table / No. / problem text / pattern / normalization.`

### 4. Ontology ID

`Match each pattern to the best ontology ID using the ontology across all 13 modules available to the bot. Format: Table / No. / text / normalization / ID / label / definition.`

### 5. Target behaviour and gap

`Identify the target behaviour. Say whether it was achieved. Show the gap.`

### 6. DREFS

`Break the gap down using DREFS ontology IDs. Format: Table / DREFS module / ID / label / gap in this case.`

### 7. COM-B

`Map the problems to COM-B. Table / No. / Capability / Opportunity / Motivation / Behavior.`

### 8. TDF

`Map the problems to TDF. Table / No. / TDF 14 Domain.`

### 9. APEASE

`Apply APEASE in fixed order S, E, Eq, P, A, Af. Format: Table / criterion / assessment / verdict / evidence.`

### 10. 13 modules

`Now break the solution down across all 13 modules and their ontology IDs. Format: Table / module / ID / label / case instantiation.`

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

## Example requests by task type

The following examples show how different specialists use the bot across different domains. Each example includes the full request and the full bot response exactly as they occurred in a live session.

**Live session reference:** [AISA 11.1 — full test session](https://chatgpt.com/share/69ce02c9-5724-8394-8c03-ac9f9da6650c)

-----

### Example 1 — Semantic normalization from plain language

**Who uses this:** Practitioners, coaches, clinicians who work without ontology knowledge and want to map what they already do to a structured codification.

**Request:**

> I work with patients who keep starting and stopping their exercise routine. In sessions, I help them figure out when exactly they tend to skip the gym and what usually triggers it. Then we come up with a plan for those specific moments — like, if they feel tired after work, they’ll do a 10-minute walk instead of nothing. I also ask them to track how many days they actually exercise each week and bring that to the next session. What ontology terms match what I’m doing? / output table #/pattern/ id /label/ definition

**Bot response:**

I’m matching your description to the closest BCIO terms and will return it in the table format you asked for.

The closest BCIO matches are these. Your description maps most strongly to goal strategising BCT, action planning BCT, and self-monitor behaviour BCT.

|pattern                                                                                                          |id                    |label                     |definition                                                                                                                                                                                                                                             |
|-----------------------------------------------------------------------------------------------------------------|----------------------|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|Helping patients identify when they tend to skip the gym and what triggers it                                    |BCIO:007008.00.17.bc  |goal strategising BCT     |A goal directed BCT in which the person analyses factors influencing the behaviour and generates, selects, or reviews strategies to increase facilitators and overcome barriers.                                                                       |
|Making a plan for those specific moments, such as “if tired after work, do a 10-minute walk instead of nothing”  |BCIO:007010.00.17.bc.f|action planning BCT       |A goal directed BCT that involves making a detailed plan for the performance of the behaviour, which must include at least one of context, frequency, duration or intensity.                                                                           |
|The same “if tired after work…” element can also be read as barrier-specific coping / relapse-prevention planning|BCIO:007008.00.17.bc  |goal strategising BCT     |A goal directed BCT in which the person analyses factors influencing the behaviour and generates, selects, or reviews strategies to increase facilitators and overcome barriers. Synonyms include relapse prevention, coping planning, problem solving.|
|Asking them to track how many days they actually exercise each week                                              |BCIO:007024.00.07.bc.f|self-monitor behaviour BCT|A monitoring BCT in which the person uses a method to monitor and record their behaviour.                                                                                                                                                              |
|The tracking piece also corresponds to the underlying process of self-monitoring                                 |BCIO:006137.00.01.ma  |self-monitoring           |A mechanism-level term used in the ontology for the self-monitoring process; it is linked in the theory layer as a core self-regulation component.                                                                                                     |

A compact summary would be: problem solving / coping planning → action planning → self-monitoring.

If you want, I can turn this into a BCIO-only table with just the final best-match rows.

-----

### Example 2 — COM-B diagnosis and BCW prioritisation

**Who uses this:** Public health programme designers, workplace wellbeing managers, HR specialists, policy consultants.

**Request:**

> Our company wants to reduce sugary drink consumption among 400 office employees in Frankfurt. A baseline survey showed: 78% know that sugar is bad for health (knowledge is not the issue). Vending machines with Coke and juice are on every floor. The canteen sells only sweetened drinks. There are no water coolers in meeting rooms. People report drinking sugary drinks “automatically” during long meetings without thinking. Please run a COM-B diagnosis and identify which BCW interventions to prioritise. Output table 1 #/pattern/ Physical Capability, Psychological Capability, Physical Opportunity, Social Opportunity, Reflective Motivation, Automatic Motivation. Table 2 Table of applicable policies and interventions with priority. Atomically.

**Bot response:**

I’m mapping the case to COM-B first, then turning that into a prioritized BCW intervention and policy table.

Below is a best-effort COM-B diagnosis and a BCW prioritisation for your workplace sugary-drink case. I am treating the strongest deficits as Physical Opportunity and Automatic Motivation, with smaller secondary deficits in Social Opportunity and Reflective Motivation. Psychological Capability is not a primary target, because your baseline already shows knowledge is high, and the APEASE/COM-B guidance says the intervention should first target the deficient COM-B component rather than defaulting to education.

**Table 1**

|#|pattern                                                                         |Physical Capability|Psychological Capability|Physical Opportunity|Social Opportunity|Reflective Motivation|Automatic Motivation|
|-|--------------------------------------------------------------------------------|-------------------|------------------------|--------------------|------------------|---------------------|--------------------|
|1|78% already know sugar is bad for health                                        |Low                |Low                     |—                   |—                 |Low–Medium           |—                   |
|2|Vending machines with Coke and juice on every floor                             |—                  |—                       |High                |—                 |—                    |High                |
|3|Canteen sells only sweetened drinks                                             |—                  |—                       |High                |—                 |Medium               |Medium              |
|4|No water coolers in meeting rooms                                               |—                  |—                       |High                |—                 |—                    |High                |
|5|People drink sugary drinks “automatically” during long meetings without thinking|—                  |Medium                  |Medium              |Medium            |Low                  |Very High           |
|6|Long meetings create default, repeated cue exposure                             |—                  |Medium                  |High                |Medium            |Low                  |High                |
|7|Overall diagnosis priority                                                      |Low                |Low–Medium              |Very High           |Medium            |Low–Medium           |Very High           |

Interpretation: The main problem is not knowledge. The dominant drivers are a cue-rich environment and default product availability, plus habitual or low-awareness drinking in meetings. That fits the COM-B guidance that Automatic Motivation is best addressed with environmental restructuring, restriction, and enablement, while Physical Opportunity is best addressed with environmental restructuring, service provision, and enablement. Social Opportunity is relevant if meeting norms reinforce the behaviour.

**Table 2**

|# |type        |item                         |priority      |why this is applicable atomically                                                                                                                                          |
|--|------------|-----------------------------|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|1 |Intervention|Environmental restructuring  |1 – Highest   |Best match for both Automatic Motivation and Physical Opportunity deficits: change what is available, visible, and easiest in meetings and shared spaces.                  |
|2 |Intervention|Restriction                  |2 – Highest   |Strong fit where unwanted behaviour is driven by easy stimulus access; applies to vending stock, canteen defaults, and meeting-room supply rules.                          |
|3 |Intervention|Enablement                   |3 – High      |Needed to convert good intentions into action through practical supports such as defaults, pre-order systems, meeting prompts, and if-then plans.                          |
|4 |Intervention|Modelling                    |4 – Medium    |Useful if meetings and teams have social norms around sugary drinks; can shift descriptive norms through managers and team leads using water visibly.                      |
|5 |Intervention|Persuasion                   |5 – Medium    |Secondary only. Can help with immediate-consequence framing and norm messaging, but should not be the lead because the case is not mainly a knowledge or intention problem.|
|6 |Intervention|Education                    |6 – Low       |Lowest priority because knowledge is already relatively high and COM-B guidance warns against targeting the wrong component.                                               |
|7 |Intervention|Training                     |7 – Low       |Limited role; only useful for micro-skills such as self-monitoring or implementation plans, not as the main strategy.                                                      |
|8 |Intervention|Incentivisation              |8 – Low–Medium|Could support uptake of healthier defaults, but less direct than changing access and cues.                                                                                 |
|9 |Intervention|Coercion                     |9 – Very Low  |Poor fit and likely less acceptable in an office health-promotion context.                                                                                                 |
|10|Policy      |Environmental/social planning|1 – Highest   |Best policy fit for installing water coolers, redesigning meeting rooms, changing vending placement, and changing defaults in the food environment.                        |
|11|Policy      |Guidelines                   |2 – High      |Suitable for internal meeting catering standards, procurement rules, and canteen/vending nutrition standards.                                                              |
|12|Policy      |Service provision            |3 – High      |Supports actual provision of healthier alternatives: water coolers, unsweetened options, refill points, meeting water service.                                             |
|13|Policy      |Communication/marketing      |4 – Medium    |Useful for norm change and launch support, but should support—not replace—environmental changes.                                                                           |
|14|Policy      |Regulation                   |5 – Medium    |Appropriate if the company can set enforceable internal rules on what can be sold or supplied.                                                                             |
|15|Policy      |Fiscal measures              |6 – Low–Medium|Could use internal price incentives, such as subsidising water or unsweetened drinks, but it is not the first lever here.                                                  |
|16|Policy      |Legislation                  |7 – Very Low  |Not a realistic primary route for a single-company office intervention.                                                                                                    |

Priority conclusion: Your best BCW package is Environmental restructuring, Restriction, Enablement, Modelling. Supported by policies: Environmental/social planning, Guidelines, Service provision, Communication/marketing.

A concrete interpretation of that package in your case would be: remove sugary drinks from default access points, add water everywhere people make the choice, set meeting and canteen standards, make water the easiest option, and visibly shift team norms. That is much more aligned to the diagnosed COM-B deficits than another awareness campaign.

-----

### Example 3A — Post-factum APEASE gap analysis

**Who uses this:** Systematic reviewers, public health analysts, NHS staff evaluating evidence before recommending scale-up.

**Request:**

> I’m reviewing a published RCT of a smoking cessation SMS intervention delivered to adults via mobile phones in New Zealand (N=1200, 6-week programme, automated texts with goal-setting and motivational prompts). The paper reports: 12-week quit rate 18% vs 9% control (p<0.01). No cost data reported. No adverse events section. Participants were self-selected from an online sign-up. No breakdown by ethnicity, income, or deprivation index. Please conduct a post-factum APEASE gap analysis of this paper.

**Bot response:**

I’m applying APEASE in post-factum mode and will mark the paper’s evidence gaps criterion by criterion from the details you provided.

Here is a post-factum APEASE gap analysis based only on the details you provided, using the published-study protocol where missing data must be marked as not assessed rather than assumed favorable. In this mode, a paper is judged not just on whether it shows an effect, but on whether it reports enough to support acceptability, practicability, effectiveness, affordability, spill-over, and equity.

**APEASE gap analysis**

|Criterion                |Evidence from your summary                                                                                                   |Status                     |Gap judgement                                                                                                                                                                                                                                                                                                          |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|Acceptability            |Participants were self-selected via online sign-up. No refusal rate, no stakeholder data, no acceptability measures reported.|Partially assessed, weakly |Recruitment shows some willingness among volunteers, but this is not enough for APEASE acceptability because the protocol expects recruitment/refusal information and, ideally, evidence beyond the target group alone.                                                                                                |
|Practicability           |SMS delivery to adults via mobile phones; automated texts; 6-week programme.                                                 |Partially assessed         |The intervention appears logistically feasible in trial conditions, but the paper summary does not report routine delivery constraints, implementation burden, staffing oversight, or sustainability under real-world service conditions. APEASE treats sustainability in routine conditions as part of practicability.|
|Effectiveness            |12-week quit rate 18% vs 9% control, p<0.01.                                                                                 |Assessed                   |This is the strongest reported dimension. The trial reports a positive short-term effect signal. But APEASE also stresses external validity, and effect evidence alone does not compensate for missing affordability, spill-over, and equity information.                                                              |
|Affordability            |No cost data reported.                                                                                                       |Not assessed               |Direct APEASE gap. Without intervention cost, cost per quitter, delivery budget, or cost of inaction comparison, affordability cannot be judged. Missing affordability is named as a common post-factum literature gap.                                                                                                |
|Spill-over / Side-effects|No adverse events section. No positive spill-over reported.                                                                  |Not assessed               |APEASE requires attention to spill-over, not just harms. With no adverse-events section and no broader spill-over reporting, neither negative nor positive spill-over is assessable. The framework explicitly warns that absence of reporting is a gap, not evidence of safety.                                        |
|Equity                   |No breakdown by ethnicity, income, or deprivation index. Participants were self-selected online.                             |Not assessed / critical gap|This is a major APEASE weakness. The framework expects stratification by subpopulation and explicitly lists socioeconomic, ethnic, and related axes. Because the sample is self-selected online and no subgroup breakdown is given, the study cannot show whether the intervention reduces or widens inequalities.     |

**Overall profile**

|Criterion                |Verdict              |
|-------------------------|---------------------|
|Acceptability            |Weak partial evidence|
|Practicability           |Partial evidence     |
|Effectiveness            |Present              |
|Affordability            |Missing              |
|Spill-over / Side-effects|Missing              |
|Equity                   |Missing, critical    |

**Methodological verdict:** Under the post-factum APEASE protocol, this paper would be judged limited, not because the RCT failed on effectiveness, but because it leaves several decision-critical dimensions unassessed. The framework’s verdict rule is that studies with critical gaps in spill-over, equity, and affordability have limited external validity and should not be used to justify scale-up without further evidence. That matches this case closely.

|What it supports                                                                        |What it does not support                                                                        |
|----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|
|The intervention likely improves short-term quitting at 12 weeks under trial conditions.|Whether the programme is affordable at service scale.                                           |
|Automated SMS delivery over 6 weeks is at least operationally possible within an RCT.   |Whether it is practical and sustainable in routine conditions outside the trial.                |
|Some self-selected adults were willing to enrol online.                                 |Whether the intervention is acceptable across the wider target population or other stakeholders.|
|                                                                                        |Whether harms, unintended consequences, or positive spill-overs occurred.                       |
|                                                                                        |Whether outcomes are equitable across ethnicity, income, or deprivation groups.                 |

**Bottom line:** Effectiveness-positive but decision-incomplete. This RCT provides a useful efficacy signal, but from an APEASE perspective it is not sufficient for confident scale-up or policy adoption because Affordability, Spill-over/Side-effects, and Equity are not assessed, while Acceptability and Practicability are only partially evidenced.

-----

### Example 3B — Scale-up verdict

**Who uses this:** Same as 3A — run after 3A when the paper makes a scale-up claim.

**Request:**

> Same paper as above. The authors conclude the intervention is “ready for national scale-up.” Based on your APEASE gap analysis, is this conclusion methodologically justified? Please provide a verdict on external validity and explain which gaps directly undermine the scale-up claim.

**Bot response:**

No. On the information provided, the claim that the intervention is “ready for national scale-up” is not methodologically justified.

My verdict on external validity is: **insufficient / not established.** The RCT gives a credible short-term effectiveness signal at 12 weeks, but APEASE is explicit that scale-up decisions require more than efficacy alone. A national scale-up claim needs evidence that the intervention is acceptable, practical, affordable, safe in terms of adverse or spill-over effects, and equitable across relevant subgroups. Those requirements are not met here.

**Verdict table**

|question                                             |verdict                      |why                                                                                                                                   |
|-----------------------------------------------------|-----------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
|Is the intervention effective under trial conditions?|Yes, provisionally           |The reported quit-rate difference supports short-term efficacy.                                                                       |
|Is the paper sufficient to support national scale-up?|No                           |Key APEASE dimensions required for scale-up are missing or only weakly assessed.                                                      |
|External validity                                    |Limited / decision-inadequate|The evidence does not show that results generalise fairly, feasibly, and safely to the national target population and service context.|

**Which gaps directly undermine the scale-up claim**

|gap                                    |why it matters for scale-up                                                                                                                                                                                                                                                                  |
|---------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|Affordability missing                  |National scale-up is a resource commitment. With no cost data, cost per quitter, delivery budget, or affordability analysis, there is no basis for judging whether rollout is financially viable. Under APEASE, missing affordability directly blocks a scale-up conclusion.                 |
|Spill-over / adverse effects missing   |A national programme should not be declared ready without evidence on harms, unintended consequences, or broader spill-over. No adverse-events section means safety and wider system effects are unassessed, not proven absent.                                                              |
|Equity missing                         |No breakdown by ethnicity, income, or deprivation is especially serious for a national smoking-cessation claim. Without subgroup evidence, the paper cannot show whether scale-up would reduce inequalities, have neutral effects, or worsen them. APEASE treats this as a core decision gap.|
|Self-selected online sample            |Recruitment through online self-sign-up weakens representativeness. It supports uptake among motivated volunteers, not necessarily among the broader smoking population reached in national rollout. That limits generalisability and therefore external validity.                           |
|Acceptability only weakly evidenced    |Enrolment shows some willingness to participate, but there are no refusal data, dropout acceptability interpretation, or stakeholder acceptability measures. Scale-up needs stronger evidence that the intervention is acceptable in routine practice, not just to trial volunteers.         |
|Practicability only partially evidenced|Automated SMS delivery looks feasible in an RCT, but the paper summary gives no evidence on routine implementation capacity, integration into services, operational burden, or sustainability at national level. That leaves the real-world delivery case incomplete.                        |

**Bottom-line judgement:** A more defensible conclusion would be: “The trial provides encouraging short-term evidence of effectiveness, but further evidence is needed before claiming readiness for national scale-up, especially on affordability, spill-over/adverse effects, equity, acceptability, and routine practicability.” The authors’ stronger statement is methodologically overstated. It extrapolates from efficacy in a selective trial sample to national implementation readiness without the supporting APEASE evidence needed to justify that jump.

-----

### Example 4 — Full CASE_TO_DESIGN_ROUTE

**Who uses this:** Intervention designers, NHS programme leads, public health teams building structured evidence-based interventions from a diagnosed case.

**Request:**

> Case: NHS hospital trust, Manchester. Target behaviour: hand hygiene compliance among healthcare assistants (HCAs). Baseline compliance: 35%. Known context: HCAs report they know the protocol. Observations show sinks are often far from patient beds. Staff report “I don’t always have time” and “nobody checks anyway.” Senior nurses rarely model hand hygiene themselves. Please run CASE_TO_DESIGN_ROUTE: Stage 1 (Diagnosis): use Source, Behaviour, Setting, MoA modules. Stage 2 (Prognosis): use BCT, Mode of Delivery, Population modules. Stage 3: APEASE evaluation in fixed order. Stage 4: policy and intervention priorities. Stage 5: TDF × COM-B priority matrix.

**Bot response:**

I’m working stage by stage and will keep it tables-first as requested, starting with the diagnosis modules you specified and carrying the conclusions forward into the design stages.

The case points to a mixed deficit pattern rather than a knowledge problem: Physical Opportunity is the clearest barrier because sinks are far from beds and staff report lack of time; Social Opportunity is also weak because “nobody checks anyway” and senior nurses do not model the target behaviour; Automatic Motivation is implicated because non-compliance appears routine under time pressure; and Reflective Motivation is weakened by low accountability.

**Stage 1 — Diagnosis**

**1A. Source module**

|pattern                                |closest ontology term                                    |status          |why it matters diagnostically                                                                                                                          |
|---------------------------------------|---------------------------------------------------------|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|HCAs                                   |BCIO:010074.00.03.so — health care assistant             |explicit        |This is the closest source/population-role match for HCAs.                                                                                             |
|Senior nurses rarely model hand hygiene|BCIO:010012.00.13.so — nursing and midwifery professional|implicit        |Senior nurses function as higher-status reference models, so their behaviour likely shapes norms and expectations.                                     |
|“Nobody checks anyway”                 |BCIO:010128.00.12.so — supervision of person source      |implicit deficit|The files define supervision of person source as corrective and skill-enhancing feedback on performance. Your case signals that this is weak or absent.|

**1B. Behaviour module**

|pattern                |closest ontology term                   |status  |diagnostic conclusion                                                                                                  |
|-----------------------|----------------------------------------|--------|-----------------------------------------------------------------------------------------------------------------------|
|Hand hygiene compliance|BCIO:050369.00.01.be.ma.sd — handwashing|explicit|The target behaviour is directly grounded as handwashing. Baseline compliance of 35% indicates a large performance gap.|
|Low baseline compliance|behavioural frequency / compliance gap  |implicit|The problem is not target ambiguity but insufficient enactment of a known behaviour.                                   |

**1C. Setting module**

|pattern                           |closest ontology term          |status                                             |diagnostic conclusion                                                   |
|----------------------------------|-------------------------------|---------------------------------------------------|------------------------------------------------------------------------|
|NHS hospital trust                |hospital / health care facility|implicit                                           |The setting is clearly a healthcare facility.                           |
|Sinks far from patient beds       |bedside physical environment   |explicit case signal, ontology term underspecified |The case strongly indicates a physical-setting barrier at point of care.|
|No one checks, nurses do not model|social environment of ward/team|explicit case signal, ontology term partly implicit|The ward social setting currently permits non-compliance.               |

**1D. Mechanisms of Action module**

|pattern                        |closest ontology term                                                          |status          |module conclusion                                                                                      |
|-------------------------------|-------------------------------------------------------------------------------|----------------|-------------------------------------------------------------------------------------------------------|
|Sinks far from beds / no time  |BCIO:006089.03.04.ma.r — physical behavioural opportunity                      |explicit        |Main barrier: the environment and workflow make the behaviour harder to perform at the required moment.|
|Nobody checks / poor modelling |BCIO:050702.00.03.ma — low social opportunity to enact a behaviour             |implicit        |Weak accountability and weak role-modelling create a permissive norm.                                  |
|Nurses rarely model behaviour  |BCIO:050776.00.01.ma — social influence process                                |implicit        |Senior staff behaviour likely influences HCA behaviour through social influence.                       |
|Nobody checks anyway           |BCIO:050979.00.10.ma — mechanism of action through feedback process to a person|implicit deficit|The case implies insufficient feedback/monitoring loops.                                               |
|Routine omission under pressure|habit-like / automatic responding                                              |implicit        |In COM-B terms this fits Automatic Motivation.                                                         |

**Stage 2 — Prognosis**

**2A. BCT module**

|proposed BCT                                     |ontology grounding                                                      |priority|why it fits Stage 1                                                                 |
|-------------------------------------------------|------------------------------------------------------------------------|--------|------------------------------------------------------------------------------------|
|Place alcohol gel / hand-rub at point of care    |BCIO:050348.05.01.bc — restructure the physical environment BCT         |1       |Best fit to the sink-distance barrier.                                              |
|Add dispensers/signage where action occurs       |BCIO:007151.00.04.bc — directly restructure the physical environment BCT|2       |Changes the environment at the exact place and time of behaviour.                   |
|Bedside prompts / visual reminders               |BCIO:007080.00.06.bc.f — prompt intended action BCT                     |3       |Prompts are recommended when intention exists but execution fails in the moment.    |
|Ward-level audit and rapid feedback              |BCIO:007022.03.07.bc — provide feedback BCT                             |4       |Directly addresses “nobody checks anyway.”                                          |
|Personal or shift-level self-monitoring          |BCIO:007024.00.07.bc.f — self-monitor behaviour BCT                     |5       |Supports behavioural regulation and keeps the target salient.                       |
|Change team norm and visible expectations        |BCIO:050349.02.01.bc — restructure the social environment BCT           |6       |Fits the norm/modelling problem.                                                    |
|Senior nurses present directive/normative message|BCIO:007075.00.05.bc — present information from credible influence BCT  |7       |Useful because the source is socially credible, but secondary to environment change.|

**2B. Mode of Delivery module**

|proposed mode                          |ontology grounding                                                     |priority|use in design                                                                           |
|---------------------------------------|-----------------------------------------------------------------------|--------|----------------------------------------------------------------------------------------|
|Brief huddle / bedside coaching        |BCIO:011003.00.01.md — face to face mode of delivery                   |1       |Best for norm-setting, modelling, and accountability.                                   |
|Posters / notices at sinks, bays, rooms|BCIO:011007.00.03.md — public notice mode of delivery                  |2       |Best for prompts at point of action.                                                    |
|Electronic environmental object mode   |BCIO:011016.00.16.md — electronic environmental object mode of delivery|3       |Strong fit if the trust uses smart dispensers or environmental reminder devices.        |
|Digital dashboards / e-feedback        |BCIO:011010.17.06.md — electronic mode of delivery                     |4       |Good for feedback loops, but should support rather than replace face-to-face leadership.|

**2C. Population module**

|population target            |ontology grounding                                       |priority|rationale                                                                   |
|-----------------------------|---------------------------------------------------------|--------|----------------------------------------------------------------------------|
|HCAs                         |BCIO:010074.00.03.so — health care assistant             |1       |Primary behaviour enacters.                                                 |
|Senior nurses / nurse leaders|BCIO:010012.00.13.so — nursing and midwifery professional|2       |Key modelling and accountability source group.                              |
|Whole ward team              |team-level social environment                            |implicit|Needed because social opportunity and norm contagion operate at group level.|

**Stage 3 — APEASE in fixed order**

|criterion         |verdict                                 |reason                                                                                                                                                                               |status               |
|------------------|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------|
|S — Spill-over    |favourable                              |Better hand hygiene should have positive spill-over for infection prevention culture and adjacent safety behaviours.                                                                 |positive with caution|
|E — Effectiveness |high for restructuring/modelling package|COM-B guidance says physical and social opportunity deficits should be matched first; that strongly supports environmental restructuring, service provision, modelling, and feedback.|strong               |
|Eq — Equity       |favourable                              |Environmental solutions usually have stronger equity profiles than knowledge-heavy solutions because they reduce reliance on memory, motivation, and individual bandwidth.           |favourable           |
|P — Practicability|moderate to high                        |Posters, huddles, feedback, and nurse modelling are practical; moving sinks is harder, but bedside dispensers and local workflow redesign are practical substitutes.                 |moderate-high        |
|A — Acceptability |moderate to high if non-punitive        |Modelling and environmental restructuring are usually acceptable; intensive policing may reduce acceptability, so feedback should be developmental rather than purely punitive.      |conditional          |
|Af — Affordability|moderate                                |Posters, prompts, huddles, and audit are relatively low cost; infrastructure changes cost more, but environmental restructuring often has good longer-run ROI.                       |moderate             |

**Stage 4 — Policy and intervention priorities**

|rank|intervention function      |priority  |atomic reason                                                                         |
|----|---------------------------|----------|--------------------------------------------------------------------------------------|
|1   |Environmental restructuring|highest   |Main answer to sink-distance and point-of-care friction.                              |
|2   |Modelling                  |highest   |Direct answer to senior nurses not modelling the target behaviour.                    |
|3   |Enablement                 |high      |Needed for prompts, workflow support, and implementation under time pressure.         |
|4   |Training                   |medium    |Only as a supplement for technique and workflow, not the lead strategy.               |
|5   |Persuasion                 |medium-low|Secondary; helps with accountability and immediate consequences, but not enough alone.|
|6   |Education                  |low       |Lowest priority because protocol knowledge is already present.                        |

|rank|policy                       |priority|atomic reason                                                                                                          |
|----|-----------------------------|--------|-----------------------------------------------------------------------------------------------------------------------|
|1   |Guidelines                   |highest |Best lever for making point-of-care hand hygiene, observation, and feedback explicit ward standards.                   |
|2   |Environmental/social planning|highest |Needed to redesign the bedside environment and ward workflow.                                                          |
|3   |Service provision            |high    |Covers supplying bedside gel, dispensers, observation tools, and audit systems.                                        |
|4   |Communication/marketing      |medium  |Useful for posters, ward norm campaigns, and public commitment, but not enough by itself.                              |
|5   |Regulation                   |medium  |Useful internally if the trust formalises accountability, but should not be the starting point without support systems.|

**Stage 5 — TDF × COM-B priority matrix**

|TDF domain                               |COM-B component                           |priority|case evidence                                                               |design implication                                                      |
|-----------------------------------------|------------------------------------------|--------|----------------------------------------------------------------------------|------------------------------------------------------------------------|
|D11 Environmental context and resources  |Physical Opportunity                      |1       |sinks far from beds; “I don’t always have time”                             |environmental restructuring, service provision, workflow redesign       |
|D12 Social influences                    |Social Opportunity                        |2       |nobody checks; senior nurses rarely model                                   |modelling, social environment restructuring, feedback                   |
|D7 Reinforcement                         |Automatic Motivation                      |3       |skipping is easier and unpunished; no immediate reinforcement for compliance|create immediate feedback and visible reinforcement for target behaviour|
|D10 Memory, attention, decision processes|Psychological Capability                  |4       |under pressure, hand hygiene can be missed at the moment of action          |prompts, checklists, bedside cues                                       |
|D14 Behavioural regulation               |Psychological Capability                  |5       |no checking means weak self-regulation and weak monitoring                  |self-monitoring + audit and feedback                                    |
|D3 Social/professional identity          |Social Opportunity + Reflective Motivation|6       |senior staff do not enact “people like us always clean hands”               |identity-relevant modelling and leadership expectations                 |
|D1 Knowledge                             |Psychological Capability                  |low     |HCAs report they know the protocol                                          |do not lead with education                                              |

**Bottom-line design verdict:** The route points to a system-first hand hygiene design, not a knowledge campaign. The best-fit package is: 1. restructure the physical environment, 2. reset the social environment through visible nurse modelling, 3. install observation/feedback/accountability, 4. add bedside prompts and simple self-monitoring.

-----

### Example 5 — Gap detection in an intervention design

**Who uses this:** Researchers, digital health developers, programme evaluators identifying what is missing before running an intervention.

**Request:**

> We’re running a 4-week online alcohol reduction programme for university students in Melbourne. The programme includes: weekly emails with information about alcohol-related health risks, a downloadable PDF with tips for reducing drinking, a one-time goal-setting exercise at enrolment, and a final quiz to test knowledge. We measure outcomes with a self-reported alcohol units questionnaire at week 4. We expect to reach 1,500 students via email. Please identify what is missing in this intervention design from an ontology perspective.

**Bot response:**

I’m checking the design against the ontology’s intervention-description layers and will flag the main missing elements rather than just critiquing content.

From a BCIO-style ontology perspective, the design is partly specified but structurally incomplete. You have some coverage of Mode of Delivery, BCT, Duration, Population, Behaviour, and Outcome measurement, but several important intervention-description layers are missing or underspecified. The biggest gaps are Mechanisms of Action, Engagement, Fidelity, Setting, Source, and actual Reach.

**What is present**

|module            |what is present                                                                                     |
|------------------|----------------------------------------------------------------------------------------------------|
|Behaviour         |Alcohol reduction is the target behaviour.                                                          |
|Mode of Delivery  |Weekly emails are a clear email mode of delivery; the PDF is a downloadable informational component.|
|BCT               |A one-time goal-setting exercise gives at least one explicit BCT component.                         |
|Dose / Schedule   |A 4-week programme with weekly emails gives a basic schedule, but only at a coarse level.           |
|Population        |University students in Melbourne are identified at a broad level.                                   |
|Evaluation finding|Week-4 self-reported alcohol units questionnaire is specified as the main outcome measure.          |

**What is missing**

|missing ontology layer           |what is missing in your design                                                                                                                                                              |why it matters                                                                                                                                                                             |
|---------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|Mechanisms of Action (MoA)       |No explicit statement of how the intervention is supposed to reduce drinking. The implicit logic is “knowledge → lower drinking,” but that pathway is not specified or justified.           |Without MoA, the design does not explain whether it is targeting risk appraisal, self-regulation, goal importance, self-monitoring, social identity, habit interruption, or something else.|
|Engagement                       |No plan for whether students will actually open the emails, download the PDF, complete the goal-setting exercise, or finish the quiz.                                                       |In the ontology, engagement is a core module, not an afterthought. Missing engagement data makes it hard to interpret any null or weak effect.                                             |
|Fidelity                         |No specification of whether the intervention will be delivered as intended: Were all 4 emails sent? Did students receive them? Was the PDF accessible? Was goal-setting completed correctly?|Fidelity is critical because weak delivery can be mistaken for an ineffective intervention.                                                                                                |
|Source                           |The intervention source is unclear. Who is sending the emails: university health service, student wellbeing team, researchers, an automated platform, or a named professional?              |Source affects credibility, trust, uptake, and interpretation of the content.                                                                                                              |
|Setting                          |“Online” is too broad. There is no real description of the behavioural setting in which drinking happens or in which the intervention is received.                                          |Setting matters because student drinking is often social, event-based, and context-triggered.                                                                                              |
|Dose precision                   |You state weekly emails for 4 weeks, but not message length, number of prompts per week, PDF use expectations, quiz burden, or intensity of exposure.                                       |BCIO treats dose as the amount of intervention delivered. Your design has only a minimal dose description.                                                                                 |
|Reach as actual, not expected    |“We expect to reach 1,500 students via email” is not the same as actual reach.                                                                                                              |The ontology distinguishes intervention reach from intended audience size.                                                                                                                 |
|Population stratification        |“University students in Melbourne” is broad, but there is no breakdown by age, year level, residence, drinking risk, international/domestic status, or subgroup barriers.                   |Missing subgroup structure weakens interpretation of both reach and equity.                                                                                                                |
|Behavioural regulation components|There is goal setting once at enrolment, but no self-monitoring, action planning refresh, coping planning, or feedback during the 4 weeks.                                                  |From an ontology/BCT perspective, the design is thin on self-regulation support beyond initial goal-setting.                                                                               |
|Tailoring / adaptation           |No tailoring by baseline drinking pattern, motivation, risk level, or engagement level.                                                                                                     |A single static content stream may fit poorly across student subgroups and drinking contexts.                                                                                              |
|Longer-term follow-up            |Only week-4 self-report is planned.                                                                                                                                                         |That gives a very narrow outcome window and tells you little about maintenance.                                                                                                            |

**Bottom line:** The main ontology-level problem is that the programme is currently described mostly as content delivery rather than as a fully specified behaviour change intervention scenario. What is most clearly missing is: 1. explicit mechanism of action, 2. engagement specification and measurement, 3. fidelity specification, 4. clearer source and setting description, 5. fuller dose description, 6. actual reach data, 7. stronger self-regulation components beyond one-time goal setting.

-----

### Example 6 — Cross-intervention comparison with APEASE and equity analysis

**Who uses this:** Commissioners, local authorities, municipalities, public health consultants choosing between intervention options for a defined population.

**Request:**

> A Dutch municipality is choosing between two physical activity interventions for adults aged 65+ in low-income neighbourhoods. Option A: Group walking programme, delivered by trained peer volunteers, 3×/week in local parks, with monthly goal-review sessions and a buddy system. Option B: Personalised digital coaching app with step-count tracking, automated motivational messages, and weekly AI-generated feedback reports. Delivered via smartphone. Compare both options using the following modules: Source, Mode of Delivery, Setting, BCT, Population. For each module produce one table with columns: # / Option / Pattern / ID / Label / Definition. ID must be the exact ontology identifier. Label must follow format: English label (Dutch label). Definition in Dutch. After all module tables, apply APEASE in fixed order S, E, Eq, P, A, Af and recommend which option is more appropriate for this population and context. Highlight any equity risks.

**Bot response:**

Below I use the closest exact ontology terms I could ground for each module. For Option B / Source, BCIO does not appear to provide a dedicated source term for “AI coach” as such, so I map it to the closest exact source construct: organisational source role. That is the weakest mapping in the set; the others are stronger.

**Source**

|#|Option|Pattern                          |ID                  |Label                                                                       |Definition                                                                                                                                                                                         |
|-|------|---------------------------------|--------------------|----------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|1|A     |trained peer volunteers          |BCIO:010104.00.05.so|peer (gelijke)                                                              |Een relatie waarbij de bron wordt beschreven als passend bij ontvangers op basis van ‘gelijkheid’ — bijvoorbeeld leeftijd, sociale status, gender, gedeelde ervaring of gedeelde gezondheidsstatus.|
|2|A     |volunteer delivery role          |BCIO:010133.00.03.so|source role related to intervention (bronrol gerelateerd aan de interventie)|Een BCI-bron waarvan de beroepsmatige of vrijwillige rol gericht is op het leveren van de gedragsveranderingsinterventie.                                                                          |
|3|B     |app/platform as delivering source|BCIO:010138.00.03.so|organisational source role (organisatorische bronrol)                       |Een BCI-bronrol die door een organisatie wordt gedragen.                                                                                                                                           |

**Mode of Delivery**

|#|Option|Pattern                          |ID                  |Label                                                                        |Definition                                                                                                                                         |
|-|------|---------------------------------|--------------------|-----------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
|1|A     |face-to-face walking sessions    |BCIO:011003.00.01.md|face to face mode of delivery (face-to-face wijze van levering)              |Een menselijke interactionele wijze van levering waarbij een interventiebron en ontvanger zich op dezelfde locatie bevinden en direct communiceren.|
|2|A     |group walks                      |BCIO:011057.00.14.md|group-based mode of delivery (groepsgebaseerde wijze van levering)           |Een wijze van levering waarbij drie of meer mensen aanwezig zijn op de locatie waar de interventie wordt geleverd.                                 |
|3|B     |smartphone app                   |BCIO:011028.00.16.md|mobile application mode of delivery (mobiele-app wijze van levering)         |Een elektronische wijze van levering waarbij de ontvanger van de interventie interacteert met een mobiele applicatie.                              |
|4|B     |personalised app for one user    |BCIO:011055.00.14.md|individual-based mode of delivery (individueel gebaseerde wijze van levering)|Een wijze van levering waarbij één ontvanger betrokken is op de locatie waar de interventie wordt geleverd.                                        |
|5|B     |automated weekly reports/messages|BCIO:011061.00.14.md|asynchronous mode of delivery (asynchrone wijze van levering)                |Een wijze van levering waarbij ontvangst van de interventie plaatsvindt geruime tijd nadat deze zijn verzonden of geleverd.                        |

**Setting**

|#|Option|Pattern                  |ID                                        |Label                               |Definition                                                                                                                                                                   |
|-|------|-------------------------|------------------------------------------|------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|1|A     |local parks              |ENVO:00000562.00.00.bc.be.p.ma.md.se.so.sd|park (park)                         |Een afgebakend gebied van land of water, meestal in natuurlijke of halfnatuurlijke aangelegde staat, dat voor een bepaald doel is bestemd, meestal recreatie of natuurbehoud.|
|2|A     |low-income neighbourhoods|BCIO:026005.00.01.se                      |low-income area (laaginkomensgebied)|Een sociaal-economische gebiedsconditie die wordt beschreven als laag inkomen, op landelijk of binnenlands niveau.                                                           |
|3|B     |low-income neighbourhoods|BCIO:026005.00.01.se                      |low-income area (laaginkomensgebied)|Een sociaal-economische gebiedsconditie die wordt beschreven als laag inkomen, op landelijk of binnenlands niveau.                                                           |

**BCT**

|#|Option|Pattern                                  |ID                    |Label                                                                                        |Definition                                                                                                                           |
|-|------|-----------------------------------------|----------------------|---------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
|1|A     |buddy system                             |BCIO:007028.03.19.bc  |social support BCT (sociale-steun-BCT)                                                       |Een gedragsveranderingstechniek waarbij stappen worden gezet om steun of hulp van een andere persoon veilig te stellen of te leveren.|
|2|A     |monthly goal-review sessions             |BCIO:007011.00.17.bc  |review behaviour goal BCT (gedragsdoel herzien-BCT)                                          |Een doelgerichte BCT die verwijst naar het herzien van een gedragsdoel.                                                              |
|3|B     |step-count tracking                      |BCIO:007025.00.07.bc.f|self-monitor outcome of behaviour BCT (uitkomst van gedrag zelf monitoren-BCT)               |Een monitoring-BCT waarbij de persoon een methode gebruikt om een uitkomst van zijn of haar gedrag te monitoren en vast te leggen.   |
|4|B     |weekly AI-generated feedback reports     |BCIO:007027.00.02.bc  |provide feedback on outcome of behaviour BCT (feedback geven over de uitkomst van gedrag-BCT)|Een feedback-BCT waarbij informatie wordt gegeven over een uitkomst van iemands eerdere uitvoering van het gedrag.                   |
|5|B     |personalised coaching prompt around goals|BCIO:007003.01.03.bc  |set behaviour goal BCT (gedragsdoel stellen-BCT)                                             |Een BCT die verwijst naar het stellen van een gedragsdoel binnen goal setting BCT.                                                   |

**Population**

|#|Option|Pattern                             |ID                                          |Label                                 |Definition                                                                                                           |
|-|------|------------------------------------|--------------------------------------------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------|
|1|A     |adults 65+                          |ADDICTO:0000352.00.20.bc.be.p.ma.md.se.so.sd|adult (volwassene)                    |Volwassene; dit is de dichtstbijzijnde exacte populatieterm die in de geladen ontologie zichtbaar is voor deze casus.|
|2|A     |many 65+ participants may be retired|BCIO:015067.00.31.be.p.ma.sd                |retired status (gepensioneerde status)|Een persoonlijk kenmerk dat aanwezig is bij een persoon die zich uit het arbeidsproces heeft teruggetrokken.         |
|3|B     |adults 65+                          |ADDICTO:0000352.00.20.bc.be.p.ma.md.se.so.sd|adult (volwassene)                    |Volwassene; dit is de dichtstbijzijnde exacte populatieterm die in de geladen ontologie zichtbaar is voor deze casus.|
|4|B     |many 65+ participants may be retired|BCIO:015067.00.31.be.p.ma.sd                |retired status (gepensioneerde status)|Een persoonlijk kenmerk dat aanwezig is bij een persoon die zich uit het arbeidsproces heeft teruggetrokken.         |

**APEASE in fixed order: S, E, Eq, P, A, Af**

|Criterion         |Option A                                                                                                                      |Option B                                                                                                                  |Verdict                                                |
|------------------|------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------|
|S — Spill-over    |Positieve neveneffecten waarschijnlijk: sociale verbondenheid, minder eenzaamheid, meer buurtcohesie.                         |Mogelijke positieve neveneffecten via zelfinzicht, maar ook risico op frustratie, digitale overbelasting of snelle uitval.|A sterker                                              |
|E — Effectiveness |Goede match met deze populatie: peerbron, buddy-systeem, vaste routine, lokaal park en doelherziening ondersteunen volhouden. |Kan effectief zijn voor digitaal vaardige deelnemers, maar effect hangt sterk af van smartphonegebruik en app-engagement. |A waarschijnlijk effectiever in context                |
|Eq — Equity       |Sterker equity-profiel zolang deelname gratis is en parken lokaal/toegankelijk zijn. Minder afhankelijk van digitale middelen.|Belangrijk equity-risico: smartphonebezit, databundel, digitale vaardigheden, visuele/cognitieve belasting, AI-fit.       |A duidelijk beter                                      |
|P — Practicability|Vereist coördinatie van vrijwilligers en weers-/mobiliteitsplanning, maar gebruik door deelnemers is eenvoudig.               |Schaalbaar technisch, maar praktische inzet in deze populatie hangt af van onboarding, support en blijvend gebruik.       |A beter voor deelnemers; B beter voor distributieschaal|
|A — Acceptability |Waarschijnlijk hoog door menselijke steun, sociale interactie en vertrouwde buurtcontext.                                     |Gemengd: sommigen waarderen autonomie, anderen ervaren appgebruik als lastig of onpersoonlijk.                            |A sterker                                              |
|Af — Affordability|Door peer-vrijwilligers waarschijnlijk betaalbaar, al zijn coördinatie en begeleiding nodig.                                  |App kan op schaal efficiënt lijken, maar ontwikkeling, onderhoud, support en digitale inclusiekosten tellen mee.          |Licht voordeel voor A in deze context                  |

**Recommendation:** Option A.

Why: Source: peer delivery is a better fit than an impersonal organisational/app source for older adults in low-income neighbourhoods. Mode of Delivery: face-to-face and group-based delivery reduces digital exclusion. Setting: local parks plus low-income neighbourhood delivery fit the lived context better than app-only delivery. BCT mix: social support plus goal review is likely to be more robust for this population than relying mainly on self-monitoring and automated feedback.

**Equity risks by option**

Option A:

- Mobiliteits- of frailty-verschillen kunnen sommige 65+ deelnemers uitsluiten.
- Parkveiligheid, bankjes, toiletten, verlichting en weersomstandigheden kunnen ongelijk verdeeld zijn tussen buurten.
- Kwaliteit en continuïteit van peer-vrijwilligers kunnen per wijk verschillen.
- Groepsformaten kunnen minder aantrekkelijk zijn voor sociaal angstige of cultureel minder aangesloten deelnemers.

Option B:

- Digitale kloof: niet iedereen heeft een geschikte smartphone, databundel of laadmogelijkheid.
- Lagere digitale geletterdheid bij een deel van de 65+ groep kan deelname en volhouden beperken.
- Visuele, cognitieve of motorische beperkingen kunnen appgebruik bemoeilijken.
- AI-feedback kan onpersoonlijk, onduidelijk of cultureel minder passend zijn.
- Groot risico dat vooral de fittere, rijkere en digitaal vaardigere subgroep profiteert, waardoor ongelijkheid toeneemt.

**Bottom line:** For adults 65+ in low-income neighbourhoods, Option A is the more appropriate and equitable choice. Option B is best treated, at most, as an optional add-on for digitally confident participants rather than the primary intervention.

-----

### Example 7 — 13-module coverage of a technical deployment paper (any domain)

**Who uses this:** Researchers, analysts, and programme evaluators who want to map any study — including non-behavioural or technical papers — to the 13-module ontology structure with full ID codification.

**This example demonstrates that the bot works with any domain where there is behaviour, not only health interventions.**

**Case:** Real-time retail planogram compliance application using computer vision and virtual shelves. Published in Scientific Reports (2025).

**Direct link to paper:** https://www.nature.com/articles/s41598-025-27773-5.pdf

**Live session with full bot output:** https://chatgpt.com/share/69ce2385-8ed4-8397-bbdd-36750f9ddd97

-----

**Request 1:**

> Organize the case into 13 ontology modules. Identify modules on each page; don’t jump between pages. Identify modules in the order they appear in the text. Modules: Source / Setting / Population / Behaviour / BCT / Mechanisms of Action / Style of Delivery / Dose / Reach / Engagement / Fidelity. 100% coverage. Output. Table. Sequence number / page / original text / normalized pattern / module.

**Bot response:**

`technical_deployment_study`

|Seq.|Page|Original text                                                                                                       |Normalized pattern                              |Module              |
|----|----|--------------------------------------------------------------------------------------------------------------------|------------------------------------------------|--------------------|
|1   |1   |Tsung-Yin Ou, Andrés Ponce, Cody Lee & Areoll Wu                                                                    |study authors / intervention developers         |Source              |
|2   |1   |deployed across over 7,000 7-Eleven stores in Taiwan                                                                |nationwide convenience-store network            |Setting             |
|3   |1   |planogram compliance system to be used in 7-Eleven stores across Taiwan                                             |retail implementation context                   |Setting             |
|4   |1   |product placement on store shelves plays a critical role in influencing consumer purchasing behavior                |target consumer purchasing behaviour            |Behaviour           |
|5   |2   |store employees the time to devote to other tasks                                                                   |store employees / frontline retail staff        |Population          |
|6   |2   |currently detect out-of-stock items                                                                                 |identify out-of-stock shelf states              |Behaviour           |
|7   |2   |company headquarters to verify this scenario on a daily basis                                                       |centralized compliance monitoring               |Fidelity            |
|8   |2   |introduces a scalable, real-time pipeline for planogram compliance verification                                     |real-time automated verification pipeline       |Style of Delivery   |
|9   |3   |captured in an image called a realogram by the end user                                                             |end-user image capture                          |Engagement          |
|10  |3   |feedback should be returned to the store employee so that necessary changes can be quickly made                     |feedback to prompt corrective action            |Mechanisms of Action|
|11  |3   |A store employee would use some electronic device to capture an image (or images) of a store shelf                  |employee-operated image submission              |Engagement          |
|12  |3   |via a mobile or web app                                                                                             |mobile/web app delivery                         |Style of Delivery   |
|13  |3   |This process of capturing a shelf image and sending it to the pipeline would be repeated for each shelf             |repeated per-shelf submission cycle             |Dose                |
|14  |3   |support multiple images for a single shelf                                                                          |multi-image capture for one target shelf        |Dose                |
|15  |3   |trained on data corresponding to 25 shelves across several retail locations                                         |multisite shelf coverage                        |Reach               |
|16  |4   |easy to understand and use for store employees                                                                      |user-friendly staff-facing system               |Engagement          |
|17  |4   |deployed across thousands of retail locations                                                                       |large-scale multisite deployment                |Reach               |
|18  |4   |obtained data from four distinct 7-Eleven stores                                                                    |four-store data collection setting              |Setting             |
|19  |4   |different time points throughout each day, covering various periods over three consecutive days                     |repeated temporal exposure / collection schedule|Dose                |
|20  |5   |Fresh foods… Room temperature drinks… Snacks… Instant noodles                                                       |product-category shelf variation                |Setting             |
|21  |5   |cover every product category and shelf type                                                                         |full category-and-shelf coverage target         |Reach               |
|22  |6   |we undertook a meticulous annotation process                                                                        |supervised manual annotation procedure          |Fidelity            |
|23  |6   |automatically organizing images into product categories using clustering / autolabeling description                 |semi-automated labeling support                 |Style of Delivery   |
|24  |7   |planogram verification system… returned to the store employee                                                       |compliance report returned to operator          |Mechanisms of Action|
|25  |7   |The planogram verification process relies on the output of the detection and classification models                  |model-driven decision support                   |Mechanisms of Action|
|26  |8   |tasks that require real-time results                                                                                |real-time computational delivery                |Style of Delivery   |
|27  |9   |We also set a threshold for the classification confidence                                                           |confidence-threshold decision rule              |Fidelity            |
|28  |9   |All the images received go through this process independently                                                       |per-image processing event                      |Dose                |
|29  |9   |the system combines each image’s detection and classification results into a single global shelf                    |multi-image integration workflow                |Style of Delivery   |
|30  |9   |virtual shelf mechanism                                                                                             |abstract shelf reconstruction mechanism         |Mechanisms of Action|
|31  |10  |out-of-stock item penalty / extra product penalty / unknown product penalty / wrong shelf penalty                   |rule-based discrepancy penalties                |Fidelity            |
|32  |10  |If the gap… is larger than some threshold, an item is considered to be missing                                      |threshold-based missing-item rule               |Fidelity            |
|33  |11  |datasets collected from 7-Eleven stores in Taiwan                                                                   |Taiwan 7-Eleven test context                    |Setting             |
|34  |11  |Training set come from three 7-Eleven stores, while data from the fourth, unseen store was reserved for the test set|store-level split across four stores            |Reach               |
|35  |12  |weighted sampling strategy during model training                                                                    |weighted sampling                               |BCT                 |
|36  |12  |Focal Loss… to reduce the effect of easier samples                                                                  |biasing learning toward hard/minority cases     |Mechanisms of Action|
|37  |12  |train the models for 100 epochs                                                                                     |fixed training repetition count                 |Dose                |
|38  |13  |few-shot accuracy… with only five samples per class                                                                 |minimal-sample exposure                         |Dose                |
|39  |13  |generalize across unseen product categories                                                                         |transfer to unseen categories                   |Reach               |
|40  |14  |deployment across more than 7,000 7-Eleven stores in Taiwan                                                         |nationwide rollout scope                        |Reach               |
|41  |14  |multi-image virtual shelf mechanism                                                                                 |virtual shelf reconstruction                    |Mechanisms of Action|
|42  |14  |improves audit completeness without imposing additional burdens on store employees                                  |lower user burden / easier completion           |Engagement          |
|43  |15  |images taken by users                                                                                               |user-generated shelf images                     |Engagement          |
|44  |15  |future deployment to thousands of 7-Eleven stores in Taiwan                                                         |scaled future store deployment                  |Reach               |
|45  |15  |all data were collected on-site in three different 7-Eleven stores, at various times and from different angles      |multisite, multi-time, multi-angle collection   |Reach               |
|46  |15  |deployment and scalability analysis… robust and efficient operation under high load conditions                      |load-tested operational robustness              |Fidelity            |
|47  |16  |source code will be made available                                                                                  |implementation source availability statement    |Source              |
|48  |16  |datasets… available from the corresponding author upon reasonable request                                           |author-mediated data access source              |Source              |
|49  |17  |Algorithm 1. Planogram Compliance Using the Needleman-Wunsch Algorithm                                              |formalized algorithm specification              |Fidelity            |
|50  |18  |References page                                                                                                     |no module-evidence on this page                 |—                   |
|51  |19  |Correspondence and requests for materials should be addressed to T.-Y.O.                                            |corresponding author / contact source           |Source              |

Coverage note: pages 1–19 covered in page order without cross-page jumping. Page 18 contains references only and is marked as no module-evidence. Module set evidenced: Source, Setting, Population, Behaviour, BCT, Mechanisms of Action, Style of Delivery, Dose, Reach, Engagement, Fidelity.

-----

**Request 2:**

> Assign each pattern an ontology ID. Output. Table. Sequence number / page / original text / normalized pattern / module / ID / label / definition.

**Bot response:**

`technical_deployment_study`

|Seq.|Page|Original text                                                                                                       |Normalized pattern                              |Module              |ID                                             |Label                                                   |Definition                                                                                                                                                     |
|----|----|--------------------------------------------------------------------------------------------------------------------|------------------------------------------------|--------------------|-----------------------------------------------|--------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
|1   |1   |Tsung-Yin Ou, Andrés Ponce, Cody Lee & Areoll Wu                                                                    |study authors / intervention developers         |Source              |BCIO:010041.00.06.so                           |author and journalist                                   |A legal, social and cultural professional that conceives and creates literary works, and interprets and communicates news and public affairs through the media.|
|2   |1   |deployed across over 7,000 7-Eleven stores in Taiwan                                                                |nationwide convenience-store network            |Setting             |BCIO:026001.00.01.r.se                         |country of intervention                                 |A geographical location of a country where the intervention takes place.                                                                                       |
|3   |1   |planogram compliance system to be used in 7-Eleven stores across Taiwan                                             |retail implementation context                   |Setting             |BCIO:026036.00.09.se                           |retail facility                                         |A facility used as an outlet for shopping.                                                                                                                     |
|4   |1   |product placement on store shelves plays a critical role in influencing consumer purchasing behavior                |target consumer purchasing behaviour            |Behaviour           |BCIO:050829.00.02.be.ma.sd                     |purchasing behaviour                                    |An economic behaviour in which the person gives money in exchange for a service or ownership of a good.                                                        |
|5   |2   |store employees the time to devote to other tasks                                                                   |store employees / frontline retail staff        |Population          |BCIO:015430.00.01.be.p.ma.sd                   |occupational role                                       |A personal role that is realised in a person by doing a specified type of work or working in a specified way.                                                  |
|6   |2   |currently detect out-of-stock items                                                                                 |identify out-of-stock shelf states              |Behaviour           |BCIO:050825.01.01.be.f.ma.sd                   |monitoring behaviour                                    |A knowledge development behaviour that involves gathering information over a period of time.                                                                   |
|7   |2   |company headquarters to verify this scenario on a daily basis                                                       |centralized compliance monitoring               |Fidelity            |BCIO:007017.08.19.bc.f                         |monitoring BCT                                          |A behaviour change technique that involves gathering or using information about performance.                                                                   |
|8   |2   |introduces a scalable, real-time pipeline for planogram compliance verification                                     |real-time automated verification pipeline       |Style of Delivery   |BCIO:008000.00.03.bc.be.p.ma.md.se.so.sd       |behaviour change intervention delivery                  |An intervention delivery in which the intervention is a behaviour change intervention.                                                                         |
|9   |3   |captured in an image called a realogram by the end user                                                             |end-user image capture                          |Engagement          |BCIO:050916.01.00.bc.be.p.ma.md.se.so.sd.e     |participant engagement with intervention                |An individual human activity of an intervention participant within one or more parts of the intervention.                                                      |
|10  |3   |feedback should be returned to the store employee so that necessary changes can be quickly made                     |feedback to prompt corrective action            |Mechanisms of Action|BCIO:050979.00.10.ma                           |mechanism of action through feedback process to a person|A behaviour change intervention mechanism of action in which the causal influence occurs in a feedback process to a person.                                    |
|11  |3   |A store employee would use some electronic device to capture an image (or images) of a store shelf                  |employee-operated image submission              |Engagement          |BCIO:050916.01.00.bc.be.p.ma.md.se.so.sd.e     |participant engagement with intervention                |An individual human activity of an intervention participant within one or more parts of the intervention.                                                      |
|12  |3   |via a mobile or web app                                                                                             |mobile/web app delivery                         |Style of Delivery   |BCIO:008000.00.03.bc.be.p.ma.md.se.so.sd       |behaviour change intervention delivery                  |An intervention delivery in which the intervention is a behaviour change intervention.                                                                         |
|13  |3   |This process of capturing a shelf image and sending it to the pipeline would be repeated for each shelf             |repeated per-shelf submission cycle             |Dose                |BCIO:009000.00.03.bc.be.d.f.ma.md.p.sd.se.sh.so|behaviour change intervention schedule of delivery      |A BCI attribute that involves its temporal organisation.                                                                                                       |
|14  |3   |support multiple images for a single shelf                                                                          |multi-image capture for one target shelf        |Dose                |BCIO:022000.00.03.bc.be.d.e.ma.md.p.sd.se.so   |behaviour change intervention dose                      |A BCI content attribute that is its amount.                                                                                                                    |
|15  |3   |trained on data corresponding to 25 shelves across several retail locations                                         |multisite shelf coverage                        |Reach               |BCIO:015095.01.04.bc.be.p.ma.md.se.so.sd.r     |intervention population                                 |A human population who are exposed to an intervention.                                                                                                         |
|16  |4   |easy to understand and use for store employees                                                                      |user-friendly staff-facing system               |Engagement          |BCIO:050916.01.00.bc.be.p.ma.md.se.so.sd.e     |participant engagement with intervention                |An individual human activity of an intervention participant within one or more parts of the intervention.                                                      |
|17  |4   |deployed across thousands of retail locations                                                                       |large-scale multisite deployment                |Reach               |BCIO:015095.01.04.bc.be.p.ma.md.se.so.sd.r     |intervention population                                 |A human population who are exposed to an intervention.                                                                                                         |
|18  |4   |obtained data from four distinct 7-Eleven stores                                                                    |four-store data collection setting              |Setting             |BCIO:026036.00.09.se                           |retail facility                                         |A facility used as an outlet for shopping.                                                                                                                     |
|19  |4   |different time points throughout each day, covering various periods over three consecutive days                     |repeated temporal exposure / collection schedule|Dose                |BCIO:009000.00.03.bc.be.d.f.ma.md.p.sd.se.sh.so|behaviour change intervention schedule of delivery      |A BCI attribute that involves its temporal organisation.                                                                                                       |
|20  |5   |Fresh foods… Room temperature drinks… Snacks… Instant noodles                                                       |product-category shelf variation                |Setting             |BCIO:026036.00.09.se                           |retail facility                                         |A facility used as an outlet for shopping.                                                                                                                     |
|21  |5   |cover every product category and shelf type                                                                         |full category-and-shelf coverage target         |Reach               |BCIO:015095.01.04.bc.be.p.ma.md.se.so.sd.r     |intervention population                                 |A human population who are exposed to an intervention.                                                                                                         |
|22  |6   |we undertook a meticulous annotation process                                                                        |supervised manual annotation procedure          |Fidelity            |BCIO:007017.08.19.bc.f                         |monitoring BCT                                          |A behaviour change technique that involves gathering or using information about performance.                                                                   |
|23  |6   |automatically organizing images into product categories using clustering / autolabeling description                 |semi-automated labeling support                 |Style of Delivery   |BCIO:008000.00.03.bc.be.p.ma.md.se.so.sd       |behaviour change intervention delivery                  |An intervention delivery in which the intervention is a behaviour change intervention.                                                                         |
|24  |7   |planogram verification system… returned to the store employee                                                       |compliance report returned to operator          |Mechanisms of Action|BCIO:050979.00.10.ma                           |mechanism of action through feedback process to a person|A behaviour change intervention mechanism of action in which the causal influence occurs in a feedback process to a person.                                    |
|25  |7   |The planogram verification process relies on the output of the detection and classification models                  |model-driven decision support                   |Mechanisms of Action|BCIO:050619.01.00.ma                           |comparison of current situation with a standard         |A comparative evaluation process that compares the current situation with a standard or benchmark.                                                             |
|26  |8   |tasks that require real-time results                                                                                |real-time computational delivery                |Style of Delivery   |BCIO:008000.00.03.bc.be.p.ma.md.se.so.sd       |behaviour change intervention delivery                  |An intervention delivery in which the intervention is a behaviour change intervention.                                                                         |
|27  |9   |We also set a threshold for the classification confidence                                                           |confidence-threshold decision rule              |Fidelity            |BCIO:007017.08.19.bc.f                         |monitoring BCT                                          |A behaviour change technique that involves gathering or using information about performance.                                                                   |
|28  |9   |All the images received go through this process independently                                                       |per-image processing event                      |Dose                |BCIO:022000.00.03.bc.be.d.e.ma.md.p.sd.se.so   |behaviour change intervention dose                      |A BCI content attribute that is its amount.                                                                                                                    |
|29  |9   |the system combines each image’s detection and classification results into a single global shelf                    |multi-image integration workflow                |Style of Delivery   |BCIO:008000.00.03.bc.be.p.ma.md.se.so.sd       |behaviour change intervention delivery                  |An intervention delivery in which the intervention is a behaviour change intervention.                                                                         |
|30  |9   |virtual shelf mechanism                                                                                             |abstract shelf reconstruction mechanism         |Mechanisms of Action|BCIO:050619.01.00.ma                           |comparison of current situation with a standard         |A comparative evaluation process that compares the current situation with a standard or benchmark.                                                             |
|31  |10  |out-of-stock item penalty / extra product penalty / unknown product penalty / wrong shelf penalty                   |rule-based discrepancy penalties                |Fidelity            |BCIO:007017.08.19.bc.f                         |monitoring BCT                                          |A behaviour change technique that involves gathering or using information about performance.                                                                   |
|32  |10  |If the gap… is larger than some threshold, an item is considered to be missing                                      |threshold-based missing-item rule               |Fidelity            |BCIO:007017.08.19.bc.f                         |monitoring BCT                                          |A behaviour change technique that involves gathering or using information about performance.                                                                   |
|33  |11  |datasets collected from 7-Eleven stores in Taiwan                                                                   |Taiwan 7-Eleven test context                    |Setting             |BCIO:026001.00.01.r.se                         |country of intervention                                 |A geographical location of a country where the intervention takes place.                                                                                       |
|34  |11  |Training set come from three 7-Eleven stores, while data from the fourth, unseen store was reserved for the test set|store-level split across four stores            |Reach               |BCIO:015095.01.04.bc.be.p.ma.md.se.so.sd.r     |intervention population                                 |A human population who are exposed to an intervention.                                                                                                         |
|35  |12  |weighted sampling strategy during model training                                                                    |weighted sampling                               |BCT                 |BCIO:033000.20.07.bc.be.p.ma.md.se.so.sd       |behaviour change technique                              |A planned process that is the smallest part of BCI content that is observable, replicable and on its own has the potential to bring about behaviour change.    |
|36  |12  |Focal Loss… to reduce the effect of easier samples                                                                  |biasing learning toward hard/minority cases     |Mechanisms of Action|BCIO:006119.07.04.ma.d.e.sh                    |associative learning                                    |Learning that involves changing the strength of association between two or more mental processes.                                                              |
|37  |12  |train the models for 100 epochs                                                                                     |fixed training repetition count                 |Dose                |BCIO:022000.00.03.bc.be.d.e.ma.md.p.sd.se.so   |behaviour change intervention dose                      |A BCI content attribute that is its amount.                                                                                                                    |
|38  |13  |few-shot accuracy… with only five samples per class                                                                 |minimal-sample exposure                         |Dose                |BCIO:022000.00.03.bc.be.d.e.ma.md.p.sd.se.so   |behaviour change intervention dose                      |A BCI content attribute that is its amount.                                                                                                                    |
|39  |13  |generalize across unseen product categories                                                                         |transfer to unseen categories                   |Reach               |BCIO:015095.01.04.bc.be.p.ma.md.se.so.sd.r     |intervention population                                 |A human population who are exposed to an intervention.                                                                                                         |
|40  |14  |deployment across more than 7,000 7-Eleven stores in Taiwan                                                         |nationwide rollout scope                        |Reach               |BCIO:015095.01.04.bc.be.p.ma.md.se.so.sd.r     |intervention population                                 |A human population who are exposed to an intervention.                                                                                                         |
|41  |14  |multi-image virtual shelf mechanism                                                                                 |virtual shelf reconstruction                    |Mechanisms of Action|BCIO:050619.01.00.ma                           |comparison of current situation with a standard         |A comparative evaluation process that compares the current situation with a standard or benchmark.                                                             |
|42  |14  |improves audit completeness without imposing additional burdens on store employees                                  |lower user burden / easier completion           |Engagement          |BCIO:050916.01.00.bc.be.p.ma.md.se.so.sd.e     |participant engagement with intervention                |An individual human activity of an intervention participant within one or more parts of the intervention.                                                      |
|43  |15  |images taken by users                                                                                               |user-generated shelf images                     |Engagement          |BCIO:050916.01.00.bc.be.p.ma.md.se.so.sd.e     |participant engagement with intervention                |An individual human activity of an intervention participant within one or more parts of the intervention.                                                      |
|44  |15  |future deployment to thousands of 7-Eleven stores in Taiwan                                                         |scaled future store deployment                  |Reach               |BCIO:015095.01.04.bc.be.p.ma.md.se.so.sd.r     |intervention population                                 |A human population who are exposed to an intervention.                                                                                                         |
|45  |15  |all data were collected on-site in three different 7-Eleven stores, at various times and from different angles      |multisite, multi-time, multi-angle collection   |Reach               |BCIO:015095.01.04.bc.be.p.ma.md.se.so.sd.r     |intervention population                                 |A human population who are exposed to an intervention.                                                                                                         |
|46  |15  |deployment and scalability analysis… robust and efficient operation under high load conditions                      |load-tested operational robustness              |Fidelity            |BCIO:007017.08.19.bc.f                         |monitoring BCT                                          |A behaviour change technique that involves gathering or using information about performance.                                                                   |
|47  |16  |source code will be made available                                                                                  |implementation source availability statement    |Source              |BCIO:010139.00.13.so                           |organisation source                                     |An organisation that is the bearer of a behaviour change intervention source role.                                                                             |
|48  |16  |datasets… available from the corresponding author upon reasonable request                                           |author-mediated data access source              |Source              |BCIO:010002.00.04.so                           |person source                                           |A person who is the bearer of a behaviour change intervention source role.                                                                                     |
|49  |17  |Algorithm 1. Planogram Compliance Using the Needleman-Wunsch Algorithm                                              |formalized algorithm specification              |Fidelity            |BCIO:007017.08.19.bc.f                         |monitoring BCT                                          |A behaviour change technique that involves gathering or using information about performance.                                                                   |
|50  |18  |References page                                                                                                     |no module-evidence on this page                 |—                   |                                               |                                                        |                                                                                                                                                               |
|51  |19  |Correspondence and requests for materials should be addressed to T.-Y.O.                                            |corresponding author / contact source           |Source              |BCIO:010002.00.04.so                           |person source                                           |A person who is the bearer of a behaviour change intervention source role.                                                                                     |

Note: some rows are close-fit ontology assignments rather than exact literal matches, especially for engineering-specific items such as weighted sampling, thresholds, and algorithm specification, because the paper is a technical deployment study and those phrases do not have direct one-to-one labels in the loaded ontology set. The page-18 row is intentionally preserved with empty ontology fields to keep full page coverage.
