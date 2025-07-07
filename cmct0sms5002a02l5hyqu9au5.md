---
title: "Chesterton’s Fence and Health Informatics: A Cautionary Guide for Kenyan Developers Refactoring EMRs"
datePublished: Mon Jul 07 2025 11:30:53 GMT+0000 (Coordinated Universal Time)
cuid: cmct0sms5002a02l5hyqu9au5
slug: chestertons-fence-and-health-informatics-a-cautionary-guide-for-kenyan-developers-refactoring-emrs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1751887712859/cfb4028b-26bc-409e-b569-72d3b72dcbcc.png
tags: healthinformatics-kenyahealthtech-softwareengineering-fhir-legacycode

---

> *"Before you tear it down, ask why it was built."*  
> — A developer’s version of Chesterton’s Fence

---

## 🔍 Introduction

In software development — and especially in **health informatics** — developers often encounter legacy logic, unexplained validations, strange database constraints, or redundant-looking workflows. The impulse is natural:

> “This rule looks unnecessary — let me remove or refactor it.”

But what if it’s not useless? What if that cryptic validation was the **only thing preventing a billing error in SHA**, or **an ICD-10 mismatch that breaks KHIS reports**?

This is where **Chesterton’s Fence** comes in — a principle every healthtech developer in Kenya should understand before making changes to EMRs, HAPI-FHIR backends, or OpenEMR forks.

---

## 🧱 What is Chesterton’s Fence?

Originally proposed by G.K. Chesterton, it states:

> *“Don’t remove a fence until you understand why it was put up in the first place.”*

Translated to software engineering:

> **Don’t remove or refactor a piece of code or a system behavior unless you fully understand what problem it was solving.**

Especially in **health informatics**, where data quality, reporting accuracy, and patient safety are critical, this principle is not optional — it’s **foundational**.

---

## Why This Matters in Kenya’s Health Informatics Landscape

Let’s take real-world examples from the Kenyan digital health ecosystem:

* **OpenEMR implementations with custom NHIF modules**
    
* **FHIR APIs validating SHA-based patient IDs**
    
* **EMR integrations for M-PESA payments and KHIS monthly reporting**
    

These systems often contain “legacy-looking” checks that junior developers want to clean up. But those checks often encode:

* **Regulatory constraints**
    
* **Business logic shaped by national reporting**
    
* **Workarounds for unreliable infrastructure**
    

---

## 🧠 Real Developer Scenarios Where Chesterton’s Fence Applies

### 🧩 1. A Strange Validation Rule in Patient Registration

```plaintext
phpCopyEditif (!preg_match('/^SHA\d{9}$/', $patientId)) {
   throw new Exception("Invalid Patient ID Format");
}
```

Looks overly strict?

> **Reality**: SHA enforces a 12-digit format. Removing this check may allow garbage data, leading to rejected claims or broken referrals.

---

### 🧪 2. Redundant Workflow in OpenEMR Lab Module

> Vitals must be entered twice: by a nurse, then confirmed by the clinician.

Why not automate or collapse this?

> **Possible Answer**:

* Required for **clinical audit trails**
    
* Mapped to **CarePlan activityStatus** in FHIR bundles
    
* Mirrors **MOH Form 19** workflow
    

---

### 🧾 3. Unexplained Delay in M-PESA Receipt Sync

```plaintext
javaCopyEditThread.sleep(10000); // Wait for M-PESA push callback
```

Looks like bad code? Sure. But removing it caused:

* Receipts to miss SHA claim bundling
    
* Race conditions in the transaction ledger
    

The original dev had no comment — but the behavior covered for **latency in mobile carrier callback systems**. The “fence” worked — just undocumented.

---

## 🧮 How to Apply Chesterton’s Fence as a Health Tech Developer

### ✅ 1. **Use Git Blame as Your Friend**

Before removing any legacy block:

* Check who added it
    
* Read commit messages
    
* Search for linked Jira, Redmine, or GitHub issues
    

```plaintext
bashCopyEditgit blame src/validation/sha_id_check.js
```

### ✅ 2. **Trace the Full Data Flow**

Before touching a single line:

* Trace the variable to the UI
    
* Trace the response to the API
    
* Trace the data into the report pipeline (KHIS, HIE, KEMRI registry)
    

Especially with **FHIR**, a small change in `Condition.code` or `Observation.status` can break downstream parsing at the Ministry of Health.

### ✅ 3. **Write a FHIR Validator Test Case**

If the “weird” structure relates to FHIR:

```plaintext
bashCopyEditjava -jar validator.jar cancer-condition-bundle.json -version 4.0.1
```

Often, what looks like awkward nesting or duplicative data is required by:

* FHIR slicing rules
    
* ValueSet bindings (e.g., ICD-10 `code` for `Condition`)
    
* Canonical Profile constraints (like `OncologyKenyaCondition`)
    

---

## 🔥 What Happens When You Ignore the Fence?

### ❌ Case: Dev Deletes a Referral Feedback Rule

The rule:

```plaintext
pythonCopyEditif feedback.date < referral.date:
   reject("Feedback cannot precede referral")
```

Looked obvious. Removed.

Result:

* Hospitals began uploading referrals without checking visit order
    
* Cancer patients received **follow-up** before official **diagnosis**
    
* Registry reports became non-sequential, leading to data cleaning errors at KEMRI
    

---

## 💡 Key Lessons for Developers in Kenyan Health Informatics

| Bad Practice | Better Practice |
| --- | --- |
| “This doesn’t make sense — delete it” | “Let me trace the full lifecycle of this feature” |
| “We don’t need this NHIF format” | “Let’s confirm with a test submission to NHIF sandbox” |
| “FHIR is just JSON” | “Let me check what profile this resource is validating against” |
| “This is redundant UI” | “Maybe it maps to a regulatory form or audit log” |

---

## 🧠 Final Thoughts: Code Is a Conversation With the Past

Chesterton’s Fence is a reminder that:

> **Legacy code is often not bad code — it’s code written to meet constraints you may not yet understand.**

As a Kenyan health tech developer, whether you're building:

* SHA integrations
    
* M-PESA billing workflows
    
* FHIR-based cancer registries
    
* Offline-first CHW apps
    

...slow down before removing that “weird” bit of logic.

Ask:

> *What problem was this solving? Is it still relevant?*

If yes — keep it.  
If no — remove it **responsibly**, and document **why**.

---

## 🧠 About the Author

I'm Stephen Macharia, a digital health engineer building **FHIR-based oncology solutions**, **EMR systems**, and **interoperability tools** in Kenya. I work at the crossroads of OpenEMR, HAPI-FHIR, SHA IDs, and health policy — where removing the wrong line of code can derail national reporting.

Check out my ongoing work at Oncology IG Kenya.

---

## 🔎 SEO Tags (Kenya-Focused Developer SEO)

* Chesterton’s Fence in health informatics
    
* EMR software development in Kenya
    
* FHIR validation in Kenyan health systems
    
* OpenEMR custom workflows Kenya
    
* SHA patient ID integration
    
* M-PESA EMR billing Kenya
    
* KHIS data reporting best practices
    
* Digital health refactoring mistakes
    
* Software development for Kenyan public health
    
* HAPI FHIR Kenya developers
    

---

## 💬 Comments Welcome

Have you ever removed a piece of code and broken a workflow you didn’t understand?  
Have you encountered “legacy logic” that turned out to be essential?

Share your thoughts — or your “Fence moment” — below.