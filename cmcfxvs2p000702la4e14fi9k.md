---
title: "How ADOdb is Integrated into OpenEMR: The Hidden Backbone of Database Access"
datePublished: Sat Jun 28 2025 07:48:20 GMT+0000 (Coordinated Universal Time)
cuid: cmcfxvs2p000702la4e14fi9k
slug: how-adodb-is-integrated-into-openemr-the-hidden-backbone-of-database-access
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1751096809285/d79f77ba-c679-4721-94d0-f34fc526ec2a.png
tags: kenyahealthtech-digitalhealthkenya-emrdevelopmentke-openemrkenya-phpdeveloperske

---

A behind-the-scenes look at how OpenEMR uses the ADOdb library to simplify and abstract database operations across the entire application.

---

## 🩺 What is OpenEMR?

OpenEMR is a widely used, open-source electronic medical record (EMR) system designed for flexibility, extensibility, and global health deployment. While most developers focus on the UI or modules, few explore its powerful yet understated database architecture — **powered by ADOdb**.

---

## 📦 What is ADOdb?

[ADOdb (Active Data Objects for PHP)](https://adodb.org) is a database abstraction library that lets developers write uniform PHP code for multiple databases like MySQL, PostgreSQL, SQLite, and others — **without changing SQL syntax manually**.

It simplifies:

* Connections
    
* Parameterized queries
    
* Transactions
    
* Fetching results
    

ADOdb is especially useful in systems like OpenEMR where portability and long-term support matter.

---

## 🧠 Why OpenEMR Uses ADOdb

Instead of relying directly on PHP’s `mysqli` or `PDO`, OpenEMR integrates ADOdb to:

* Enable **multi-database support** (MySQL, MariaDB, PostgreSQL\*)
    
* Centralize SQL logic and error handling
    
* Simplify the developer interface (`sqlQuery()`, `sqlStatement()`)
    

> “Write once, run on any SQL engine — in theory and mostly in practice.”

---

### 1\. **Declared in composer.json**

```plaintext
jsonCopyEdit"require": {
  "adodb/adodb-php": "5.22.9"
}
```

Composer installs ADOdb into `/vendor/adodb/adodb-php`.

### 2\. **Bootstrapped via** `sql.inc`

```plaintext
phpCopyEditrequire_once($GLOBALS['srcdir'] . "/../vendor/adodb/adodb-php/adodb.inc.php");
$db = ADONewConnection($GLOBALS['db_type']);
$db->Connect(...);
```

### 3\. **Helper Functions Abstract It**

```plaintext
phpCopyEdit$sql = "SELECT * FROM patient_data WHERE pid = ?";
$res = sqlStatement($sql, [$pid]);
$row = sqlFetchArray($res);
```

These functions are wrappers for ADOdb under the hood.

### 4\. **Used Throughout the App**

Anywhere in OpenEMR you’ll find:

* `sqlQuery()`
    
* `sqlInsert()`
    
* `sqlStatement()`
    
* `sqlFetchArray()`
    

They’re all driven by ADOdb’s internal engine.

---

## 🐘 What About PostgreSQL?

Technically supported via ADOdb — but unofficial and **not production-tested**. Expect some issues with MySQL-specific SQL like `LIMIT`, `ON DUPLICATE KEY`, and schema definitions.

✅ Recommended: Stick to MySQL or MariaDB for stability.

---

## 💡 Why This Matters

As an implementer or EMR developer, understanding ADOdb:

* Helps you **debug data issues**
    
* Lets you **extend OpenEMR safely** (e.g., M-PESA integration)
    
* Keeps your modules **database-agnostic**
    

Even for Kenya-specific EMRs like JaliEMR, built on OpenEMR, this knowledge is key to safe billing, reporting, and financial integrations.

---

## 🛠️ Next Up?

I’ll explore how to:

* Build a secure, Composer-based M-PESA integration
    
* Log payments into OpenEMR using ADOdb
    

Stay tuned!

---

## 📬 Let’s Connect

Have you integrated OpenEMR into your local project? What database challenges have you faced?

Drop a comment below or reach out via [@steve-macharia](https://github.com/steve-macharia)!

---