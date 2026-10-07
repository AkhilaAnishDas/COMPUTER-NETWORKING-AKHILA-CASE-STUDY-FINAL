# Computer Networking & Cyber Security — Vaseworth Security Assessment

## Case Study No. 154

### Vaseworth Glassblowing Studio & Gallery

**Security Assessment, Firewalls, VPN & Hardening**

---

## Student Details

| Detail         | Information                          |
| -------------- | ------------------------------------ |
| **Name**       | Akhila Anish Das                     |
| **Roll No.**   | 150096725016                         |
| **Cohort**     | Larry Page                           |
| **Program**    | B.Tech CSE 2025–2029                 |
| **Semester**   | 3                                    |
| **Sprint**     | 1                                    |
| **Subject**    | Computer Networking & Cyber Security |
| **Case Study** | 154                                  |

---

# 1. Project Overview

Vaseworth Glassblowing Studio & Gallery operates a Head Office and three gallery locations connected through the public Internet.

During the previous month, four security incidents were observed:

1. A gallery switch detected multiple MAC addresses associated with the default-gateway IP.
2. The online gallery and commission-order server became unavailable after a large number of half-open TCP connections.
3. A gallery associate entered network credentials into a fake IT-support portal.
4. An administrator's credentials were exposed while using Telnet.

This case study analyzes these incidents and proposes a practical network security improvement plan.

The proposed solution focuses on:

* CIA Triad analysis
* Attack identification
* Firewall architecture
* Site-to-site IPSec VPN
* Router and switch hardening
* VLAN-based network separation
* Wireless security
* ACLs
* Staff security awareness

---

# 2. Problem Statement

Vaseworth requires a secure network architecture capable of protecting its Head Office, three gallery locations, online gallery services and commission-order records.

The existing incidents indicate risks involving:

* ARP spoofing and Man-in-the-Middle attacks
* SYN flood / Denial-of-Service
* Phishing and credential compromise
* Cleartext Telnet management traffic
* Unencrypted site-to-site communication
* Insufficient wireless network separation
* Weak network-device security

The objective is to identify these risks and recommend appropriate security controls using concepts covered in the Computer Networking & Cyber Security syllabus.

---

# 3. Objectives

The project addresses the following objectives:

### Objective 1 — Incident Analysis

Identify the attack technique involved in each incident and determine which CIA triad properties are affected.

### Objective 2 — Firewall Architecture

Recommend where a stateful firewall and a Next-Generation Firewall (NGFW) should be used and justify the decision based on risk and cost.

### Objective 3 — IPSec VPN

Design site-to-site IPSec VPN connections between the Head Office and each gallery, including IKE Phase 1 and Phase 2 parameters.

### Objective 4 — Device Hardening

Develop a router and switch hardening checklist covering:

* SSH
* AAA
* Legal warning banner
* Disabling Telnet
* Disabling unnecessary services
* Layer-2 security

### Objective 5 — Wireless Security

Design separate wireless security solutions for:

* Gallery staff
* Gallery visitors

and explain why the networks must use separate VLANs.

### Objective 6 — Staff Awareness

Prepare a simple security-awareness message addressing phishing and credential protection.

---

# 4. Incident Analysis

| Incident                                      | Attack                                | CIA Impact                             |
| --------------------------------------------- | ------------------------------------- | -------------------------------------- |
| Gateway IP mapped to multiple MAC addresses   | ARP Spoofing / MITM                   | Integrity, potentially Confidentiality |
| Large number of half-open TCP connections     | SYN Flood / DoS                       | Availability                           |
| Fake IT-support login portal                  | Phishing / Credential Compromise      | Confidentiality                        |
| Administrator credentials visible over Telnet | Cleartext Management Traffic Exposure | Confidentiality                        |

---

# 5. Proposed Security Architecture

The proposed architecture uses multiple layers of security.

### Internet Edge

A **Next-Generation Firewall (NGFW)** is recommended at the Internet-facing edge because the online gallery and commission-order services are exposed to public Internet traffic.

### Internal and Site Traffic

A **stateful firewall** can be used for predictable internal and gallery-to-HQ traffic where advanced application inspection is not required.

### Site-to-Site Communication

