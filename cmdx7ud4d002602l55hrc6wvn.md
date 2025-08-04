---
title: "🩺 How to Use HAPI FHIR Server in Kenya: A Step-by-Step Guide with Oncology IG Profiles"
datePublished: Mon Aug 04 2025 14:38:58 GMT+0000 (Coordinated Universal Time)
cuid: cmdx7ud4d002602l55hrc6wvn
slug: how-to-use-hapi-fhir-server-in-kenya-a-step-by-step-guide-with-oncology-ig-profiles

---

**Published on Hashnode by Stephen Maina — August 2025**  
**Tags:** #FHIR #Kenya #DigitalHealth #HAPIFHIR #HealthIT #SHA #Oncology

---

## 🌍 Why HAPI FHIR Matters for Kenya

Kenya is undergoing a **digital health revolution**. With the rollout of the **Social Health Authority (SHA)** and the push for **Universal Health Coverage (UHC)**, there's increasing demand for **interoperable health systems**.

To achieve this, Kenya is aligning with global data exchange standards — and **FHIR (Fast Healthcare Interoperability Resources)** is at the center of it all.

### ✅ Enter HAPI FHIR Server

HAPI FHIR is an open-source, Java-based server that allows you to:

* Host a **FHIR-compliant REST API**
    
* Validate resources like `Patient`, `Observation`, and `Condition`
    
* Load **Kenya-specific profiles**, e.g., for **oncology**, **HIV**, or **maternal health**
    

In this blog, I’ll walk you through how to set up a HAPI FHIR server in Kenya and load profiles from a real-world FHIR project: **Oncology IG Kenya**.

---

## 🔍 Real-World Use Case: Oncology Implementation Guide – Kenya

Kenya’s cancer burden is rising. According to **Kenya National Cancer Control Strategy (2023–2027)**, over **47,000 new cancer cases** are reported annually. To standardize cancer data, a FHIR-based **Oncology Implementation Guide (IG)** is being developed, aligned with SHA’s reimbursement workflows and Ministry of Health priorities.

This guide defines:

* FHIR **StructureDefinitions** (custom Patient, Condition, Procedure, etc.)
    
* **ValueSets** for common terminologies used in Kenyan cancer care
    
* **CodeSystems** for local classifications and diagnostic codes
    

---

## ⚙️ Setting Up HAPI FHIR Server in Kenya

### 📁 Project Structure

```plaintext
bashCopyEdit~/Desktop/Oncology-IG-Kenya/Oncology-IG-Kenya-main     # Contains FHIR resources
~/Desktop/oncology-fhir-server-kenya-main              # HAPI FHIR Dockerized server
```

---

## 🛠 Step-by-Step: Load Profiles into HAPI FHIR Server

### Step 1: Start the Server

Run your server using Docker:

```plaintext
bashCopyEditcd ~/Desktop/oncology-fhir-server-kenya-main
docker-compose up -d
```

Wait for it to be accessible at:

```plaintext
bashCopyEdithttp://localhost:8080/fhir/metadata
```

---

### Step 2: Prepare Profile Files

Ensure your JSON files (e.g., `OncologyPatient.json`, `OncologyCondition.json`) are valid and located in:

```plaintext
bashCopyEdit~/Desktop/Oncology-IG-Kenya/Oncology-IG-Kenya-main/profiles/
```

Each file must have a valid FHIR `id`.

---

### Step 3: Upload Profiles to HAPI Server

Use `curl` to upload one by one:

```plaintext
bashCopyEditcurl -X PUT \
  -H "Content-Type: application/fhir+json" \
  -d @OncologyPatient.json \
  http://localhost:8080/fhir/StructureDefinition/OncologyPatient
```

Repeat for:

* `StructureDefinition`
    
* `ValueSet`
    
* `CodeSystem`
    

---

### Step 4: Automate with a Shell Script (Optional)

```plaintext
bashCopyEdit#!/bin/bash
FHIR_BASE="http://localhost:8080/fhir"
for file in ~/Desktop/Oncology-IG-Kenya/Oncology-IG-Kenya-main/profiles/*.json; do
  id=$(jq -r '.id' "$file")
  echo "Uploading $id..."
  curl -X PUT -H "Content-Type: application/fhir+json" -d @"$file" "$FHIR_BASE/StructureDefinition/$id"
done
```

Install `jq` if needed: `sudo apt install jq`

---

## 🎯 Why This is Critical for Kenya’s Health Sector

### 1\. **Supports Interoperability Goals**

Kenya’s **Digital Health Investment Roadmap (2022–2030)** identifies **interoperability** and **standardized health records** as key to UHC. HAPI FHIR makes it possible to implement and share structured data across counties and providers.

### 2\. **Aligns with SHA & KHIS**

* SHA requires structured patient data for **claims and reimbursements**
    
* KHIS (Kenya Health Information System) supports **FHIR-based integration**
    
* Using HAPI FHIR server with Kenya-specific IGs ensures **clean, valid data** flows into these national systems
    

### 3\. **Enables Scalable, Local Solutions**

Unlike proprietary systems, HAPI FHIR is **open-source**, customizable, and supported by a global community — perfect for Kenyan developers and implementers.

---

## 🧠 Bonus: How to Extend This

You can now:

* Add **SMART-on-FHIR** authentication
    
* Build React or Angular frontends for cancer registries
    
* Integrate with **OpenMRS**, **OpenEMR**, or **DHIS2**
    
* Validate incoming data using your custom Oncology profiles
    

---

## 🚀 Conclusion

FHIR is no longer just a global standard — it's a **local necessity**. With HAPI FHIR, Kenya’s developers and health IT implementers can build powerful, interoperable, standards-based systems that scale.

Loading IG profiles like **Oncology-IG-Kenya** into your HAPI FHIR server is the first step toward a connected, data-driven health ecosystem.

---

💬 **Let’s connect!**  
Are you building FHIR tools in Kenya? Let's collaborate.  
📧 Reach me at: `stephen@medby-tech.co.ke`  
🔗 Find this project on GitHub: [github.com/Medby-Tech](https://github.com/Medby-Tech)  
🔍 Tags: #FHIR #KenyaDigitalHealth #SHA #UHC #OncologyData #OpenHIE #HAPIFHIR