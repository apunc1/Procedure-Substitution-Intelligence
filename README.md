# Procedure Substitution Intelligence

**Tagline:** *Discover how procedures are used in place of one another.*

## 1. Product Purpose

Build a healthcare analytics application that identifies **potential procedure substitution patterns** in claims data and explains how, when, where, and among whom one procedure appears to be used instead of another.

The system combines:

* CPT/HCPCS reference data
* claims utilization
* diagnosis relationships
* clinical concepts
* provider behavior
* place of service
* temporal patterns
* coverage and authorization context
* graph analytics
* machine learning
* evidence-grounded LLM explanations

The central question is:

> **“Which procedures appear to serve similar roles in observed healthcare utilization, and what evidence supports that relationship?”**

The product should **not** automatically conclude that two procedures are clinically equivalent or that one is an appropriate replacement for another.

---

# 2. Core Business Questions

The application should help an analyst answer:

* Which procedures are potentially substituting for one another?
* How frequently is the relationship observed?
* Which providers use both procedures?
* Are the procedures associated with similar diagnoses?
* Are they used for similar clinical concepts?
* Are they performed in similar places of service?
* Did utilization shift from one procedure to another over time?
* Did a coding change precede a utilization shift?
* Did a payer policy change coincide with a shift?
* Are the procedures substitutes in all settings or only certain contexts?
* Is there authoritative evidence that the procedures are alternatives?
* What additional information should an analyst review?

---

# 3. Important Definition

The application should distinguish three different concepts.

### Observed Substitution

Claims data shows that utilization of Procedure B increases as utilization of Procedure A decreases within a defined population/context.

### Utilization Similarity

Two procedures have similar:

* diagnoses
* providers
* specialties
* POS
* clinical concepts
* temporal utilization patterns

### Clinical Equivalence

An authoritative clinical source explicitly indicates that the procedures are alternatives or equivalent for a particular indication.

These are **not the same thing**.

Your application should never turn:

> “These procedures are frequently used by the same providers for similar diagnoses”

into:

> “Procedure B is clinically equivalent to Procedure A.”

That distinction makes the project substantially more defensible.

---

# 4. Portfolio Positioning

This project fits naturally with your existing **Procedure-to-Clinical-Concept Graph**.

### Procedure Intelligence

> What is this procedure and how is it used?

### Clinical Concept Graph

> What healthcare concepts and activities connect to this procedure?

### Procedure Substitution Intelligence

> What other procedures appear to occupy a similar role in observed utilization?

### Coding Change Intelligence

> Did a coding-system change affect procedure utilization?

### Payer Policy Diff Engine

> Did a policy change affect which procedures are used?

### Prior Authorization Risk Engine

> What authorization risks exist before the procedure?

### Denial Prevention Engine

> What could cause the claim to fail?

The resulting portfolio becomes:

```text
              PROCEDURE
                  │
                  ▼
        PROCEDURE INTELLIGENCE
                  │
                  ▼
       CLINICAL CONCEPT GRAPH
                  │
          ┌───────┴────────┐
          ▼                ▼
  SUBSTITUTION         CODING CHANGE
  INTELLIGENCE         INTELLIGENCE
          │                │
          └───────┬────────┘
                  ▼
            PAYER POLICY
                  │
                  ▼
        PRIOR AUTHORIZATION
                  │
                  ▼
          CLAIM ACTIVITY
                  │
                  ▼
        DENIAL PREVENTION
```

---

# 5. Example Use Case

Suppose Procedure A and Procedure B are both observed for a particular set of diagnoses.

The application finds:

```text
Procedure A
   ↓
Diagnosis X
Diagnosis Y
Diagnosis Z

Procedure B
   ↓
Diagnosis X
Diagnosis Y
Diagnosis Z
```

They also share:

* providers
* specialties
* POS
* clinical concepts

Then over time:

```text
Procedure A utilization
████████████
██████████
███████

Procedure B utilization
████
██████
█████████
```

The engine could identify:

> **Potential substitution pattern:** Procedure B increased while Procedure A decreased within the selected population and time period.

It would then show the evidence behind that observation.

---

# 6. Primary Product Workflow

