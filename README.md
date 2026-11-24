# CJIS Public-Safety AI — UI Patterns

**Human-centered interface patterns for CJIS v6.1-compliant public-safety AI systems**

Author: **Prince Cobb** — UI/UX architecture, compliance-constrained interaction design for investigative AI.

## Executive summary

AI systems built for U.S. public-safety and criminal-justice agencies face a *last-mile* bottleneck: the models may work, but adoption stalls at the interface, where the FBI CJIS Security Policy's access-control, session, authentication, audit and integrity requirements must become concrete, usable screen states. Agencies cannot deploy what their analysts cannot operate within policy, and vendors re-solve the same compliance-UI problems one procurement at a time.

This repository documents a reusable interface architecture that treats each applicable FBI CJIS Security Policy v6.1 control family (NIST SP 800-53 rev. 5-aligned) as a **designed UI condition** — so that compliance is enforced in the interaction layer itself, and the same patterns transfer across agencies and vendors without redesigning the front end each time. The patterns were developed for a deployed investigative-AI platform, and are published here in sanitized form so other teams can adopt them directly.

## How interface states enforce CJIS v6.1 control families

| Control family | Policy requirement (public) | Front-end UI state |
|---|---|---|
| **AC-3, AC-6** — Access Control | Enforce approved authorizations; least privilege for access to criminal justice information and functions. | Role-based visibility flags: nav items, case actions and export controls render only for authorized roles; unauthorized access resolves to an explicit access-denied state, never a silent failure. |
| **AC-11, AC-12** — Session Lock & Termination | Lock the session after inactivity; terminate at defined conditions or user request. | Session-control state sequence: idle-warning modal with visible countdown → obscured screen on lock → forced re-authentication to resume → hard sign-out state on termination. |
| **IA-2** — Identification & Authentication | Uniquely identify and authenticate users; MFA for CJI access. | Sign-in flow with MFA step state; re-authentication modal gates privileged views after lock or elevation. Credential fields never prefill. |
| **AU-2, AU-14** — Audit & Accountability | Log auditable events; provide session audit capability and review tools. | Audit-log surfacing: notifications table with immutable mono timestamps and read-state badges; per-case activity trail exposed in the UI rather than buried in backend logs. |
| **MP** — Media Protection | Protect and control CJI media; sanitize before release or reuse. | Redaction workflow states: a redaction-finished notification places the redacted copy in a review state for checking before any release; the original stays access-controlled throughout. |
| **SI-4** — System & Information Integrity | Flag and verify system-generated information before reliance on it. | AI verification disclaimers: every model-generated insight is labeled as AI output and carries a verify-before-action disclaimer state; unverified insights cannot be cited into reports. |

The full matrix, including the AC-11/AC-12 session-control state sequence, is in the compiled PDF below.

## Repository contents

| Path | What it is |
|---|---|
| [`docs/Human-Centered_UI_Patterns_for_CJIS_v6.1-Compliant_Public-Safety_AI_Systems.pdf`](docs/Human-Centered_UI_Patterns_for_CJIS_v6.1-Compliant_Public-Safety_AI_Systems.pdf) | The compiled pattern set: the CJIS v6.1 UI Compliance & Session-Control Matrix, the Design-System & Cognitive-Load Architecture Map, and the Triage & Evidence-Review State Flow. |
| [`tokens/investigative-ai.tokens.json`](tokens/investigative-ai.tokens.json) | The design tokens behind the patterns — the token budget the architecture map draws as "27 color roles, 8 type styles, 6 spacing, 3 radii": the dark-mode surface ladder, text, brand, state, badge, border and chart color roles (28 named color tokens, including the derived third chart hue), 8 type styles across Display/Headings/Text/Mono, the 6-step 4px spacing scale and 3 corner radii, with per-token usage notes and audited WCAG 2.1 contrast ratios. |
| [`architecture/component-layer-tree.yaml`](architecture/component-layer-tree.yaml) | The component layer tree of the dashboard screen plus the full prototype hotspot map — the structure a design or engineering team can rebuild directly. |
| [`architecture/architecture-map.md`](architecture/architecture-map.md) | How the library is organized: tokens → variables, atoms → molecules → organisms → template, and the modular 12-column layout rules. |
| [`architecture/cognitive-load-map.md`](architecture/cognitive-load-map.md) | The attention-gradient rationale: how each dashboard zone is budgeted for a kind of attention and how the tokens enforce that budget. |

## Adopting these patterns

- **Design teams** can rebuild the structure in Figma from `component-layer-tree.yaml` and `architecture-map.md`: the tokens map 1:1 to a `Core/Color` variable collection (single `dark` mode) and a `Core/Dimension` collection.
- **Engineering teams** can consume `tokens/investigative-ai.tokens.json` directly as CSS custom properties or through a token pipeline; every color token carries its computed WCAG 2.1 contrast ratio and usage rule.
- **Compliance reviewers** can use the control-family matrix as a checklist for whether an interface makes each CJIS v6.1 requirement a designed, testable UI state rather than a policy PDF promise.

## Sanitization

All documentation in this repository uses placeholder data only. No criminal-justice information, internal endpoints, hostnames, timeout values or model details appear; redacted items are explicitly marked. Policy language is paraphrased from the public FBI CJIS Security Policy v6.1; consult the published policy for authoritative text. Backend model behavior is out of scope — these patterns document the interaction layer only.

## License

Released under the [MIT License](LICENSE) so that other agencies, vendors and design teams are free to adopt, adapt and redistribute these patterns, with attribution preserved in the license notice.

## Author

**Prince Cobb** — designer and maintainer of the investigative-ai interface library and of the compliance-interaction patterns documented here.
