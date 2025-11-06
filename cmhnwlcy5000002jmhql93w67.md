---
title: "Tackling the DICOM Wall: Setting up a PACS (Orthanc) in a Kali/Ubuntu VM Lab"
datePublished: Thu Nov 06 2025 20:52:56 GMT+0000 (Coordinated Universal Time)
cuid: cmhnwlcy5000002jmhql93w67
slug: tackling-the-dicom-wall-setting-up-a-pacs-orthanc-in-a-kaliubuntu-vm-lab
tags: dicom-pacs-orthanc-healthcareit-medtech-cybersecurity-dicomnetworking-dcmtk-virtualmachine-linux

---

## Introduction: From Zero to DICOM Lab

Setting up a functional DICOM network is the first hurdle in healthcare IT, research, or security testing. We aimed to create a robust lab environment: a **Kali Linux VM** acting as a DICOM client (SCU/Scanner) and an **Ubuntu Host** running the popular open-source PACS server, **Orthanc** (SCP/PACS).

While the network connection (Host-Only Adapter) was solid, we hit two major walls: an elusive file compression error and a persistent security rejection. This guide documents the full troubleshooting process, culminating in a stable, two-way DICOM communication.

**Our Goal:** Successfully perform both **C-STORE** (sending data) and **C-FIND** (querying data) from the Kali VM to the Orthanc Host.

---

## 🚧 Part 1: Initial Success and the Compression Trap (C-STORE)

Our first step was to send an image using the DCMTK utility `storescu`.

### The Setup

* **PACS Server (Ubuntu Host):** IP `192.168.56.1`, AE Title `ORTHANC`, Port `4242`
    
* **Client (Kali VM):** IP `192.168.56.101`, AE Title `SCANNER_VM`
    

### Problem 1: Transfer Syntax Rejection

The initial attempt using the file `0020.DCM` failed immediately:

Bash

```plaintext
storescu 192.168.56.1 4242 -aec SCANNER_VM -aet ORTHANC 0020.DCM

# Error message showed:
# W: DIMSE Warning: ... unable to convert dataset from 'RLE Lossless' transfer syntax to 'Little Endian Explicit'
# E: Store Failed, file: 0020.DCM: DIMSE Failed to send message
```

The practical fix was to simply extract a known, uncompressed DICOM image (`IMG-0001-00001.dcm`) from a viewer archive to bypass the compression issue.

### ✅ C-STORE Solution: Using a Known Uncompressed File

The successful C-STORE command:

Bash

```plaintext
storescu 192.168.56.1 4242 -aec SCANNER_VM -aet ORTHANC IMG-0001-00001.dcm
# Output confirmed: I: Storage request successful (status: 0x0000)
```

**Verification:** The `Ultra_fast_Brain` study successfully appeared in the Orthanc web interface. **C-STORE success!**

---

## 🛑 Part 2: The C-FIND Security Wall

With C-STORE working, we attempted the crucial C-FIND (Query) to test two-way communication:

### The Failure: No Acceptable Presentation Contexts

The query failed, despite the previous successful C-STORE:

Bash

```plaintext
findscu -v 192.168.56.1 4242 -aec SCANNER_VM -aet ORTHANC -k PatientName="*"
# Output: E: No Acceptable Presentation Contexts
```

**Meaning:** Orthanc is blocking the C-FIND service due to strict security.

### The Solution: Three Configuration Fixes in `/etc/orthanc/orthanc.json`

This required editing the main Orthanc configuration file on the **Ubuntu Host** to fix permissions and network binding.

#### **1\. Peer Authorization (The AE Title Fix)**

We told Orthanc to trust the Kali VM's AE Title and IP:

JSON

```plaintext
"DicomPeers" : {
  "SCANNER_VM" : [ "192.168.56.101" ]
},
```

#### **2\. Enable Query/Retrieve Services (The Permission Fix)**

We explicitly enabled C-FIND for all authorized peers:

JSON

```plaintext
"QueryRetrieveSettings" : {
  "AllowFindSOPClass" : true,
  "AllowGetSOPClass" : true,
  "AllowMoveSOPClass" : true
},
```

#### **3\. Network Binding (The Host-Only Fix)**

Crucially, we forced Orthanc to listen on all interfaces (`0.0.0.0`) so it could receive the requests coming over the Host-Only network.

JSON

```plaintext
"DicomHost" : "0.0.0.0",
"HttpHost" : "0.0.0.0",
```

**Crucial Step:** Always `sudo service orthanc stop` then `sudo service orthanc start` after every configuration change!

---

## 🎉 Conclusion: Two-Way Communication Achieved

After implementing all three configuration fixes and ensuring the JSON syntax was perfect, the C-FIND command finally succeeded!

### Final Working Command and Output

Bash

```plaintext
findscu -v 192.168.56.1 4242 -aec SCANNER_VM -aet ORTHANC -k PatientName="*"

# Successful Output Snippet confirmed the patient data was retrieved!
# I: Received Find Response (Pending)
# ...
# I:   (0010,0010) PN [Ultra_fast_Brain]
# I: Received Find Response (Success)
```

The persistent `E: No Acceptable Presentation Contexts` error was ultimately a combination of security permissions and network binding—common pitfalls in DICOM networking that required diving deep into the server's configuration. Your lab environment is now fully functional!

---

#DICOM #PACS #Orthanc #HealthcareIT #MedTech #Cybersecurity #DICOMNetworking #DCMTK #VirtualMachine #Linux