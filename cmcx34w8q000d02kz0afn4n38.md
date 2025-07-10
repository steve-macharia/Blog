---
title: "Strict Types in PHP: Like a Bouncer for Your Arguments"
datePublished: Thu Jul 10 2025 07:47:29 GMT+0000 (Coordinated Universal Time)
cuid: cmcx34w8q000d02kz0afn4n38
slug: strict-types-in-php-like-a-bouncer-for-your-arguments
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1752133608439/24066846-0046-4af1-ab1e-2390264555a9.png

---

## 💬 Introduction

PHP is famously loose when it comes to types. You can pass a string where a number is expected, and PHP just shrugs and says, “Sure, I’ll make it work.”

But that kind of flexibility comes at a cost: silent bugs, unpredictable behavior, and developers crying at 2 AM.

Enter `strict_types=1` — PHP’s equivalent of a bouncer at the club door. If your data isn't on the guest list (i.e., wrong type), it’s not getting in.

In this blog, we’ll dissect how `strict_types` works, what it *actually* enforces, and how to use it to bulletproof your PHP functions and return types. Warning: type jugglers will not be allowed past this point.

---

## 🛠️ What is `strict_types`?

In PHP 7 and above, you can specify **scalar type declarations** (like `int`, `float`, `string`, `bool`) for function parameters and return values. But by default, PHP still performs **type coercion**.

### 🔄 Coercive Mode (Default)

```plaintext
phpCopyEditfunction multiply(int $a, int $b): int {
    return $a * $b;
}

echo multiply("5", "4"); // Output: 20
```

No complaints from PHP. It converts `"5"` and `"4"` into integers silently.

---

### 🛑 Strict Mode

Add this to the **top** of your file:

```plaintext
phpCopyEdit<?php
declare(strict_types=1);
```

Now PHP acts like that strict bouncer:

```plaintext
phpCopyEditfunction multiply(int $a, int $b): int {
    return $a * $b;
}

echo multiply("5", "4"); 
// ❌ TypeError: Argument #1 must be of type int, string given
```

No coercion. No mercy. You either pass the right type, or you get kicked out.

---

## 🧪 Under the Hood: Coercion vs Strict Typing

PHP values are internally stored as `zval` structures, which contain the actual value and its type (e.g., `IS_STRING`, `IS_LONG`).

In **coercive mode**, PHP will perform automatic type juggling at the time of function calls. In **strict mode**, PHP throws a `TypeError` if the types don’t match exactly.

| Mode | `"5"` passed to `int $a` | Behavior |
| --- | --- | --- |
| Default | Allowed (converted to `5`) | Works silently |
| `strict_types=1` | Not allowed | Throws `TypeError` |

---

## 🧰 Return Types are Enforced Too

```plaintext
phpCopyEditdeclare(strict_types=1);

function half(int $value): int {
    return $value / 2;
}
```

Even though this looks fine, `3 / 2 = 1.5`, which is a **float**, and your function promises to return an **int**.

**💥 Result:**

```plaintext
pgsqlCopyEditFatal error: Uncaught TypeError: Return value must be of type int, float returned
```

### ✅ Fix:

```plaintext
phpCopyEditreturn (int)($value / 2); // manually cast
```

---

## 📦 Real-World Example

Imagine you’re working on a health record system like [Jali-EMR](https://github.com/Jali-EMR). A function expects a patient ID as an `int`, but someone accidentally passes `"42 "` (a string with a space).

Without strict types:

```plaintext
phpCopyEditgetPatient(int $id) { ... }
getPatient("42 "); // still works? 🤷‍♂️
```

Until it fails in production when `"42 "` is used in a DB query — and it **doesn’t match any record**.

With strict types:

```plaintext
phpCopyEdit// TypeError before it hits your DB — exactly what you want.
```

---

## 🧵 Best Practices

1. **Always add** `declare(strict_types=1);` at the top of every PHP file.
    
2. **Use scalar and return type hints consistently:**
    
    ```plaintext
    phpCopyEditfunction calculateBMI(float $weight, float $height): float { ... }
    ```
    
3. **Avoid relying on PHP’s type juggling** — it leads to edge-case bugs.
    
4. **Use static analysis tools** like:
    
    * [PHPStan](https://phpstan.org/)
        
    * [Psalm](https://psalm.dev/)
        
    * [Intelephense](https://marketplace.visualstudio.com/items?itemName=bmewburn.vscode-intelephense-client)
        

---

## 🔄 Inheritance, Interfaces, and `strict_types`

PHP enforces that overriding methods **must** match parameter and return types **exactly** in strict mode:

```plaintext
phpCopyEditinterface Engine {
    public function start(string $key): bool;
}

class DieselEngine implements Engine {
    public function start(string $key): bool {
        return true;
    }
}
```

Try returning `int` instead of `bool` in the subclass, and PHP will **crash your inheritance party**.

---

## ⚠️ Gotchas

1. `strict_types` is per-file, not global.
    
    * If File A calls a function in File B, only File B’s declaration matters for strictness.
        
2. **Built-in PHP functions always use coercion**, even in strict mode.
    
    ```plaintext
    phpCopyEditstrlen("123") // always works
    ```
    
3. **Be cautious with mixed types**:
    
    * `function process($x)` still allows anything unless typed.
        

---

## 🧠 Final Thoughts

`strict_types=1` isn’t about making PHP harder. It’s about making bugs easier to prevent.

It's like a bouncer: annoying at first, but you'll thank it when it keeps the wrong types out of your system.

If you're building APIs, healthcare software, fintech apps, or anything critical — **enable strict types**. It’s your first line of defense against chaos.

---

## 📚 Further Reading

* [PHP RFC: Scalar Type Declarations](https://wiki.php.net/rfc/scalar_type_hints)
    
* [PHP RFC: Return Type Declarations](https://wiki.php.net/rfc/return_types)
    
* [PHP Manual on Type Declarations](https://www.php.net/manual/en/functions.arguments.php#functions.arguments.type-declaration)
    
* [Pest Testing Framework](https://pestphp.com)
    
* [PHPStan Static Analysis Tool](https://phpstan.org)
    

---

## 👨‍💻 Author

**Stephen Macharia**  
EMR Developer @ Medby-Tech  
📚 Read more at stephen-macharia.hashnode.dev  
🔗 [Jali-EMR GitHub](https://github.com/Jali-EMR)