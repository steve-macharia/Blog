---
title: "When Health Tech Gets It Wrong in Kenya: The Danger of Weight-Based Alerts Without Context"
datePublished: Fri Jul 04 2025 15:57:25 GMT+0000 (Coordinated Universal Time)
cuid: cmcozzuqe000902jr03d870df
slug: when-health-tech-gets-it-wrong-in-kenya-the-danger-of-weight-based-alerts-without-context
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1751644546597/55d27808-7764-4b1a-b819-53e50597fea2.png
tags: digitalhealthkenya-kenyaemr-healthitke-smarthospitalske-mohkenyatech

---

In Kenya’s growing digital health landscape—powered by SHA eClaims, EMRs like KenyaEMR and OpenMRS, and Ministry of Health (MOH) data standards—**clinical decision support systems (CDSS)** are critical to improving healthcare outcomes.

But what happens when a system built to help, **gets it wrong**?

---

### 🏥 Real Case From a Kenyan Facility

A **35-year-old Kenyan male**, weighing just **42 kilograms**, walked into a health facility in Kiambu County for **routine chronic care follow-up**. He had been battling a long-standing condition, possibly HIV-related wasting or advanced diabetes.

Despite the EMR clearly showing his age, sex, and diagnosis history, the **system wrongly flagged him as a pediatric case**. Why?

Because he weighed **less than 45 kg**.

> 🚨 The system triggered a **"Pediatric Underweight Alert"**, assuming the patient was a child—simply based on weight.

---

### ❌ The Logic That Failed Kenyan Context

Many digital health platforms used in Kenya—including those in **Level 3 and 4 facilities**—use alerts like:

```plaintext
plaintextCopyEditIf weight < 45kg → trigger pediatric malnutrition flag
```

This may work in theory. But in Kenya’s real clinical settings—where we manage **HIV**, **TB**, **cancer**, and **malnourished adults in arid counties** like Turkana or Wajir—such logic falls apart.

It led to:

* Wrong **triage routing** to the pediatric desk
    
* **Incorrect nutrition recommendations** based on child protocols
    
* **Confusion among clinicians**, especially interns and nurses
    
* A near-miss where a pediatric supplement dosage was almost administered to an adult male
    

---

### 🧠 What Kenyan Systems Should Do Instead

Let’s make this local. A better rule would be:

> **“Only trigger pediatric malnutrition alerts if patient age is under 18 AND weight is &lt; 45 kg.”**

Even better for Kenya:

✅ Use **BMI-for-age**, not just raw weight  
✅ Incorporate **chronic illness indicators** like HIV, TB, or oncology tags  
✅ Build the logic around **MOH data dictionaries** and **KenyaEMR workflows**

---

### ⚠️ Why This Matters to Kenya’s Health Sector

This isn’t just a coding issue—it’s a **patient safety issue**.

If EMRs used in SHA-accredited hospitals or UHC pilot counties keep using over-simplified logic, we will:

* Undermine **trust in eHealth systems** like AfyaCare, IQCare, and OpenMRS
    
* Cause **alert fatigue** among Kenyan clinicians
    
* Potentially harm vulnerable adults with low body weight
    

---

### ✅ Lessons for Kenyan Health Developers

Let’s stop designing systems that treat **weight as identity**.

Instead, we must:

* Validate alerts against **age and diagnosis**
    
* Test EMR rules with real patient data from facilities in **Isiolo, Kisumu, or Garissa**
    
* Engage with **clinical officers and nurses** when developing decision logic
    

---

> 📢 **Let’s build digital health systems in Kenya that think like clinicians, not just like calculators.**

---

### 📌 Author

**Stephen Macharia**  
Digital Health Engineer | EMR Workflow Designer | FHIR Enthusiast  
📍 Nairobi, Kenya  
📧 medbytech254@gmail.com  
🌐 [https://medby-tech.netlify.app](https://medby-tech.netlify.app)