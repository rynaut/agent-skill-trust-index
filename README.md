# Agent Skill Trust Index

**An opinionated, compliance-first index of the AI agent skill security ecosystem — plus a public rubric for scoring any skill on trust before you install it.**

> Most skill directories rank on popularity and usefulness. This one doesn't.
> It exists to answer one question: **is this skill safe to give an autonomous agent ambient authority over your file system, secrets, and network?**

Maintained by [Rynaut — Architecting Automation](https://www.youtube.com/@ArchitectingAutomation).
Principle-first. Framework-agnostic. Vendor-neutral.

---

## Why this exists

When you install an agent skill, you don't approve a tool — you grant persistent, ambient authority to read files, run shell commands, and open network connections, often without continuous oversight. That gap between *what you think you authorized* and *what the skill can actually do* is the core risk.

Large-scale analysis of production skills has found that roughly **one in four carries at least one security defect**, and that skills bundling executable scripts are meaningfully more likely to be vulnerable than instruction-only ones. Coordinated poisoning campaigns have already weaponized this at scale.

So the useful question isn't "is this skill popular?" It's "does this skill earn trust before it gets it?" This index is built entirely around that question.

> ⚠️ **Disclaimer:** This index is research-assisted and curated by an independent author. Tool descriptions and any ratings are summaries, not audits, and may be incomplete or out of date. **Verify every claim against primary sources before acting.** Nothing here is a security guarantee or professional advice.

---

## The Trust Rubric

Any skill can be scored on nine axes. Each axis is rated **Pass / Caution / Fail**. The lowest individual rating caps the overall tier — a single Fail on a credential-handling axis is not offset by passing everything else.

| # | Axis | The question it answers |
|---|------|--------------------------|
| 1 | **Provenance & Identity** | Is the author/source verifiable? Is the package signed or attested? |
| 2 | **Declared vs Actual** | Does the documented behavior match what the code actually does? Any undocumented "shadow" capability? |
| 3 | **Least Privilege** | Does it request only the access its stated purpose needs — or far more? |
| 4 | **Code Safety** | Bundled executable scripts? `exec`/`eval`/`subprocess`, obfuscation, or `curl \| bash`? |
| 5 | **Data Handling** | Any path from secrets (env vars, key files) to a network sink? |
| 6 | **Instruction Integrity** | Hidden instructions in markdown, comments, or tool metadata? Prompt-injection / tool-poisoning patterns? |
| 7 | **Dependency Hygiene** | Pinned versions? Known CVEs? Actively maintained? |
| 8 | **Tamper Resistance** | Can the package be silently swapped *after* you approve it? Is integrity bound to content, not just file identity? |
| 9 | **Compliance Fitness** | Is it documented, auditable, and suitable for a regulated or high-assurance environment? |

### Trust Tiers

| Tier | Meaning | Condition |
|------|---------|-----------|
| 🟢 **Verified** | Earns trust. Safe to install in most contexts. | No Fail; at most one Caution |
| 🟡 **Caution** | Review before installing. Acceptable with mitigations. | No Fail; multiple Cautions |
| 🔴 **Avoid** | Do not install without remediation. | Any Fail on axes 1, 5, or 6 |

Full methodology and scoring guidance: see [`SCORING.md`](./SCORING.md).
Entry format for contributions: see [`TEMPLATE.md`](./TEMPLATE.md).

---

## The Ecosystem Index

These are the **tools, frameworks, and standards** that help you evaluate skill trust across the lifecycle. Each is annotated by *what it covers* and *where it fits* — not given a safety score (these are the instruments, not the skills under test).

> Links and capabilities below are summarized from public sources and should be verified independently.

### 🔍 Pre-install scanners

| Tool | Approach | Lifecycle stage |
|------|----------|-----------------|
| **SkillSpector** (NVIDIA) | Two-stage: static AST + dependency analysis, then optional LLM semantic evaluation; taint tracking for chained exfiltration paths | Pre-install |
| **SkillSieve** | Hierarchical triage: cheap static filter → structured semantic decomposition → multi-model adjudication for the highest-risk cases | Pre-install / registry scale |
| **SkillProbe** | Multi-agent collaborative auditing; maps declared capability against actual implementation to surface shadow functions | Pre-install / alignment |

### 🔐 Provenance & runtime integrity

| Tool / Standard | Approach | Lifecycle stage |
|-----------------|----------|-----------------|
| **Verified-skill signing** (e.g. detached signatures + machine-readable skill cards) | Cryptographic provenance: ownership, dependencies, limitations, verification status | Publish / install |
| **Audit-runtime sealing** (e.g. SIGIL-style verification loaders) | Binds executed code to audited code; blocks post-approval tampering | Runtime |

### 📚 Evidence & standards

| Resource | What it gives you |
|----------|-------------------|
| **OWASP Agentic Skills Top 10** | Reference taxonomy for skill-layer risks |
| **OSV.dev** | Free, unauthenticated CVE lookups for dependencies |
| **"Agent Skills in the Wild" (Liu et al., 2026)** | The 26.1% / 2.12x empirical baseline |
| **"Malicious Agent Skills in the Wild" (Liu et al., 2026)** | Ground-truth attacker behavior and archetypes |

*(Add the canonical URL for each entry when you publish; left out here so nothing ships unverified.)*

---

## Worked Example — applying the rubric

*Illustrative only. Not a real skill. Shows how a Fail propagates.*

**`auto-deploy-helper` (hypothetical)** — "Automates your deploy pipeline."

| Axis | Rating | Note |
|------|--------|------|
| Provenance & Identity | 🟡 Caution | Unsigned, single-maintainer |
| Declared vs Actual | 🔴 Fail | Reads `~/.aws/credentials` — never mentioned in docs |
| Least Privilege | 🔴 Fail | Requests full filesystem + network |
| Data Handling | 🔴 Fail | Env vars flow to an outbound POST |
| Instruction Integrity | 🟢 Pass | No hidden directives found |

**Tier: 🔴 Avoid.** A single undocumented credential read to a network sink is disqualifying regardless of how clean the rest looks. *This is the whole point of the index: one axis can sink a skill.*

---

## How to contribute

1. Read [`SCORING.md`](./SCORING.md) so ratings stay consistent.
2. Copy [`TEMPLATE.md`](./TEMPLATE.md), fill it in, open a PR.
3. Cite a primary source for every claim. Unsourced ratings are rejected.
4. We list **tools/standards** freely; we are deliberately conservative about publishing **Fail** ratings on named third-party skills without reproducible evidence.

---

## License

Content: CC BY 4.0. Do what you like, keep attribution.

---

*Built by Rynaut. If your agent trusts a skill, you trust it too — so make it earn that trust at the gate.*
