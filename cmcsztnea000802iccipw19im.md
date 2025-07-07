---
title: "The Dunning–Kruger Effect in Computing and Health Informatics: A Technical Deep Dive"
datePublished: Mon Jul 07 2025 11:03:40 GMT+0000 (Coordinated Universal Time)
cuid: cmcsztnea000802iccipw19im
slug: the-dunningkruger-effect-in-computing-and-health-informatics-a-technical-deep-dive
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1751886155817/ec4a456f-c33a-46c9-be66-77758e794e30.png

---

*“The greatest obstacle to discovery is not ignorance – it is the illusion of knowledge.”*  
— Daniel J. Boorstin

---

## 1\. Introduction

In the intersection of **computing** and **health informatics**, there's an often unaddressed risk that is not about missing data, slow APIs, or bad code — it’s about **misjudged competence**.

The **Dunning–Kruger effect**, a cognitive bias where individuals with limited knowledge overestimate their expertise, is particularly harmful in this domain. When applied to **electronic medical record (EMR) development**, **FHIR implementations**, **clinical decision support (CDS)**, and **digital health system design**, the consequences are amplified — because human lives are involved.

This post unpacks the Dunning–Kruger effect from a **technical perspective** and illustrates how it manifests in health informatics, using examples from **EMR architecture**, **interoperability standards**, **medication safety systems**, and **real-world constraints in countries like Kenya**.

---

## 2\. Understanding the Dunning–Kruger Curve

The Dunning–Kruger curve typically follows four stages:

* **Mount Stupid**: High confidence, low competence
    
* **Valley of Despair**: Realization of complexity
    
* **Slope of Enlightenment**: Gradual competence-building
    
* **Plateau of Sustainability**: Expert-level competence with humility
    

In health tech, Mount Stupid often looks like:

> “FHIR is just REST + JSON. I can build a national health data exchange in two sprints.”

And the Valley of Despair hits when:

> “Why are my Observation resources rejected by the HAPI server? Oh, I’m missing LOINC. Wait — LOINC isn’t even used in Kenya?”

---

## 3\. Manifestation in Computing (with Technical Examples)

Let’s examine how this bias affects computing teams building health systems:

### 3.1 Misuse of Standards Due to Superficial Understanding

**Example:**  
A developer reads that “FHIR supports patient records” and decides to store all custom Kenyan oncology metadata in `Patient.extension`.

> **Problem**: While technically allowed, this violates semantic interoperability. Extensions are a last resort, not a default. Key data like `cancer stage`, `treatment plan`, or `payment mode` should go into structured FHIR resources like `Condition`, `CarePlan`, and `Coverage`.

### 3.2 Overconfidence in Security without Threat Modeling

**Example:**  
A junior engineer enables HTTPS on an OpenEMR instance and assumes it’s secure.

> **Problem**: TLS is just one layer. What about:

* SQL injection through legacy forms?
    
* Insecure SHA ID transmission over M-PESA receipts?
    
* No audit logs for failed login attempts?
    

A proper **threat model** would consider these vectors. Overconfidence skips this step.

### 3.3 Reinventing the Wheel

**Example:**  
Teams manually implement terminology mapping instead of leveraging `CodeSystem` and `ValueSet` FHIR resources or terminology servers.

> **Result**: Broken workflows when codes like `"C50.9"` (ICD-10: Breast Cancer) are inconsistently mapped across different reports.

---

## 4\. Manifestation in Health Informatics (Kenyan Context)

### 4.1 Assumptions of Universal Infrastructure

Many global health developers assume consistent power and internet. This leads to:

* Failures of cloud-only EMRs in rural Kenya
    
* Lack of offline-first support in apps
    
* M-PESA integration breaking due to lack of local caching or queuing
    

A **Dunning–Kruger mindset** ignores ground realities like:

* Data entry done by Community Health Workers (CHWs) on solar-powered devices
    
* Patient identifiers requiring SHA validation with intermittent network
    

### 4.2 Misapplication of AI in Clinical Decision Support

Example: A system flags penicillin allergies but fails to capture **beta-lactam** family cross-reactivity.

> **Why?** Because the implementer didn’t understand clinical pharmacology or the structure of drug classification hierarchies in RxNorm/ATC.

True CDS systems require:

* **Ontology-aware querying**
    
* Support for **synonyms**, **cross-mappings**, and **nested relationships**
    
* Integration with known **adverse event vocabularies**
    

---

## 5\. How to Technically Mitigate the Dunning–Kruger Effect

### ✅ 5.1 Institutionalize Peer Reviews

Every major workflow (referrals, billing, consent, etc.) must be reviewed by:

* Domain experts (oncologists, CHWs, MOHs)
    
* System architects
    
* Terminology and standards specialists
    

### ✅ 5.2 Adopt Progressive Complexity Frameworks

Avoid building complex workflows in one step. Use stages:

| Stage | Goal |
| --- | --- |
| P0 | Minimal FHIR-compliant `Patient`, `Condition`, `Encounter` |
| P1 | Add `CarePlan`, `Procedure`, `MedicationRequest` |
| P2 | Interoperability tests with public registries or HAPI servers |
| P3 | Clinical logic support, offline-first sync, M-PESA + SHA integration |

This reduces overreach caused by misplaced confidence.

### ✅ 5.3 Use Formal Evaluation Metrics

Use measures such as:

* **FHIR validator score**
    
* **Code coverage (unit/integration tests)**
    
* **Clinical usability testing (via CHWs)**
    
* **Security audits (OWASP)**
    

Overconfidence often crumbles under empirical evaluation.

---

## 6\. Final Thoughts

In health informatics, **technical excellence and humility must go hand in hand**. The cost of premature confidence is not just project failure — it’s clinical risk.

The best technologists in health systems are those who:

* Ask questions
    
* Say “I don’t know — yet”
    
* Document their assumptions
    
* Continuously test and revise
    

So next time you're designing a `Bundle` or debating HL7 v2 vs FHIR, pause. Ask:

> *“Am I designing this from evidence, or from assumed knowledge?”*

Because true expertise starts when the illusion of knowledge fades.

---

## 🛠️ About the Author

I'm Stephen Macharia, a digital health engineer building EMRs and FHIR-based solutions in Kenya, including Oncology IG Kenya. I work with OpenEMR, HAPI servers, M-PESA billing flows, and SHA-based patient ID systems to modernize care delivery in low-resource settings.

---

## 💬 Let's Discuss

* Have you ever encountered a “Mount Stupid” moment in health tech?
    
* How do you balance technical ambition with clinical safety?