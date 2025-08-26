---
title: "*Automating Pentests with Nmap + Metasploit: My pentest_orchestrator.sh Workflow*"
datePublished: Tue Aug 26 2025 11:19:04 GMT+0000 (Coordinated Universal Time)
cuid: cmesge1lo000702i9fvgc22ty
slug: automating-pentests-with-nmap-metasploit-my-pentestorchestratorsh-workflow
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1756206888763/6f85ed1e-c5ae-4a6d-94e0-b4469f669917.png

---

Pentesting often involves repetitive tasks: scanning targets, importing results, and looking for exploit paths. I got tired of running Nmap and then manually importing into Metasploit, so I built a small script — `pentest_`[`orchestrator.sh`](http://orchestrator.sh) — that automates the entire workflow.

In this post, I’ll walk you through how it works and how you can use it for your own ethical hacking labs.

---

## 🔹 Why Automate the Workflow?

The traditional pentesting flow looks like this:

1. Run **Nmap** to scan the target.
    
2. Save the results (usually in XML).
    
3. Import them into **Metasploit’s database**.
    
4. Run exploit suggestion modules.
    

That’s 4–5 manual commands, every time. Instead, I wanted:

```plaintext
sudo ./pentest_orchestrator.sh <target_ip> --quick
```

And boom 💥 — everything from scanning to exploit suggestions happens automatically.

---

## 🔹 Running a Scan on [Localhost](http://Localhost)

Here’s a sample run against my [localhost](http://localhost) (`127.0.0.1`):

```plaintext
sudo ./pentest_orchestrator.sh 127.0.0.1 --quick
```

The script:

* Checks **Metasploit ↔ PostgreSQL** connection
    
* Runs an Nmap scan
    
* Imports results into Metasploit
    
* Generates exploit suggestions
    

Example output:

```plaintext
[*] Checking Metasploit ↔ PostgreSQL connectivity...
[*] Connected to msf. Connection type: postgresql.

[*] Running Nmap on 127.0.0.1...
5432/tcp open  postgresql PostgreSQL DB 9.6.0 or later
...
[*] Successfully imported /home/kali/nmap_quick.xml
[✓] Pentest Orchestration Complete.
```

Now my Metasploit database shows:

```plaintext
msfconsole
msf6 > hosts
msf6 > services
```

And it’s ready for exploitation.

---

## 🔹 Applying It on Web IPs

Yes, you can also point this at a **web server IP**:

```plaintext
sudo ./pentest_orchestrator.sh <web_ip>
```

If ports `80` or `443` are open, Metasploit will suggest HTTP modules. If `5432` (Postgres) is open, it’ll suggest database modules.

⚠️ Important: Only scan systems you own, or have explicit permission to test (lab machines, bug bounty scopes, client pentests).

---

## 🔹 What’s Next After Scanning?

Once you have services imported into Metasploit, you can try:

**Checking web service version:**

```plaintext
use auxiliary/scanner/http/http_version
set RHOSTS <target_ip>
run
```

**Testing Postgres default creds:**

```plaintext
use auxiliary/scanner/postgres/postgres_login
set RHOSTS <target_ip>
set USERNAME postgres
set PASSWORD postgres
run
```

**If successful:**

```plaintext
use exploit/multi/postgres/postgres_copy_from_program_cmd_exec
```

---

## 🔹 Try It on Safe Targets

Instead of hitting random IPs on the internet (illegal 🚫), use:

* **Metasploitable2 VM** (intentionally vulnerable Linux VM)
    
* **DVWA (Damn Vulnerable Web App)**
    
* Online labs like **TryHackMe** or **HackTheBox**
    

That way, you can test safely.

---

## 🔹 Conclusion

My `pentest_`[`orchestrator.sh`](http://orchestrator.sh) script turns a clunky workflow into a one-liner:

```plaintext
sudo ./pentest_orchestrator.sh <target_ip> --quick
```

Scan → Import → Suggest → Ready for exploitation.

If you’re building your own pentesting toolkit, automation like this saves time and keeps your recon organized inside Metasploit.