Each gallery establishes an **IPSec VPN tunnel** with the Head Office.

```text
             PUBLIC INTERNET
                    |
                 NGFW
                    |
              HEAD OFFICE
              /    |    \
             /     |     \
          IPSec  IPSec  IPSec
           VPN    VPN    VPN
           /       |       \
    Gallery 1  Gallery 2  Gallery 3
```

---

# 6. IPSec VPN Design

Three site-to-site VPN connections are proposed:

* Head Office ↔ Gallery 1
* Head Office ↔ Gallery 2
* Head Office ↔ Gallery 3

### IKE Phase 1

| Parameter        | Proposed Configuration |
| ---------------- | ---------------------- |
| Authentication   | Pre-Shared Key         |
| Encryption       | AES-256                |
| Hash / Integrity | SHA-256                |
| DH Group         | Group 14               |
| Lifetime         | 86400 seconds          |

### IPSec Phase 2

| Parameter  | Proposed Configuration |
| ---------- | ---------------------- |
| Encryption | AES-256                |
| Integrity  | SHA-256                |
| PFS        | DH Group 14            |
| Lifetime   | 3600 seconds           |

### Security Purpose

* **AES-256** → Protects confidentiality
* **SHA-256** → Provides integrity protection
* **DH Group 14** → Supports secure key exchange
* **Authentication** → Verifies VPN peers
* **IPSec** → Protects business traffic travelling across the public Internet

---

# 7. Device Hardening

Network devices should be hardened to reduce unauthorized access and protect management traffic.

The hardening plan includes:

### Secure Management

* Disable Telnet
* Enable SSH
* Use SSH version 2
* Use strong administrator credentials
* Configure AAA authentication

### Administrative Security

* Use privilege-based access
* Configure a legal warning banner
* Configure console authentication
* Apply session timeouts

### Service Security

* Disable unnecessary services
* Disable insecure management protocols
* Allow only required remote-management protocols

### Switch Security

* Port security
* MAC address restrictions
* DHCP snooping
* Secure unused ports
* VLAN separation

These controls directly address the Telnet and ARP-spoofing incidents.

---

# 8. Wireless Security Design

Vaseworth should operate two separate wireless networks.

## Staff Wi-Fi

**Recommended security:** WPA3

**Network:** Staff VLAN

Staff devices can access authorized internal business resources according to security policy.

## Guest Wi-Fi

**Recommended security:** WPA2 where broad device compatibility is required.

**Network:** Guest VLAN

Guest devices should receive Internet access but should not have direct access to internal business resources.

If all guest infrastructure and devices support WPA3, WPA3 can also be used.

---

# 9. Why Staff and Guest Wi-Fi Must Be Separate

Staff and visitors should never share the same VLAN.

A shared VLAN could allow untrusted visitor devices to interact at Layer 2 with internal devices.

Separate VLANs reduce the risk of:

* Unauthorized access
* Network scanning
* ARP-based attacks
* Traffic interception
* Access to internal servers
* Lateral movement

The proposed structure is:

```text
              SWITCH
              /    \
             /      \
      Staff VLAN    Guest VLAN
          |              |
     Staff Devices   Visitor Devices
          |              |
   Internal Access   Internet Only
```

---

# 10. ACL Security

ACLs can be used as an additional access-control layer.

Guest traffic should be restricted from reaching sensitive internal resources while allowing appropriate Internet access.

This follows the principle of **least privilege**.

---

# 11. Security Controls and Threats

| Threat                           | Security Control                                                |
| -------------------------------- | --------------------------------------------------------------- |
| ARP Spoofing                     | Port Security, MAC restrictions, DHCP Snooping, VLAN separation |
| SYN Flood                        | NGFW, Stateful Inspection, DoS/SYN protection                   |
| Phishing                         | Staff Awareness, Secure Authentication, AAA                     |
| Telnet Exposure                  | SSH and Telnet Disablement                                      |
| Internet Attacks                 | NGFW                                                            |
| Unencrypted Site Communication   | IPSec VPN                                                       |
| Guest Access to Internal Network | Separate Guest VLAN + ACL                                       |
| Unauthorized Switch Devices      | Port Security + Unused Port Shutdown                            |

---

