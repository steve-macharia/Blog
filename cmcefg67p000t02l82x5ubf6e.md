---
title: "What is Non-Intuitive Code?"
datePublished: Fri Jun 27 2025 06:24:33 GMT+0000 (Coordinated Universal Time)
cuid: cmcefg67p000t02l82x5ubf6e
slug: what-is-non-intuitive-code
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1751004948475/3ca7ae16-7ed0-4ee1-b56e-afe62298dfbd.png
tags: emrsystemskenya-cleancodepracticeske-digitalhealthkenya-kenyasoftwaredevelopment

---

## A Pragmatic Exploration Through Software and Design Thinking

> “It’s not enough for code to work. It should also make sense.”  
> — Inspired by *The Pragmatic Programmer* and *The Design of Everyday Things*

---

In Kenya’s software engineering landscape — across digital health, fintech, education platforms, and government systems — we frequently encounter code that functions correctly but is difficult to reason about. This is what we refer to as **non-intuitive code**.

This article draws technical inspiration from *The Pragmatic Programmer* by Andy Hunt and Dave Thomas, and *The Design of Everyday Things* by Don Norman, to explore what non-intuitive code is, why it’s dangerous, and how to design code that aligns with human understanding.

---

## Defining Non-Intuitive Code

Non-intuitive code is code that **fails the cognitive expectation of the developer** reading or maintaining it. It does not necessarily throw errors, but it breaks the reader’s mental model. It’s difficult to follow, unexpected in its behavior, or obscures its purpose.

From a design standpoint, this is comparable to a door with a pull handle that requires pushing. It violates the principle of affordance and forces the user to think unnecessarily — a core idea in Don Norman’s work on usability.

In software, that extra cognitive load results in bugs, slower development, and increased maintenance costs.

---

## Principles from *The Pragmatic Programmer*

*The Pragmatic Programmer* frames code as **a form of communication**, not merely instructions for machines. When that communication becomes unclear, code becomes a liability.

Key principles related to intuitive design include:

* **Communicate Intent**: Code should reflect what it is trying to achieve, not just how.
    
* **Orthogonality**: Functions or components should behave independently to avoid side effects that surprise the developer.
    
* **Avoid Cleverness**: Clever code is often opaque. The most elegant solution is usually the most straightforward one.
    
* **Design by Contract**: When inputs and outputs are well-defined, developers can predict behavior.
    

Non-intuitive code often emerges when these principles are ignored, especially in projects without peer review, documentation, or modular structure.

---

## Lessons from *The Design of Everyday Things*

In *The Design of Everyday Things*, Norman introduces the idea that usability issues are almost always design issues — not user failures.

His concepts apply directly to code:

| Design Principle | Software Equivalent |
| --- | --- |
| Affordance | Clear function and variable naming |
| Mapping | Logical code structure and file organization |
| Feedback | Meaningful error messages and return values |
| Visibility of State | Transparent state transitions and control flow |
| Error Prevention | Guard clauses, input validation, strong typing |

Non-intuitive code violates these principles by obscuring state, hiding cause and effect, and forcing developers to rely on internal assumptions instead of visible cues.

---

## Why This Matters in Enterprise and Public Sector Systems

In Kenya’s software sector, developers maintain and extend critical systems in healthcare (EMRs), finance (wallets, billing platforms), education (LMSs), and government (citizen registries). Many of these platforms are used across counties or agencies, with contributors from diverse backgrounds and varying levels of experience.

Non-intuitive code creates challenges such as:

* High onboarding time for new team members
    
* Increased risk of regression errors during updates
    
* Slower incident response during system outages
    
* Greater reliance on individual developers (knowledge silos)
    

For example, a county health records system written without clear mappings between user roles and access controls may work — until a misconfiguration allows unintended patient access. The root cause is often not a bug, but non-intuitive code design.

---

## Recognizing Non-Intuitive Code (Without Looking at Code)

You are likely dealing with non-intuitive code if:

* Understanding a function requires opening three unrelated files
    
* The name of a class or method doesn’t describe what it actually does
    
* Changing one line unexpectedly affects another module
    
* State changes are silent or hidden behind shared globals
    
* Logic is “clever” but not obvious to new developers
    

These issues slow down maintenance and weaken software reliability, especially in high-impact systems like health, insurance, and citizen services.

---

## What Developers and Teams Can Do

Creating intuitive code requires intentional effort. Strategies include:

| Strategy | Benefit |
| --- | --- |
| Use descriptive naming conventions | Improves readability and predictability |
| Follow logical file structures | Helps future contributors navigate faster |
| Separate concerns into small units | Avoids coupling and confusion |
| Document edge cases and decisions | Supports institutional memory |
| Conduct cross-level peer reviews | Ensures clarity across experience levels |
| Use tools like PHPStan, Psalm, and CodeSniffer | Automatically enforce consistency and sanity checks |

Intuitive code isn't always shorter — but it's always more predictable. That predictability is critical in environments with distributed teams, evolving policies, and strict audit requirements.

---

## Final Thought

Non-intuitive code is not a technical failure; it’s a **design failure**. It violates both the principles of pragmatic programming and those of user-centered design.

In fast-evolving technology ecosystems like Kenya’s — where digital health infrastructure, education platforms, and open-source contributions are all accelerating — **intuitive code is an investment in long-term sustainability**.

The next developer might not sit in your office. They may be from a different county, institution, or even country. Write code that they can understand without guessing.

---

## Continue the Conversation

Are you maintaining legacy systems, contributing to national registries, or building modern APIs for Kenyan institutions? Share your experiences with code that made (or broke) your project.

Feel free to connect with me @steve-macharia for more content on software architecture, open standards like HL7 FHIR, and clean coding in critical systems