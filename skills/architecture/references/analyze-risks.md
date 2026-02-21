# analyze-risks
# Layer: 2 Executor
# Output: transient

## Purpose

Identify technical, dependency, and timeline risks based on the design system spec before committing to an architecture.

## Input

- Required: `.loom/design/system.md`
- Optional: `.loom/ideation/requirements.md` (for scope and priority context)

## Execution

Scan both documents and evaluate risks across three categories:

**1. Technical risks** — implementation challenges likely to cause rework:

| Risk | Severity | Why | Mitigation |
|------|----------|-----|------------|
| [description] | high / medium / low | [reason] | [approach] |

Common patterns to check for:
- Real-time features (websockets, live sync) with no prior architecture decision
- Offline-first requirements conflicting with server-side state
- Auth requirements (SSO, OAuth, MFA) not yet scoped
- Third-party integrations with unclear API limits or reliability

**2. Dependency risks** — external services or libraries that could block delivery:

| Dependency | Type | Risk | Alternative |
|------------|------|------|-------------|
| [name] | API / library / service | [risk description] | [fallback option] |

**3. Timeline risks** — features that are likely underestimated:

| Feature | Concern | Recommendation |
|---------|---------|----------------|
| [name] | [why it's risky] | [simplify / defer / spike first] |

## Checkpoint

If any `high` severity risk is found, pause and present it to the user:

```
High-severity risk identified before architecture design:

[Risk description]
Why: [reason]
Suggested mitigation: [approach]

Please confirm how you'd like to proceed before I continue.
```

## Output (transient)

```
risks:
  technical:
    - { description, severity, mitigation }
  dependency:
    - { name, type, risk, alternative }
  timeline:
    - { feature, concern, recommendation }
  has_blockers: true | false
  summary: "[N] risks found — [N] high severity"
```
