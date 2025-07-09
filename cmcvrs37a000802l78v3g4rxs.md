---
title: "How I Used Pest for EMR Unit Testing — Pros, Cons, and Gaps"
datePublished: Wed Jul 09 2025 09:41:49 GMT+0000 (Coordinated Universal Time)
cuid: cmcvrs37a000802l78v3g4rxs
slug: how-i-used-pest-for-emr-unit-testing-pros-cons-and-gaps

---

Electronic Medical Record (EMR) systems are notoriously complex — from patient demographics to drug interactions, everything must be tested with care and precision. When I began writing unit tests for our EMR project (based on OpenEMR), I chose [**Pest**](https://pestphp.com) — a modern PHP testing framework — to simplify and scale our testing strategy.

In this post, I’ll walk you through **how I used Pest**, what **worked great**, what **could be better**, and why I still believe it's an excellent fit for PHP-based health software.

---

## ⚙️ The Setup: Pest + OpenEMR (Jali-EMR)

Our EMR system is a customized fork of [OpenEMR](https://github.com/openemr/openemr), running PHP 8.2 with a Dockerized architecture. Pest was added as a dev dependency:

```plaintext
bashCopyEditcomposer require pestphp/pest --dev
./vendor/bin/pest --init
```

I used Pest to write unit tests for:

* Patient registration and validation logic
    
* Encounter and observation logic
    
* Allergy checking (like beta-lactam detection)
    
* Backend services like report generation
    
* Auth/permissions checks (roles like nurse, doctor, admin)
    

---

## ✅ The Pros of Pest for EMR Unit Testing

### 1\. ✨ **Clean, Readable Syntax**

Compare this:

```plaintext
phpCopyEdittest('A new patient is assigned an ID', function () {
    $patient = registerPatient(['name' => 'John']);
    expect($patient->id)->not->toBeNull();
});
```

To this (typical PHPUnit):

```plaintext
phpCopyEditpublic function testPatientIdAssigned() {
    $patient = registerPatient(['name' => 'John']);
    $this->assertNotNull($patient->id);
}
```

With Pest, my **tests read like documentation**, which matters when onboarding new devs in healthcare.

---

### 2\. ⚡ **Fast Test Execution**

Pest loads faster, and with `--parallel`, it reduced our test suite runtime from **58 seconds to 21 seconds**. On an EMR system with lots of I/O and database mocking, that’s a game-changer.

---

### 3\. 🧪 **Elegant Data-Driven Tests**

For example, allergy detection across similar drug names:

```plaintext
phpCopyEdittest('detects drug class allergy', function ($drug) {
    expect(detectAllergy($drug))->toBeTrue();
})->with([
    'Penicillin',
    'Amoxicillin',
    'Cloxacillin',
]);
```

In EMRs, where many rules depend on clinical equivalency, Pest's `->with()` makes such tests clean and scalable.

---

### 4\. 🔁 **Reusable Hooks (beforeEach/afterEach)**

```plaintext
phpCopyEditbeforeEach(function () {
    $this->patient = createTestPatient();
});
```

This reduced boilerplate across tests and helped isolate test cases involving multiple encounters or appointments.

---

### 5\. 🔌 **Friendly with Legacy Code**

Even though OpenEMR isn’t written with modern DDD or TDD in mind, Pest’s compatibility with PHPUnit made it easy to wrap legacy classes and test them.

```plaintext
phpCopyEdituses(Tests\TestCase::class);
```

I could test `PatientService`, `AllergyEngine`, and even `BillingManager` without changing their internal structure.

---

## ❌ The Cons (A Few Rough Edges)

### 1\. 🛠️ No Built-in Mocking (yet)

Pest doesn’t come with mocking or spying tools. You must use:

* PHPUnit mocks
    
* Third-party packages like Mockery
    

For example, mocking a billing service:

```plaintext
phpCopyEdit$mock = Mockery::mock(BillingService::class);
$mock->shouldReceive('charge')->once()->andReturn(true);
```

Not a deal-breaker, but worth noting.

---

### 2\. 🧱 Laravel Bias in Docs

Although Pest works fine in raw PHP and Symfony, much of its documentation and examples assume **Laravel**. In an EMR system where Laravel may not be used, I had to dig a bit more.

---

### 3\. 🧰 Test Discovery in Large Projects Can Be Tricky

When tests are deeply nested (e.g., `tests/Modules/Clinical/Allergy`), Pest’s default test discovery sometimes misses files if the naming convention isn’t followed precisely.

Solution: follow naming conventions like `*.test.php`.

---

## 🕳️ Gaps in Pest for EMR Testing

These are **not bugs**, but areas where Pest doesn’t fully solve things out-of-the-box in a medical software context:

| Gap | Why It Matters |
| --- | --- |
| 🧪 No built-in database transaction handling | In medical systems, test isolation is critical. Laravel handles this with `RefreshDatabase`, but plain PHP doesn't. |
| ❌ No built-in test coverage enforcement | You must wire in `phpdbg` or Xdebug manually and parse results. |
| 🔐 No built-in user role simulation | EMRs depend heavily on role-based permissions. Pest doesn’t offer helpers like `asDoctor()` or `asAdmin()` — you must build this yourself. |
| 📄 No medical-specific matchers | No `expectDiagnosis()->toBeValidICD10()` — domain-specific matchers would help! |

---

## 💭 Final Thoughts

Despite the few limitations, Pest is **elegant, fast, and expressive**. In an EMR context where codebases can be large, older, or heavily regulated, Pest provided us with a **modern testing toolkit** that made testing **fun and maintainable**.

We still use PHPUnit underneath, but Pest helped us **write more tests, faster**, and that means **fewer bugs** in production — which matters when lives are involved.

---

## 🧠 Bonus Tips

* Use Pest in CI/CD for test gating before deployments.
    
* Write your own custom expectations (e.g., `expect($visit)->toBeCompleted()`).
    
* Combine Pest + `phpmetrics` to get a real feel of test quality.
    

---

## 📚 Resources

* Pest Official Docs
    
* [Mockery (for mocking)](https://github.com/mockery/mockery)
    
* [PHPUnit for compatibility](https://phpunit.de/)
    
* [OpenEMR Testing Wiki](https://github.com/openemr/openemr/wiki)