```text
Claims + Procedure Reference Data
                ↓
          Data Engineering
                ↓
       Procedure Normalization
                ↓
        Clinical Context Layer
                ↓
       Procedure Relationship Model
                ↓
       Similarity Candidate Generation
                ↓
        Substitution Detection
                ↓
        Temporal Validation
                ↓
        Context Segmentation
                ↓
         Evidence Assembly
                ↓
          AI Explanation
                ↓
             Streamlit
```

---

# 7. MVP Scope

Do not begin by attempting to determine clinical equivalence.

The MVP should focus on:

1. Procedure relationship discovery
2. Shared diagnosis analysis
3. Shared provider analysis
4. Shared clinical-concept analysis
5. Temporal utilization analysis
6. Observed substitution indicators
7. Evidence-based investigation

The first version should answer:

> **“Where do these procedures appear to behave similarly in the available claims data?”**

---

# 8. Procedure Pair Discovery

Start by generating candidate procedure pairs.

Potential signals:

### Shared Diagnoses

How much overlap exists between the diagnoses associated with two procedures?

### Shared Providers

How often are both procedures used by the same providers?

### Shared Specialties

Do they occur within similar specialties?

### Shared POS

Are they performed in similar settings?

### Shared Clinical Concepts

Do their associated diagnoses map to similar clinical concepts?

### Temporal Relationship

Does increased use of one coincide with decreased use of another?

---

# 9. Procedure Similarity Features

For every procedure pair calculate features such as:

```text
procedure_a
procedure_b

shared_diagnosis_count
shared_diagnosis_ratio

shared_provider_count
shared_provider_ratio

shared_specialty_count
shared_specialty_ratio

shared_pos_count

clinical_concept_overlap

provider_overlap
diagnosis_similarity

utilization_correlation

temporal_lag
```

These become the foundation for the relationship model.

---

# 10. Diagnosis Similarity

One of the strongest signals should be diagnosis overlap.

For each procedure:

```text
Procedure A
    ↓
Diagnosis distribution

Procedure B
    ↓
Diagnosis distribution
```

Compare those distributions using:

* Jaccard similarity
* cosine similarity
* weighted overlap
* shared diagnosis percentage

For example:

```text
A diagnoses:
X 50%
Y 30%
Z 20%

B diagnoses:
X 45%
Y 35%
Z 20%
```

The application can report a high **observed diagnosis-profile similarity**.

---

# 11. Clinical Concept Similarity

Use your **Procedure-to-Clinical-Concept Graph**.

Example:

```text
Procedure A
   ↓
ICD-10
   ↓
Clinical Concept

Procedure B
   ↓
ICD-10
   ↓
Clinical Concept
```

Then compare the clinical concepts associated with the two procedures.

Potential concepts:

* anatomical system
* disease category
* symptom
* condition
* body region
* diagnostic category

Again, describe this as **claims-derived clinical-concept overlap**, not clinical equivalence.

---

# 12. Provider Overlap

Calculate:

> What percentage of providers using Procedure A also use Procedure B?

And:

> What percentage of Procedure B providers also use Procedure A?

This helps distinguish procedures that occupy similar operational roles from procedures that simply happen to share a diagnosis.

---

# 13. Specialty Similarity

Compare provider specialty distributions.

Example:

```text
Procedure A

Orthopedics       42%
Primary Care      20%
Pain Medicine     18%
Other             20%

Procedure B

Orthopedics       45%
Primary Care      18%
Pain Medicine     20%
Other             17%
```

This provides another utilization-context signal.

---

# 14. Place-of-Service Similarity

Compare:

* office
* inpatient
* outpatient hospital
* ASC
* emergency department
* laboratory
* DME
* other POS

A pair might appear similar overall but have very different settings.

The application should therefore allow:

> **Compare within a specific POS.**

---

# 15. Temporal Substitution Detection

This is where the project becomes much more interesting.

Suppose:

```text
Year       Procedure A    Procedure B

2023          10,000         2,000
2024           9,000         4,000
2025           6,500         7,000
```

The system detects that B increased while A decreased.

But don't automatically call this substitution.

Instead calculate:

### Direction

A ↓ / B ↑

### Magnitude