# 12. Implementation Plan

### Phase 1 — Incident Response

* Investigate ARP spoofing activity
* Identify suspicious devices
* Secure affected switch ports
* Reset compromised credentials

### Phase 2 — Device Hardening

* Disable Telnet
* Enable SSH
* Configure AAA
* Configure warning banners
* Disable unnecessary services
* Secure unused switch ports

### Phase 3 — Network Security

* Configure Staff VLAN
* Configure Guest VLAN
* Apply switch-security controls
* Configure DHCP snooping
* Apply ACL policies

### Phase 4 — Firewall Security

* Deploy NGFW at Internet edge
* Protect public-facing services
* Configure stateful policies
* Restrict unnecessary inbound traffic

### Phase 5 — VPN

* Configure HQ VPN endpoint
* Configure Gallery VPN endpoints
* Configure IKE Phase 1
* Configure IPSec Phase 2
* Verify VPN connectivity

### Phase 6 — Awareness

* Conduct phishing awareness training
* Explain credential protection
* Establish suspicious-email reporting procedures

---

# 13. Verification

The proposed configuration can be verified using appropriate network-device commands and testing procedures.

Verification areas include:

* SSH status
* VTY configuration
* AAA configuration
* VLAN configuration
* Port security
* DHCP snooping
* Trunk configuration
* IKE security associations
* IPSec security associations

Testing should confirm that:

* Telnet access is blocked.
* SSH access works for authorized administrators.
* Guest devices cannot access protected internal resources.
* Staff devices can access permitted resources.
* Unauthorized switch devices trigger security controls.
* HQ-to-gallery traffic uses the IPSec VPN.

---

# 14. Expected Security Improvements

The proposed architecture improves Vaseworth's security in several areas.

### Confidentiality

Protected through:

* IPSec encryption
* SSH
* WPA3
* Secure authentication
* Guest network isolation

### Integrity

Protected through:

* SHA-256 integrity mechanisms
* Layer-2 security
* VLAN segmentation
* Controlled network access

### Availability

Improved through:

* NGFW deployment
* Stateful inspection
* SYN-flood/DoS protection
* Controlled Internet traffic

---

# 15. Key Learning Outcomes

This case study demonstrates the practical application of Computer Networking & Cyber Security concepts.

The project applies:

* CIA Triad
* Common attack types
* ARP and MITM
* DoS/SYN flood
* Firewalls
* NGFW
* VPN
* IPSec
* IKE
* VLANs
* ACLs
* Switch security
* SSH
* AAA
* Wireless security
* Network hardening

---

# 16. Deliverables

The project contains the following required deliverables:

* Incident-to-CIA Triad mapping table
* Firewall architecture recommendation
* Firewall placement and justification
* Site-to-site IPSec VPN design
* IKE Phase 1 parameters
* IPSec Phase 2 parameters
* Device-hardening checklist
* Wireless security design
* Staff vs Guest VLAN separation
* Staff-awareness note
* Security improvement plan

---

# 17. Project Files

```text
Vaseworth-CN-Cyber-Security/
│
├── README.md
│
├── Vaseworth_CN_Cyber_Security_Report.pdf
│
└── Project_Documentation/
    └── Security_Assessment_Report.pdf
```

---

# 18. Conclusion

The Vaseworth case study demonstrates how cybersecurity controls can be applied to real-world networking problems.

The proposed solution uses a **risk-based and layered security approach**. An NGFW protects the Internet-facing edge, stateful firewall controls protect predictable network traffic, IPSec VPNs secure communication between the Head Office and galleries, and device hardening protects network infrastructure.

Separate Staff and Guest VLANs improve network isolation, while WPA3 and secure authentication strengthen wireless and administrative access.

Finally, staff awareness is an important part of the security architecture because technical controls alone cannot completely prevent phishing and credential compromise.

The proposed solution therefore provides a practical approach to improving Vaseworth's **Confidentiality, Integrity and Availability** while remaining aligned with the concepts covered in the Computer Networking & Cyber Security syllabus.

---

## Student

**Akhila Anish Das**
**Roll No.: 150096725016**
**Cohort: Larry Page**
**B.Tech CSE 2025–2029**
**Semester 3 | Sprint 1**
