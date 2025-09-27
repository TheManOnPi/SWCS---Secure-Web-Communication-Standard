# WASfS — Web Addressing Service for SWCS

> **WAS** (Web Addressing Service) is the core runtime component of SWCS, responsible for enforcing content rules, context analysis, and network synchronization across nodes.

---

## Overview

WAS provides **secure, ad-free, and safe web access** under SWCS. It is a mesh-networked service with optional mirrored central stores (WAS Bank) to ensure consistent and resilient protection even if some nodes go offline.

WAS is designed to scan content, enforce safety policies, and coordinate updates across the SWCS network without executing remote scripts, making it a low-risk but highly effective safety layer.

---

## Core Features

### Security & Integrity

* **CMD (Content Modification Detection):** detects and flags tampered content.
* **Context analyzers:** scan HTML and linked content for malicious, toxic, or harmful material.
* **Uptime monitoring:** ensures nodes are reliable; malfunctioning or compromised nodes are flagged.
* **Random site fetching:** preemptively fetches external content to disrupt malware targeting.
* **GitHub integration:** fetches official rule bundles and analyzer updates.
* **Hash verification:** ensures integrity of downloaded rules and content.

### User Safety

* **Kid Safe Mode:** blocks pornography, flags non-kid-safe domains.
* **Permanent bans:** suicide or self-harm instructional sites are blocked network-wide.
* **Self-harm intervention:** redirects to supportive resources with optional escalation.
* **Helpline whitelisting:** ensures all official support resources remain accessible.
* **Toxic link warnings:** “Are you sure?” prompts for flagged sites.
* **Fake news detection:** flags or blocks misinformation.
* **Ad-free:** no promotions or third-party tracking.

### Network & Synchronization

* **Mesh network:** nodes cover each other; no single point of failure.
* **WAS Bank:** central database of rules, bans, and reputation.
* **Mirrors:** optional local copies of WAS Bank for redundancy and local policy control.
* **Auto-sync & versioning:** nodes reconcile missed updates, and rollbacks are supported.
* **CloudLink/WebSockets:** secure messaging layer for inter-node coordination.

### Updates & Maintenance

* Nodes fetch signed updates from GitHub or mirrors.
* Context analyzers and CMD rules are versioned to prevent regression.
* Offline operation is supported with caching; syncing occurs once the node reconnects.

---

## Developer Notes

* WAS nodes are **read-only for web content** (do not execute scripts).
* Nodes can operate in **strict** (enforce) or **permissive** (inform-only) mode.
* Local configuration allows integration with specific organizational policies or custom rule sets.
* All network messages (update propagation, ban announcements) are signed to prevent tampering.

---

## Philosophy

* WAS embodies SWCS's mission: **clean, safe, resilient web access.**
* It actively blocks harmful content, fake news, and malware while maintaining **privacy-first, ad-free browsing.**
* Mesh networking and mirrored WAS Bank instances ensure the system is **resilient, decentralized, and user-focused.**

---

## Contact & Resources

* GitHub repo: [https://github.com/TheManOnPi/SWCS---Secure-Web-Communication-Standard](https://github.com/TheManOnPi/SWCS---Secure-Web-Communication-Standard)
* Issues & contributions: via GitHub tracker
* License: MIT