How large was the change?

### Timing

Did the changes occur in the same period?

### Persistence

Did the pattern continue?

### Context

Did it occur within the same:

* diagnosis
* provider
* specialty
* POS
* payer
* geography?

---

# 16. Contextual Substitution

A particularly strong feature would be finding substitution **within the same clinical/utilization context**.

Instead of:

```text
Procedure A total utilization
vs
Procedure B total utilization
```

calculate:

```text
Procedure A + Diagnosis X + Specialty Y + POS Z

vs

Procedure B + Diagnosis X + Specialty Y + POS Z
```

This dramatically reduces false relationships caused by broad utilization differences.

---

# 17. Patient-Level Sequence Analysis

Where the data supports it, investigate whether the same beneficiaries receive different procedures.

For example:

```text
Patient
 ↓
Procedure A
 ↓
Later encounter
 ↓
Procedure B
```

Or:

```text
Population
 ↓
Procedure A decreases
 ↓
Procedure B increases
```

Use patient-level analysis carefully and only with appropriately de-identified/synthetic data.

The goal is to identify **observed utilization sequences**, not infer that B replaced A clinically.

---

# 18. Provider-Level Substitution

Another powerful analysis:

> Do individual providers switch between Procedure A and Procedure B?

For each provider:

```text
Provider A

Procedure A:
2024 → 80%
2025 → 55%

Procedure B:
2024 → 20%
2025 → 45%
```

This may be a stronger substitution signal than population-level changes.

You can then investigate:

* provider
* specialty
* diagnosis
* POS
* time period

---

# 19. Procedure Pair Evidence Record

Create a core table:

```text
procedure_relationship_df
```

Fields:

```text
procedure_a
procedure_b

shared_diagnosis_count
diagnosis_similarity

shared_provider_count
provider_overlap

specialty_similarity
pos_similarity

clinical_concept_overlap

utilization_correlation
temporal_relationship

a_utilization_before
a_utilization_after

b_utilization_before
b_utilization_after

shared_context_count

relationship_type
evidence
```

Possible relationship types:

```text
SIMILAR_UTILIZATION
POTENTIAL_SUBSTITUTION
TEMPORAL_SHIFT
SHARED_CLINICAL_CONTEXT
SHARED_PROVIDER_USAGE
```

Avoid labeling a pair “equivalent” unless authoritative evidence supports it.

---

# 20. Graph Architecture

This project should heavily leverage graph analytics.

Create:

```text
Procedure A
     │
     ├── Diagnosis X
     ├── Diagnosis Y
     ├── Provider 1
     ├── Provider 2
     └── Concept Z

Procedure B
     │
     ├── Diagnosis X
     ├── Diagnosis Y
     ├── Provider 1
     ├── Provider 3
     └── Concept Z
```

The overlap creates a relationship between A and B.

Graph edge attributes could include:

```text
diagnosis_overlap
provider_overlap
concept_overlap
specialty_overlap
pos_overlap
temporal_similarity
```

---

# 21. Relationship Strength

Internally you can calculate a composite relationship measure.

For example:

```text
Relationship Evidence
=
Diagnosis overlap
+
Provider overlap
+
Clinical concept overlap
+
Specialty overlap
+
POS overlap
+
Temporal evidence
```

But don't expose this as an unexplained:

> “Substitution score = 94.”

Instead show the components.

Example:

```text
Observed Evidence

Diagnosis overlap: 82%
Provider overlap: 64%
Clinical concept overlap: 76%
POS similarity: 91%
Temporal relationship: Present
```

This is much more useful to an analyst.

---

# 22. Machine Learning Opportunities

After the analytical foundation is established, add ML.

## A. Procedure Embeddings

Create embeddings representing each procedure based on:

* diagnoses
* providers
* specialties
* POS
* clinical concepts
* utilization patterns

Then identify similar procedures.

## B. Clustering

Cluster procedures based on utilization context.

## C. Link Prediction

Predict potential procedure relationships that aren't obvious from simple overlap.

## D. Temporal Models

Identify procedure pairs with synchronized or opposing utilization trends.

---

# 23. Candidate Ranking Without a Black Box

Instead of saying:

