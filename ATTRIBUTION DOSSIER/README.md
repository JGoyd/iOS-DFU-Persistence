# Stepped-On Silicon: Post-DFU Certificate Persistence
## iOS Device Activation Certificates Survive Factory Reset

**TL;DR:** Device activation certificates containing IMEI, MAC address, UDID, and serial number in plaintext survive DFU restore via hardware-level protection. Certificate generated DURING DFU mode, proving pre-knowledge of activation. Taiwan Mobile infrastructure leverages persistence for unauthorized MDM re-enrollment.

---

## The Finding

**Date:** January 9, 2026  
**Location:** Apple Store, Atlanta, GA, USA, Lenox Mall
**Action:** DFU (Device Firmware Update) restore  
**Result:** All surveillance infrastructure survived

**Forensic Evidence:**
```
File:    sysdiagnose_2026.01.09_14-48-25-0500_iPhone-OS_iPhone_23C55.tar.gz
SHA-256: 4d9bb243f983ce7f6ec77f7bd95fd2b3cb797133777d770625b12e1cff5eec3d
Download: https://drive.proton.me/urls/8ZK08NWN3R#Yhjcx5HOopn4
```

---

## Timeline

```
14:39:11 EST - DCRT activation certificate ISSUED by FEDRCDC-UCRT-SUBCA
               (Device in DFU mode at Apple Store - NO operating system)
               
14:48:25 EST - DFU restore COMPLETES

14:48:25 EST - sysdiagnose captured (0-3 seconds post-completion)

14:49:08 EST - DCRT certificate DELIVERED to device
               (Certificate survived factory reset)
```

**Gap:** Certificate pre-generated 9 minutes 57 seconds before delivery

---

## Three-Tier Certificate Chain

```
SEP Root CA (Hardware - Secure Enclave, pre-installed)
    ↓
FEDRCDC-UCRT-SUBCA (Activation Authority)
    ↓
Device DCRT Leaf (Serial: 01:9B:A4:4E:45:FA)
    Contains: IMEI/MAC/UDID/Serial in plaintext
```

All three certificates survived DFU via silicon-enforced disk1s8 "protect" flag.

**Critical Privacy Violation:** Certificate embeds IMEI, MAC, UDID, Serial in plaintext - enables permanent tracking, cannot be changed.

---


## Verification

```bash
curl -L -o sysdiagnose.tar.gz https://drive.proton.me/urls/8ZK08NWN3R#Yhjcx5HOopn4
shasum -a 256 sysdiagnose.tar.gz  # Expected: 4d9bb243f983ce7f...
tar -xzf sysdiagnose.tar.gz
find . -name "0000000000000005.tracev3"  # Contains DCRT certificate
```

---

## Implications

DFU restore is NOT complete erasure. Activation certificates with IMEI/MAC/UDID survive via hardware protection. Enables permanent tracking and MDM re-enrollment.



---

**License:** Public Disclosure | **Date:** January 12, 2026
