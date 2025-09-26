Status: In development
Note: Everything here is subject to change

# SWCS — Secure Web Communication Standard (You need WAS as well)

> **SWCS** (Secure Web Communication Standard) is a next‑generation protocol layer and ecosystem for safe, private, and resilient web access. SWCS is implemented by **WAS** (Web Addressing Service) nodes and designed to make the web trustworthy again — ad‑free, integrity‑first, and user‑centric.

---

## Vision

SWCS aims to replace or augment traditional HTTP/HTTPS with a standard that **builds safety, integrity, and resilience into the communication layer**. In practice this means: fewer scams, less abuse, no trackers, and a web where users are nudged toward help when they need it.

This is not a polite suggestion. It’s a *middle finger* to the garbage that broke the web. Bc I dont like google :)

---

## Key Principles

* **Privacy-first:** no hidden tracking, no injected ads, no manipulative promotions.
* **Integrity-verified:** content tampering is detected and handled by default.
* **Resilient-by-design:** a mesh architecture plus mirrored central stores ensure the network keeps working under partial outages.
* **Safety-centric:** protocol-level protections for self-harm, toxic content, fake news, and malware.

---

## Features (short)

* WAS-based addressing and filtering
* Mesh networking with CloudLink / WebSockets
* Central rule store (WAS Bank) with mirrors
* CMD — Content Modification Detection (hashing + verification)
* Context analyzers scanning HTML *and linked content* (no script execution)
* Kid Safe Mode, permanent ban for suicide-how-to sites
* Soft interventions for self-harm content ("Maybe get some help")
* Fake news and toxic link flagging + "Are you sure?" prompt
* Ad-free browsing (no promotions or third-party injected content)
* GitHub integration for official rule updates
* Auto-sync, versioning, and rollback capabilities

---

## Architecture (overview)

SWCS defines both the **protocol behavior** and the **ecosystem architecture**. The main components are:

1. **WAS (Web Addressing Service)** — runtime nodes that enforce rules, run context analyzers, and serve or block content according to SWCS policy.
2. **CloudLink / WebSockets** — real-time messaging layer used for node synchronization and event distribution.
3. **WAS Bank** — the canonical rule & metadata store (ban lists, signatures, reputation). The WAS Bank can be mirrored.
4. **Mirrors** — independently hosted copies of the WAS Bank (for redundancy, local policy, or privacy). Mirrors sync with the canonical bank and sign updates.
5. **GitHub repo** — source of official rule bundles, analyzer models, and versioned releases. Nodes can fetch verified bundles from this repo.

Nodes are expected to operate offline-capably by caching rules and to reconcile via the bank or mirrors when they come back online.

---

## Security model

SWCS assumes active adversaries (malicious nodes, man-in-the-middle, poisoned content). Defenses include:

* **Signed rule bundles & hash verification** (reject unverified updates).
* **CMD (Content Modification Detection)** to detect tampering of content passing through nodes.
* **Node reputation & uptime monitoring** to isolate flaky or malicious nodes.
* **Mirrors with trust verification** to avoid accepting poisoned mirror updates.
* **Passive HTML scanning only** — SWCS nodes do not execute remote scripts during random fetches, reducing attack surface.

> *Threat model and mitigations must be explicitly expanded in any deployment.*

---

## Safety & UX rules

* **Kid Safe Mode**: blocks pornographic content, and tags non‑kid‑safe domains. Parents/administrators may add local overrides.
* **Permanent Ban**: domains dedicated to giving instructions for self‑harm are permanently banned (non‑appealable) and propagated across the network.
* **Self‑Harm Interventions**: content that appears harmful triggers a friendly interstitial: "Maybe get some help" with localized helplines and supportive redirects. The user can be offered an opt‑in escalation to get live help.
* **Toxic Links**: flagged links show an "Are you sure?" confirmation before navigation.
* **Fake News**: flagged content is annotated; high‑confidence malicious sources can be blocked.

All safety logic is multi‑factor: automated analyzers + network reputation.

---

## Developer / Integration Notes

### Message & sync patterns (example)

SWCS favors lightweight, JSON‑over‑WebSocket messages for real‑time events. Example: a node announcing a new ban:

```json
{
  "type": "ban.update",
  "source": "WAS-Bank",
  "signature": "<base64-sig>",
  "payload": {
    "domain": "badexample.tld",
    "reason": "self-harm-manual",
    "timestamp": "2025-09-26T12:00:00Z"
  }
}
```

Nodes must verify the `signature` against an agreed trust root before applying updates.

### Content scan results

Nodes SHOULD expose sanitized metadata rather than raw blocked content. Example:

```json
{
  "url": "https://example.tld/article",
  "flags": ["fake-news","toxic-link"],
  "summary": "This article makes unverified claims about X.",
  "safe_alternatives": ["https://trustednews.tld/summary"]
}
```

### Rules & updates

* Official rule bundles live as releases in the GitHub repo and are signed. ps: Not made that file yet
* Mirrors pull bundles and push anchor metadata to WAS Bank for discovery. ps: Not made that file yet
* Nodes may operate in permissive (inform-only) or strict (enforce) modes depending on deployment.

---

## Installation & usage (developer quickstart)

This repo is a specification and starter kit. Typical steps for a prototype WAS node:

1. `git clone https://github.com/TheManOnPi/SWCS---Secure-Web-Communication-Standard.git` ps: This is a place holder link 
2. Follow the `examples/` directory (TBD) to run a dev CloudLink server or connect to an existing one.
3. Load a signed rule bundle from `releases/` or point the node to a trusted mirror.
4. Start the WAS node; it will register with CloudLink and begin syncing.

---

## Governance & policy

SWCS defines safety constraints at the protocol level — but policy (who can ban, who can sign updates, how appeals are handled) is a social problem as much as a technical one. Suggested governance primitives:

* **Trusted Maintainers**: an initial set of keyholders who sign official bundles.
* **Proposal Workflow**: public pull requests + review + multisig release process.
* **Appeals & Rollback**: versioned bundles and easy rollback for false positives.

Document governance clearly in `GOVERNANCE.md` for any production deployment. ps: Not made that file yet

---

## Contributing

Contributions are welcome — follow these basics:

* Open issues for spec changes or feature requests.
* Use pull requests to propose edits to the spec; include rationale and tests/examples.
* Major changes should include an explicit migration/compatibility plan.

See `CONTRIBUTING.md` for detailed guidelines (TBD). ps: Not made that file yet

---

## Legal & License

This project is licensed under the **MIT License**. See the `LICENSE` file in the repo.

---

## Contact & Links

* Repo: [https://github.com/TheManOnPi/SWCS---Secure-Web-Communication-Standard](https://github.com/TheManOnPi/SWCS---Secure-Web-Communication-Standard)
* Issues & PRs: use the GitHub issue tracker.

---

## Appendix — Quick FAQ

**Q: Does SWCS execute remote scripts when scanning?**
A: No. SWCS nodes scan HTML and linked resources as text only — they do **not** execute remote JavaScript during random fetches.

**Q: What if my WAS node is offline?**
A: Other mesh nodes and mirrors cover protections. Nodes sync with WAS Bank or mirrors when reconnecting.

**Q: Can organizations run their own rules?**
A: Yes — mirrors support local extensions; nodes can be configured to prefer a local mirror for policy.

---

*Read this file as the canonical project introduction. For deeper protocol definitions, message formats, and module specs, consider adding `SPEC.md`, `GOVERNANCE.md`, and `IMPLEMENTATION_GUIDE.md`.* ps: these files are not done yet