> “The model says these are substitutes.”

produce:

```text
Candidate Relationship

Procedure A ↔ Procedure B

Evidence:
✓ High diagnosis overlap
✓ High provider overlap
✓ Similar POS distribution
✓ Shared clinical concepts
✓ Opposing utilization trend
✓ Persistent for 8 months
```

Then:

> **Potential substitution relationship identified.**

This keeps the result explainable.

---

# 24. Coding Change Integration

This is one of the strongest connections to your **Coding Change Intelligence** project.

Suppose:

```text
Procedure A
        ↓
Deleted / revised
        ↓
Procedure B appears
        ↓
Procedure B utilization increases
```

The application can investigate:

> Did Procedure B's utilization increase following a coding-system change affecting Procedure A?

This does **not** prove causation, but it creates a strong investigation workflow.

---

# 25. Payer Policy Integration

Connect the project to your **Payer Policy Diff Engine**.

Example:

```text
Policy Change
      ↓
Procedure A
      ↓
Coverage / PA requirement changes
      ↓
Procedure B utilization
      ↓
Potential utilization shift
```

The application could identify:

> A utilization shift occurred following a policy change affecting Procedure A.

Then show the policy evidence and claims evidence separately.

---

# 26. Prior Authorization Integration

Compare authorization requirements between procedures.

Example:

| Procedure | PA           | Historical Utilization |
| --------- | ------------ | ---------------------: |
| A         | Required     |                 10,000 |
| B         | Not required |                  3,500 |

If utilization shifts from A to B after a policy change, the system can surface the relationship for investigation.

It should **not** conclude that the PA requirement caused the substitution.

---

# 27. Denial Integration

Investigate whether procedure pairs have different denial patterns.

Example:

```text
Procedure A
   ↓
Authorization denials

Procedure B
   ↓
Lower historical authorization-denial frequency
```

The system can report:

> Historical denial patterns differ between the procedures.

It should not infer that providers switched procedures to avoid denials without evidence supporting that explanation.

---

# 28. Streamlit Application

## Page 1 — Procedure Relationship Explorer

Input:

* Procedure A
* Procedure B

Output:

```text
Procedure A
Procedure B

Diagnosis similarity
Provider overlap
Specialty similarity
POS similarity
Clinical concept overlap
Temporal relationship
```

---

# 29. Page 2 — Discover Similar Procedures

Select one procedure.

Return:

```text
Potentially Related Procedures

Procedure B
Procedure C
Procedure D
Procedure E
```

For each show the underlying evidence:

* diagnosis overlap
* provider overlap
* concept overlap
* POS similarity
* utilization relationship

Do not present them as a ranked “best substitutes” list.

Instead group them by relationship type:

```text
Shared utilization context
Temporal shift
Shared providers
Shared diagnoses
```

---

# 30. Page 3 — Substitution Investigation

This is the primary product page.

Example:

```text
Procedure A → Procedure B

Observed relationship:
Potential substitution pattern
```

Then:

### Shared Clinical Context

Diagnoses and clinical concepts.

### Shared Providers

Providers using both procedures.

### Utilization

Before/after trends.

### Temporal Relationship

A ↓ while B ↑.

### Setting

POS comparison.

### Evidence

Supporting claims metrics.

---

# 31. Page 4 — Provider Switching

Show providers that use both procedures.

Example:

```text
Provider
       ↓
Procedure A utilization
       ↓
Procedure B utilization
       ↓
Change over time
```

This is potentially one of the most compelling views in the application.

---

# 32. Page 5 — Population / Patient Analysis

Show:

* beneficiaries receiving A
* beneficiaries receiving B
* beneficiaries receiving both
* sequence patterns
* diagnosis context
* time between procedures

Again, describe these as **observed claims sequences**.

---

# 33. Page 6 — Coding & Policy Context

For the selected procedure pair:

Show:

* code changes
* policy changes
* coverage
* authorization
* effective dates

Example:

```text
Procedure A
    ↓
Coding Change
    ↓
Policy Change
    ↓
Utilization Change
    ↓
Procedure B
```

This ties multiple portfolio projects together.

---

# 34. Page 7 — AI Procedure Analyst

