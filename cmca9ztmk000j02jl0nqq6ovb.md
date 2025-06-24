---
title: "Title: Building a FHIR Implementation Guide for Oncology in Kenya"
datePublished: Tue Jun 24 2025 08:40:47 GMT+0000 (Coordinated Universal Time)
cuid: cmca9ztmk000j02jl0nqq6ovb
slug: title-building-a-fhir-implementation-guide-for-oncology-in-kenya
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1750754296909/cc571b1b-3d7c-415b-a57a-3e3a4bfdc79e.png
tags: fhir-kenya-health-tech-oncology-kenya-digital-health-smart-health-kenya-openemr-kemri

---

🔗 View the live Implementation Guide: [https://steve-macharia.github.io/Oncology-IG-Kenya/](https://steve-macharia.github.io/Oncology-IG-Kenya/)

### Why We Built This Guide

Cancer is one of the top causes of morbidity in Kenya, yet most EMRs and health systems lack a common, structured standard for oncology data. The **Kenya National Cancer Control Strategy 2023–2027** outlines a vision for improving registry reporting, care coordination, and national monitoring.

To support this, we built a **FHIR R4-based Implementation Guide (IG)** tailored to Kenyan oncology workflows. This guide:

* Uses **ICD-10** for diagnosis coding
    
* Uses **ATC** for medication classification
    
* Adopts **SHA (Social Health Authority)** patient identifiers
    
* Reflects real policies, registries, and EMR use cases in Kenya
    

---

### 🧰 Tools and Standards We Used

* **FHIR R4** — Core data modeling standard
    
* **SUSHI + FSH (.fsh)** — Authoring and compiling profiles
    
* **IG Publisher** — Building the browsable IG
    
* **HAPI FHIR Server** — Testing the profiles
    
* **Kenya-specific documents** — For workflows and policy grounding
    

---

### 📦 Key FHIR Profiles

---

**1\. OncologyKenyaEncounter**

Used to document structured oncology visits.

```plaintext
bashCopyEditProfile: OncologyKenyaEncounter
Parent: Encounter
Id: oncology-kenya-encounter

* status = #finished
* class = http://terminology.hl7.org/CodeSystem/v3-ActCode#AMB "ambulatory"
* type = http://id.who.int/icd10#C50 // Breast cancer
* subject 1..1 MS
* period.start 1..1 MS
* serviceProvider 1..1 MS
```

---

**2\. OncologyKenyaCareplan**

Captures treatment plans (e.g., chemotherapy cycles).

```plaintext
makefileCopyEditProfile: OncologyKenyaCareplan
Parent: CarePlan
Id: oncology-kenya-careplan

* status = #active
* intent = #plan
* subject 1..1 MS
* activity.detail.code = http://id.who.int/icd10#Z51.1 // Chemotherapy session
```

---

**3\. OncologyKenyaMedicationRequest**

Represents chemotherapy or oncology drug prescriptions using ATC codes.

```plaintext
makefileCopyEditProfile: OncologyKenyaMedicationRequest
Parent: MedicationRequest
Id: oncology-kenya-medicationrequest

* status = #active
* intent = #order
* medicationCodeableConcept = http://www.whocc.no/atc#L01X02 // Cisplatin
* subject 1..1
* authoredOn 1..1
* requester 1..1
```

---

**4\. OncologyKenyaClaim**

Represents claim submissions to SHA (Social Health Authority) for reimbursement.

```plaintext
bashCopyEditProfile: OncologyKenyaClaim
Parent: Claim
Id: oncology-kenya-claim

* status = #active
* type = http://terminology.hl7.org/CodeSystem/claim-type#institutional
* patient 1..1
* insurer 1..1
* diagnosis.sequence 1..1
* diagnosis.diagnosisCodeableConcept = http://id.who.int/icd10#C54.1
```

---

**5\. OncologyKenyaQuestionnaireResponse**

Captures structured data from national cancer screening forms.

```plaintext
makefileCopyEditProfile: OncologyKenyaQuestionnaireResponse
Parent: QuestionnaireResponse
Id: oncology-kenya-questionnaireresponse

* questionnaire = Canonical(OncologyKenyaScreeningForm)
* status = #completed
* subject 1..1
* authored 1..1
```

---

### 🌍 Kenyan Localization

**Identifiers:**  
We used SHA’s patient identifier system:

```plaintext
pgsqlCopyEdit* identifier.system = "https://sha.go.ke/identifier/patient-id"
* identifier.value = "SHA-12345678"
```

**Terminology:**

* **ICD-10**: For diagnosis (mandatory in MoH reports)
    
* **ATC**: For medications (aligned with Kenyan EML)
    
* **No SNOMED CT**: Not used in Kenya due to licensing and infrastructure constraints
    

---

### 🏗️ How We Built the IG

**1\. Install SUSHI**

```plaintext
nginxCopyEditnpm install -g fsh-sushi
```

**2\. Initialize IG Project**

```plaintext
csharpCopyEditsushi --init Oncology-IG-Kenya
```

**3\. Add** `.fsh` Profiles

Save your `.fsh` files in `input/fsh/`.

**4\. Compile Using SUSHI**

```plaintext
nginxCopyEditsushi .
```

**5\. Build with IG Publisher**

```plaintext
nginxCopyEditjava -jar publisher.jar -ig ig.ini
```

**6\. Host Online**

We deployed the IG using GitHub Pages:  
🔗 [https://steve-macharia.github.io/Oncology-IG-Kenya/](https://steve-macharia.github.io/Oncology-IG-Kenya/)

---

### 🧪 Testing with HAPI FHIR Server

* Uploaded the profiles to a local HAPI FHIR server
    
* Created resources using `POST /CarePlan`
    
* Validated profiles with `$validate`
    
* Queried using `patient.identifier=SHA-12345678`
    

---

### ⚠️ Key Challenges and Solutions

| Challenge | Solution |
| --- | --- |
| Lack of local standard vocabularies | ICD-10 + ATC as reliable base |
| Mapping Kenya workflows into FHIR | Used actual forms, registries, and policies |
| IG validation errors | Fixed with SUSHI and HL7 Validator |
| No LOINC/SNOMED access | Removed dependencies entirely |

---

### 🚀 What’s Next?

* Integrate the IG into **Jali-EMR** (our OpenEMR-based platform)
    
* Add cancer registry reporting resources
    
* Collaborate with **MoH**, **SHA**, and **KEMRI** for national testing
    
* Open it for implementation across health systems in Kenya
    

---

### 💬 Want to Collaborate?

We are open to working with:

* EMR vendors in Kenya
    
* Cancer registries and researchers
    
* Digital health implementers
    

**GitHub:** [https://github.com/steve-macharia](https://github.com/steve-macharia)  
**Twitter:** [https://twitter.com/stephenmacharia](https://twitter.com/stephenmacharia)

---

🔗 View the full Implementation Guide:  
[https://steve-macharia.github.io/Oncology-IG-Kenya/](https://steve-macharia.github.io/Oncology-IG-Kenya/)