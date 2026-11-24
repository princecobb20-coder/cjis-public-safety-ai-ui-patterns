# Cognitive-Load Map

**Author / maintainer: Prince Cobb.** Why the dashboard is shaped the way it is: each zone is budgeted for a kind of attention, and the tokens enforce the budget.

## The attention gradient

The layout follows the F-pattern scan of a returning analyst, spending visual weight in strict priority order:

| Zone (scan order) | Question it answers | Load type | Token enforcement |
|---|---|---|---|
| 1 · Page header | "Where am I? What's my one action?" | ~0 — recognition | `page-title` + the view's **only** `brand-primary` button |
| 2 · KPI band | "Is anything on fire?" | glance | `stat-value` numerals; deltas in `caption` + state color |
| 3 · Latest casework | "Where was I?" | resumption | hero card, verb-first actions ("Open", "Go to last chat") |
| 4 · Case search | "Take me somewhere specific" | directed recall | `surface-inset` input; rows = title + `mono-meta` + badge |
| 5 · Notifications | "What happened while I was away?" | triage queue | table rhythm; `state-info` "New" badges carry the signal |

Three load-reduction mechanisms do most of the work:

- **One primary action per view** (Hick's law): `brand-primary` appears exactly once. Everything else is secondary/ghost, so choice cost stays flat as features grow.
- **Progressive disclosure**: KPI cards show number → qualifier → breakdown in descending type sizes; the detail is there but costs nothing to skip.
- **Signal = color + lightness + label**: no state is ever hue-only, so the alert layer survives color-vision deficiency and peripheral glancing.

## State-driven alert badges

The badge layer is a tiny state machine; every state is a fill/text/border triple from the tokens, never a tinted improvisation:

```
            notification arrives
                    │
   ┌─────────── NEW (state-info tint · highest salience) ─ pulls the eye
   │ user views         │
   ▼                    ▼
  READ (neutral surface-2 · recedes)            case states:
                                     OPEN (badge-open-* · AAA green)
                                     ON HOLD (state-warning tint)
                                     CLOSED (neutral · lowest salience)
```

Salience is rationed: **unread + open are the only loud states.** Read and Closed are deliberately quiet so the queue's visual weight tracks the real work remaining — the screen itself is the to-do list.

## Dark-mode load rationale

Elevation-by-lightness (see the surface ladder in `tokens/investigative-ai.tokens.json`) keeps the z-order readable without shadows, which smear on dark grounds. AAA-level text ratios everywhere (9:1–17:1) are not gold-plating: analysts read this screen for hours, and the contrast headroom is what lets `text-muted` and quiet badges *recede* while still passing AA/AAA. The flagged 2.0:1 `border-default` is consistent with this — hairlines organize, they never inform.

## Budget rules for new screens

1. One `brand-primary` action per view; add a second and one of them is wrong.
2. New status = new StatusBadge variant from the state tokens, with a text label — never a bare dot.
3. Nothing animates except state transitions; motion is a signal, not decoration.
4. Every new zone must name which row of the attention-gradient table it joins. If it doesn't fit a row, it's a new screen, not a new zone.
