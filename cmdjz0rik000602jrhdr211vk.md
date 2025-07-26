---
title: "Why Most Kenyan Developers Misuse Code Comments (And What Uncle Bob Recommends Instead)"
seoTitle: "Clean Code for Kenyan Programmers: Commenting Best Practices"
seoDescription: "Learn how Kenyan developers can write cleaner code using Uncle Bob’s comment principles. Avoid messy comments and boost code quality today"
datePublished: Sat Jul 26 2025 08:11:00 GMT+0000 (Coordinated Universal Time)
cuid: cmdjz0rik000602jrhdr211vk
slug: why-most-kenyan-developers-misuse-code-comments-and-what-uncle-bob-recommends-instead
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753518894724/e789ee6c-893a-4f66-a1d0-1e5d4986a96d.webp
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1753517299184/1e82281d-ce33-46e3-900c-e21fddb750f0.webp
tags: cleancode-kenyadevelopers-commentingbestpractices-unclebobprinciples-kenyatech-hashnodekenya-codereadability-nairobidevelopers-softwareengineeringke-codequalitytips

---

### 📰 Meta Description (SEO Optimized):

Are you a Kenyan developer still relying on heavy code comments to explain your logic? Discover why Uncle Bob says that’s a mistake—and learn how to write clean, self-documenting code that speaks for itself.

---

## 🧼 Why Uncle Bob Says Comments Are a Code Smell: A Kenyan Perspective

As a Kenyan developer, especially in local startups or freelance projects involving NHIF, MPESA, or eCitizen integrations, you’ve probably commented your code like this:

```plaintext
phpCopyEdit// This function calculates tax
function tx($x) {
  return $x * 0.16;
}
```

But if you follow **Uncle Bob (Robert C. Martin)**—the software legend behind *Clean Code*—you’ll realize that this isn’t clean at all.

> **“The best comment is no comment.” — Uncle Bob**

So why do developers in Kenya—and globally—still over-comment their code?

---

## 🇰🇪 Common Commenting Habits in Kenyan Developer Teams

From building eHealth solutions in counties like Kiambu, to integrating payments in Laravel or Flutter for SMEs in Nairobi, here’s what I’ve observed:

| Situation | What Kenyan Devs Do | Uncle Bob’s Recommendation |
| --- | --- | --- |
| Poor function naming | Use comments to explain logic | Use descriptive function names instead |
| Legacy codebases | Leave outdated comments | Refactor code to speak for itself |
| Fast client deadlines | Write dirty code, then patch with comments | Write clean, readable code from the start |

---

## 🧠 The Problem With Comments

Uncle Bob explains in [this video](https://www.youtube.com/watch?v=2a_ytyt9sf8) that **comments lie**, get outdated, and are a **bandage for unclear code**. Instead of commenting *what* the code does, you should **make the code readable**.

Here’s how to apply that in your day-to-day as a Kenyan developer:

---

## ✅ Commenting the Uncle Bob Way — With Real-World Examples

### ❌ Bad: Using Comments for Meaning

```plaintext
phpCopyEdit// Calculate NHIF deductions
function cnh($a) {
  return $a * 0.02;
}
```

### ✅ Good: Let the Code Speak

```plaintext
phpCopyEditfunction calculateNhifDeduction(float $grossSalary): float {
  return $grossSalary * 0.02;
}
```

Now even your intern in Kisumu or a fellow dev in Nakuru will understand it *without a single comment*.

---

## ✨ When Are Comments Acceptable?

According to Uncle Bob—and it applies in the Kenyan dev scene too—you can use comments when:

* Explaining **why** something is done a certain way (especially with local payment APIs like Safaricom Daraja)
    
* Marking **TODOs** or **FIXMEs**
    
* Adding **legal or licensing notes**
    

But avoid comments like:

```plaintext
phpCopyEdit// Loop through patients
foreach ($patients as $patient) {
    // Do something
}
```

---

## 💡 Pro Tip for Kenyan Devs:

Use **descriptive variables, small functions, and consistent naming** to eliminate the need for comments. This boosts:

* 🔍 SEO ranking of your GitHub or portfolio
    
* 👥 Team collaboration in agencies
    
* 🔐 Code quality in regulated sectors like healthcare and fintech
    

---

## 🎯 Conclusion

If you want to stand out in Kenya’s competitive tech ecosystem—from Nairobi’s iHub to Mombasa’s SwahiliBox—start writing **clean code, not commented code**.

Uncle Bob’s advice isn’t just theory—it’s practical. Clean code saves time, money, and reputation.

---

## 🔗 Resources for Further Learning

* 📺 [Uncle Bob’s YouTube Lesson on Functions](https://www.youtube.com/watch?v=2a_ytyt9sf8)
    
* 📘 *Clean Code* by Robert C. Martin
    
* 🇰🇪 Kenyan dev blogs on hashnode.com