The AI assistant should answer:

> Why are these procedures being identified as potentially related?

> What diagnoses do they share?

> Which providers use both?

> Did utilization shift over time?

> Did a coding change occur?

> Did a payer policy change?

> What evidence supports the relationship?

> What should I investigate next?

The LLM should synthesize evidence rather than generate the relationship itself.

---

# 35. AI Guardrails

The AI must:

* distinguish utilization similarity from clinical equivalence
* distinguish association from causation
* identify the source of each claim
* use only retrieved evidence
* avoid inventing clinical guidance
* avoid declaring a procedure appropriate/inappropriate
* avoid recommending one procedure over another
* avoid saying one procedure “should replace” another
* identify uncertainty
* distinguish authoritative evidence from claims-derived evidence

A safe response:

> “The procedures show substantial overlap in observed diagnosis, provider, and place-of-service patterns. Claims data also shows an opposing utilization trend during the selected period. These findings support investigating a potential utilization substitution relationship; they do not establish clinical equivalence or causation.”

---

# 36. Authoritative Clinical Evidence

Eventually add external authoritative sources that can distinguish utilization relationships from documented clinical alternatives.

Potential sources include:

* CMS
* specialty-society guidance
* payer policies
* official coding guidance
* clinical terminology/reference sources

Create an evidence type:

```text
AUTHORITATIVE_ALTERNATIVE
```

separate from:

```text
CLAIMS_OBSERVED_RELATIONSHIP
```

This distinction is central to the product.

---

# 37. Evaluation Strategy

Because true “substitution” is difficult to label from claims alone, don't evaluate only against an arbitrary ground truth.

Evaluate several components.

### Similarity Evaluation

Do known related procedures have higher similarity than unrelated procedures?

### Temporal Detection

Can the system detect synthetic utilization shifts?

### Provider Switching

Can known injected provider-level shifts be detected?

### Graph Validation

Do procedure pairs with shared clinical context show stronger graph connectivity?

### False Positive Analysis

Investigate why apparently unrelated procedures were paired.

### Explainability

Can an analyst understand why the relationship was identified?

---

# 38. Synthetic Test Scenarios

Create synthetic scenarios with known patterns.

### Scenario 1 — True Utilization Shift

```text
A ↓ 40%
B ↑ 50%
```

### Scenario 2 — No Relationship

```text
A ↓
B remains stable
```

### Scenario 3 — Shared Diagnosis but No Substitution

Both procedures serve the same broad population but move independently.

This is important because it tests whether your detector confuses similarity with substitution.

### Scenario 4 — Policy Change

Policy affecting A changes, followed by an increase in B.

The system should identify the temporal association but not claim causality.

### Scenario 5 — Coding Change

A code is deleted/replaced and B subsequently appears.

Again, surface the evidence rather than asserting causation.

---

# 39. Development Phases

## Phase 1 — Procedure Relationship MVP

Build:

* claims ingestion
* procedure normalization
* diagnosis relationships
* provider relationships
* specialty
* POS
* procedure-pair metrics

No ML or LLM.

---

## Phase 2 — Similarity Intelligence

Add:

* diagnosis similarity
* provider overlap
* specialty similarity
* POS similarity
* clinical-concept overlap

Build Procedure Relationship Explorer.

---

## Phase 3 — Temporal Substitution

Add:

* time-series analysis
* utilization shifts
* provider switching
* before/after comparisons
* persistence

---

## Phase 4 — Graph Intelligence

Integrate:

* Procedure → Diagnosis
* Procedure → Provider
* Procedure → Clinical Concept
* Procedure → POS

Create procedure relationship graphs.

---

## Phase 5 — Machine Learning

Add:

* embeddings
* clustering
* procedure similarity
* link prediction
* temporal relationship modeling

---

## Phase 6 — Policy & Coding Context

Integrate:

* Coding Change Intelligence
* Payer Policy Diff Engine
* authorization requirements
* coverage

---

## Phase 7 — AI Procedure Analyst

Add:

* evidence retrieval
* natural-language questions
* relationship explanations
* source-aware summaries

---

## Phase 8 — Monitoring

Eventually monitor procedure relationships continuously.

