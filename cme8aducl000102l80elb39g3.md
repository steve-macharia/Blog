---
title: "TDD: Writing Tests Before Code… Because Future You Hates Debugging"
datePublished: Tue Aug 12 2025 08:35:34 GMT+0000 (Coordinated Universal Time)
cuid: cme8aducl000102l80elb39g3
slug: tdd-writing-tests-before-code-because-future-you-hates-debugging
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1754987666075/6617d1f9-ff26-4ffe-bca4-f4bb15f05ec1.png

---

If you’ve ever spent an entire afternoon trying to figure out *why the thing you just wrote is now on fire*, then **Test-Driven Development (TDD)** might be your new best friend.

It’s not witchcraft, it’s not a cult (although it does have rules), and it might just save you from rage-quitting your next project.

---

## **What on Earth is TDD?**

TDD is a simple but disciplined development practice:

1. **Red** – Write a test that fails (because the feature doesn’t exist yet).
    
2. **Green** – Write the minimal code to make it pass.
    
3. **Refactor** – Clean the code without breaking the test.
    

Then… repeat. Over and over. Like flossing for your codebase.

---

## **A Quick History Lesson (Without Putting You to Sleep)**

* **1980s** – Smalltalk devs are already doing “test-first programming.”
    
* **1990s** – Kent Beck formalizes it as part of Extreme Programming (XP).
    
* **2002** – Beck publishes *Test-Driven Development: By Example*, and suddenly TDD is the cool kid in the Agile playground.
    

Fast forward to today, TDD is a favorite tool in professional software teams — and a secret weapon in **DevOps pipelines**.

---

## **Why Bother with TDD?**

Because:

* It forces you to **clarify requirements** before coding.
    
* You **catch bugs early**, before they become expensive therapy sessions.
    
* You **refactor without fear** (your tests have your back).
    
* You look like a wizard when code “just works” after refactoring.
    

---

## **TDD in DevOps: A Match Made in Automation Heaven**

DevOps loves **fast feedback** and **low drama**. TDD fits perfectly:

* **Shift-Left Testing** – Testing happens before code is written.
    
* **CI/CD Friendly** – Tests run automatically on every commit.
    
* **Safer Deployments** – No “we’ll fix it in production” disasters.
    
* **Infrastructure as Code** – Yes, you can even TDD your Kubernetes configs.
    

Think of TDD in DevOps as putting a **seatbelt on your code** before it races through the deployment pipeline.

---

## **Does Everyone Use TDD?**

Nope. For example, **OpenEMR** — a hugely popular open-source EMR — has solid automated testing with PHPUnit, API, and E2E tests. But they don’t explicitly follow TDD. Tests often come *after* the code.  
This is common in many projects — testing is there, but not necessarily test-first.

---

## **Let’s Try TDD in PHP (with PHPUnit)**

Here’s a quick **String Calculator Kata** in PHP, done TDD style:

### **Step 1 – Write the failing test (Red)**

```plaintext
phpCopyEdit<?php
use PHPUnit\Framework\TestCase;
require 'StringCalculator.php';

class StringCalculatorTest extends TestCase
{
    public function testEmptyStringReturnsZero()
    {
        $calc = new StringCalculator();
        $this->assertEquals(0, $calc->add(""));
    }

    public function testSingleNumberReturnsThatNumber()
    {
        $calc = new StringCalculator();
        $this->assertEquals(5, $calc->add("5"));
    }
}
```

Run it:

```plaintext
nginxCopyEditphpunit StringCalculatorTest.php
```

You’ll see:

```plaintext
sqlCopyEditFailed asserting that null matches expected 0.
```

Perfect — it’s failing.

---

### **Step 2 – Write the minimal code to pass (Green)**

```plaintext
phpCopyEdit<?php
// StringCalculator.php
class StringCalculator
{
    public function add($numbers)
    {
        if ($numbers === "") return 0;
        return (int) $numbers;
    }
}
```

Run tests again — they pass. 🎉

---

### **Step 3 – Refactor**

Code is already minimal, but as we add features, we’ll keep cleaning it up without breaking tests.

---

## **Getting Started with TDD**

1. Install PHPUnit:
    
    ```plaintext
    luaCopyEditcomposer require --dev phpunit/phpunit
    ```
    
2. Write your first failing test.
    
3. Write the code to make it pass.
    
4. Refactor — keep tests green.
    

---

## **Final Thoughts**

TDD isn’t about writing more tests — it’s about **designing with intention**.  
It might feel slow at first, but the payoff is huge: cleaner code, fewer bugs, and the sweet, sweet confidence of shipping without panic attacks.

If nothing else, remember:

> **Write tests first, so future you can spend more time coding and less time crying in the debugger.**