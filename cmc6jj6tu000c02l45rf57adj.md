---
title: "When 0mg Broke Our Pipeline: A FHIR, HL7v2, and Mirth Connect Failure"
datePublished: Sat Jun 21 2025 17:56:43 GMT+0000 (Coordinated Universal Time)
cuid: cmc6jj6tu000c02l45rf57adj
slug: when-0mg-broke-our-pipeline-a-fhir-hl7v2-and-mirth-connect-failure
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1750529605817/06c74a9d-aef5-4727-9c61-f3dcb6254b5a.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1750528594979/238039ab-524a-42f2-8058-6c277cc39827.png
tags: digitalhealth-kenyahealth-healthitkenya-ehealth-interoperabilitykenya-kenyaemr-smarthealtharchitecture-if-referencing-shagoke-mohkenya-healthstandardskenya-openhie-openmrs

---

In digital health, `"0"` must be treated as **data**, not as **absence**. A real failure in our interoperability stack taught us that **0mg is not null** — and mistaking it as such can jeopardize patient safety.

---

### 💥 Clinical Scenario

A doctor intended to **temporarily pause** a patient's medication:

> Paracetamol 0mg PO BID × 3 days

This entry of `0mg` was **not an omission**, but an **intentional instruction**.

But due to how our systems handled the value `0`, the pause instruction never reached the EMR. The result? The patient continued receiving 500mg doses twice daily.

---

### 🔧 System Architecture

* **EMR**: Internal hospital medication entry form
    
* **Integration Engine**: Mirth Connect (HL7v2 → FHIR transformation)
    
* **FHIR Server**: HAPI FHIR storing `MedicationRequest`
    
* **Downstream App**: Consuming FHIR API
    

---

### 🧨 Where the Pipeline Broke

#### Step 1: HL7v2 Segment Generated

```plaintext
hl7CopyEditRXE|^^^Paracetamol|0|mg|...
```

The clinician used `0` to signal a **temporary hold**.

---

#### Step 2: Mirth Connect Transformation

```plaintext
javascriptCopyEditvar dosage = msg['RXE']['GiveAmountMinimum'].toString();
if (!dosage) {
    msg['RXE']['GiveAmountMinimum'] = null;
}
```

**Issue**: In JavaScript, `0` is a falsy value — so `if (!dosage)` evaluates to `true`, setting the field to `null`.

---

#### Step 3: FHIR `MedicationRequest` Created

```plaintext
jsonCopyEdit{
  "resourceType": "MedicationRequest",
  "status": "active",
  "medicationCodeableConcept": {
    "text": "Paracetamol"
  },
  "dosageInstruction": []
}
```

There’s **no** `dosageInstruction`, which downstream systems interpret as “no changes.”

---

### ⚠️ Outcome

* The hold instruction was **never recorded**
    
* Medication continued as if `0mg` was **not entered**
    
* Nurses administered previous dose
    
* Doctor’s intent was lost — with **no audit trail**
    

---

### ✅ Fix: Preserve `0` as Valid Data

#### ✔️ JavaScript Fix in Mirth

```plaintext
javascriptCopyEditvar dosage = msg['RXE']['GiveAmountMinimum'].toString();
if (dosage === '') {
    msg['RXE']['GiveAmountMinimum'] = null;
}
```

This ensures that only truly **empty** values are treated as missing.

---

#### ✔️ Updated FHIR Output

```plaintext
jsonCopyEdit{
  "resourceType": "MedicationRequest",
  "status": "active",
  "medicationCodeableConcept": {
    "text": "Paracetamol"
  },
  "dosageInstruction": [
    {
      "text": "Temporarily hold medication",
      "doseAndRate": [
        {
          "doseQuantity": {
            "value": 0,
            "unit": "mg"
          }
        }
      ]
    }
  ],
  "statusReason": {
    "text": "Hold due to elevated liver enzymes"
  }
}
```

---

#### ✔️ Frontend Input Validation (UI)

```plaintext
javascriptCopyEditif (input === null || input === undefined || input === '') {
    throw "Dosage must be entered — even if it's 0";
}
```

**Lesson**: `0` is **not** missing — it’s a **clinical directive**.

---

### 🧠 Lessons Learned

| Layer | What to Fix |
| --- | --- |
| UI | Accept and validate `0` as valid dosage |
| Mirth | Don’t treat `0` as missing; use strict conditions |
| FHIR | Represent `0mg` clearly with supporting text |
| EMR | Differentiate between `0` and blank/null properly |

---

### 🛡 Final Thought

A `0mg` dose is a **medical decision**. Ignoring it is not just a tech bug — it’s a clinical risk.

> **In health data, zero is not the absence of intent. It is intent.**

---

✍️ *Stephen Macharia*  
Championing safe digital health in Kenya’s interoperability stack.

---