```text
New Claims
    ↓
Procedure Relationships
    ↓
Utilization Shift Detection
    ↓
New Relationship
    ↓
Investigation Queue
```

---

# 40. Suggested Repository

```text
procedure-substitution-intelligence/
│
├── app.py
├── requirements.txt
├── README.md
│
├── data/
│   ├── raw/
│   └── processed/
│
├── src/
│   ├── ingest.py
│   ├── claims.py
│   ├── procedures.py
│   ├── diagnoses.py
│   ├── providers.py
│   ├── concepts.py
│   ├── similarity.py
│   ├── substitution.py
│   ├── temporal.py
│   ├── graph.py
│   ├── evidence.py
│   └── llm.py
│
├── notebooks/
│   ├── 01_procedure_relationships.ipynb
│   ├── 02_similarity.ipynb
│   ├── 03_temporal_shifts.ipynb
│   └── 04_substitution_detection.ipynb
│
└── tests/
    ├── test_similarity.py
    ├── test_temporal.py
    └── test_substitution.py
```

---

# 41. Technical Stack

### Data

* Python
* pandas
* DuckDB
* Parquet

### Analytics

* NumPy
* SciPy
* scikit-learn

### Graph

* NetworkX
* Plotly

### NLP

* embeddings
* semantic similarity
* LLM API

### Application

* Streamlit

### Deployment

* Streamlit Community Cloud

---

# 42. Final Architecture

```text
                    CLAIMS
                      │
                      ▼
              DATA ENGINEERING
               Python/DuckDB
                      │
                      ▼
             PROCEDURE ENTITIES
                      │
          ┌───────────┼───────────┐
          ▼           ▼           ▼
      DIAGNOSES    PROVIDERS     POS
          │           │           │
          └───────────┼───────────┘
                      ▼
              CLINICAL CONCEPTS
                      │
                      ▼
              PROCEDURE GRAPH
                      │
                      ▼
             PROCEDURE PAIRS
                      │
        ┌─────────────┼─────────────┐
        ▼             ▼             ▼
    Similarity     Temporal       Provider
     Analysis       Analysis       Switching
        │             │             │
        └─────────────┼─────────────┘
                      ▼
             RELATIONSHIP ENGINE
                      │
              ┌───────┴────────┐
              ▼                ▼
       Similar Utilization   Potential
                             Substitution
              │                │
              └───────┬────────┘
                      ▼
                POLICY / CODE
                  CONTEXT
                      │
                      ▼
                EVIDENCE LAYER
                      │
                      ▼
             AI PROCEDURE ANALYST
                      │
                      ▼
                  STREAMLIT
                      │
                      ▼
               INVESTIGATION
```

# 43. Portfolio Description

A concise portfolio description:

> **Built a Procedure Substitution Intelligence platform using healthcare claims, procedure, diagnosis, provider, and clinical-concept data to identify potential procedure substitution patterns. Combined graph-based relationships, utilization similarity, provider switching, and temporal analysis with evidence-grounded AI explanations while distinguishing observed claims relationships from clinical equivalence.**

Shorter resume version:

> **Developed a graph- and ML-based Procedure Substitution Intelligence application to identify potential procedure substitution patterns from claims data using diagnosis overlap, provider behavior, clinical concepts, and temporal utilization analysis.**

---

# 44. What Makes This Project Technically Interesting

The strongest version of this project is **not**:

```text
CPT A → find CPTs that look similar
```

It is:

```text
                 PROCEDURE A
                      │
       ┌──────────────┼──────────────┐
       ▼              ▼              ▼
   Diagnoses       Providers        POS
       │              │              │
       └──────────────┼──────────────┘
                      ▼
               Clinical Concepts
                      │
                      ▼
              PROCEDURE B
                      │
                      ▼
             Temporal Analysis
                      │
                      ▼
            Provider Switching
                      │
                      ▼
             Policy / Code Changes
                      │
                      ▼
              Evidence Layer
                      │
                      ▼
             AI Explanation
```

That turns the project into a demonstration of **healthcare graph analytics + temporal analytics + ML + NLP/LLM + data engineering**, while preserving the important distinction between **what claims data shows and what clinical evidence establishes**.
