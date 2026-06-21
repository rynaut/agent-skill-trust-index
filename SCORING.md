# Scoring Methodology

This document defines how each axis is rated **Pass / Caution / Fail**, so that any two reviewers reach roughly the same tier for the same skill. The rubric is the point of this project — the index is just its application.

> ⚠️ Ratings are summaries based on inspection, not formal audits. They can be wrong or stale. Always verify against the skill's actual source.

---

## How to score

1. Read the skill's documentation (the `SKILL.md` / instruction file) **and** every bundled script.
2. Rate all nine axes.
3. Apply the tier rules: any **Fail on axis 1 (Provenance), 5 (Data Handling), or 6 (Instruction Integrity)** forces **🔴 Avoid**. Otherwise the lowest rating caps the tier.
4. Cite evidence for each non-Pass rating (file + line, or the exact instruction text).

---

## Axis definitions

### 1. Provenance & Identity
- **Pass** — Verifiable author; signed package or attested source; identity survives scrutiny.
- **Caution** — Real but unverified author; unsigned; single maintainer.
- **Fail** — Anonymous/typosquatted name; impersonates a known project; no traceable origin.

### 2. Declared vs Actual
- **Pass** — Code does what the docs say, no more.
- **Caution** — Minor over-declaration (asks for capability it doesn't use).
- **Fail** — Under-declaration: code has a capability the docs hide ("shadow feature").

### 3. Least Privilege
- **Pass** — Access scoped tightly to stated purpose.
- **Caution** — Broader than needed but plausibly justified.
- **Fail** — Sweeping filesystem/network/shell access with no functional reason.

### 4. Code Safety
- **Pass** — Instruction-only, or scripts with no dangerous constructs.
- **Caution** — Bundled scripts using subprocess/dynamic calls with clear, benign intent.
- **Fail** — `exec`/`eval` on dynamic input, obfuscated payloads, `curl \| bash`, remote code fetch.

### 5. Data Handling  *(disqualifying axis)*
- **Pass** — No access to secrets, or access with no network egress.
- **Caution** — Reads config that may contain secrets, but no exfiltration path.
- **Fail** — Any traceable flow from credentials/env vars/key files to a network sink.

### 6. Instruction Integrity  *(disqualifying axis)*
- **Pass** — No hidden or adversarial instructions anywhere.
- **Caution** — Aggressive but visible prompt language; broad triggers.
- **Fail** — Hidden directives (HTML comments, zero-width chars, metadata injection), instruction-override, or "don't tell the user" patterns.

### 7. Dependency Hygiene
- **Pass** — Pinned, maintained, no known CVEs.
- **Caution** — Unpinned or aging dependencies; no current CVEs.
- **Fail** — Known-vulnerable dependency, or abandoned package in a sensitive role.

### 8. Tamper Resistance
- **Pass** — Integrity bound to content hash; tampering after approval is detectable.
- **Caution** — Standard registry, no post-approval verification.
- **Fail** — Mutable config trusted by file identity (classic time-of-check/time-of-use gap).

### 9. Compliance Fitness
- **Pass** — Documented, auditable, change-tracked; defensible in a regulated context.
- **Caution** — Usable but thin documentation; limited audit trail.
- **Fail** — Opaque behavior that cannot be reviewed or evidenced.

---

## Reviewer discipline

- **Default to Caution, not Pass,** when you can't fully verify an axis. Absence of evidence isn't evidence of safety.
- **Never assign a Fail to a named third-party skill without reproducible evidence** (a file/line others can check). Reputational claims need proof.
- **Tools and standards are not scored** — they're the instruments, listed in the index for what they cover.
- Re-review on any version bump. A skill that passed last month is a different artifact today.
