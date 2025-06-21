---
title: "Building Kenya’s Oncology Digital Future with FHIR and Open Standards"
datePublished: Sat Jun 21 2025 16:08:45 GMT+0000 (Coordinated Universal Time)
cuid: cmc6focdi000h02l4co3a4d9p
slug: building-kenyas-oncology-digital-future-with-fhir-and-open-standards

---

### Kenya Needs Interoperable Cancer Care — Here's What We're Doing

Kenya’s cancer burden is growing rapidly, with over 47,000 new cases annually and 32,000 deaths according to the Kenya Cancer Policy 2020–2030. Despite this, oncology data is still fragmented across paper records, local EMRs, and disconnected registries.

It’s time to change that.

---

### 💡 My Mission: Kenya’s Oncology Implementation Guide (Oncology-IG-Kenya)

I’m currently building the **Oncology Implementation Guide for Kenya (Oncology-IG-Kenya)** — a standards-based project using **FHIR (Fast Healthcare Interoperability Resources)** to digitize oncology workflows across Kenya.

This guide will define how data like:

* Diagnosis (ICD-10)
    
* Chemotherapy regimens
    
* Cancer staging
    
* Claims and reimbursements
    
* Patient encounters
    
* Cancer registry reporting
    

...can be shared **electronically and accurately** across facilities, counties, and national systems like SHA (Smart Health Authority of Kenya).

---

### 🛠️ Built on Global Standards, Customized for Kenya

Most global tools like SNOMED CT or LOINC are **too heavy or unavailable** in Kenya.

That’s why this project uses:

* **FHIR R4** as the backbone
    
* **ICD-10** for diagnosis (per Kenya MoH policy)
    
* Local adaptations from:
    
    * Kenya Cancer Policy 2020–2030
        
    * KEMRI Cancer Registry
        
    * Kenya National Cancer Control Strategy 2023–2027
        
    * County-specific registry research (e.g., Nyandarua)
        

---

### ⚙️ Real Code, Real Tools

This isn’t theory. I’m working hands-on with:

* 🔗 [HAPI FHIR Server](https://hapifhir.io/)
    
* 🧪 Custom FHIR Profiles: `OncologyKenyaEncounter.fsh`, `OncologyKenyaCarePlan.fsh`, `OncologyKenyaMedicationRequest.fsh`
    
* 🧾 Claim workflows adapted to Kenyan insurance realities (NHIF, SHA)
    
* 📝 Questionnaire + QuestionnaireResponse for cancer screening and early detection
    

All code is stored and versioned using **GitHub**, and the final package will be open-source for use by counties, private hospitals, and EMR vendors.

---

### 🌍 Why This Matters

* It empowers **county-level registries** like Nyandarua to report and act on real-time data
    
* It supports **early cancer screening** and **financial access** via structured claims
    
* It lays the groundwork for **national e-cancer registries** compatible with SHA
    

---

### 📢 Call to Action

I invite:

* Kenyan health tech devs
    
* Public sector innovators
    
* Global health data standards advocates
    
* Local EMR vendors
    

...to collaborate and **build local-first digital health tools** that scale.

---

### 👨🏾‍💻 Follow My Journey

I'll be publishing technical breakdowns, workflows, and implementation tricks **daily** on this blog. From HL7 quirks to Kenya-specific challenges — no gatekeeping here.

---

> **Stephen Macharia**  
> FHIR enthusiast | Cancer informatics implementer | Kenyan health systems advocate  
> 🌐 [GitHub](https://github.com/steve-macharia) | 💬 Let's connect on this journey!