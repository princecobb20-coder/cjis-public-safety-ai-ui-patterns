# Architecture Map

**Author / maintainer: Prince Cobb.** The structural map of the investigative-ai interface library — how the Figma library is organized, how tokens become variables, and how components compose into the modular layout.

## Figma library structure

One library file, five pages, published as a team library:

```
investigative-ai — Design System          (library file · owner: Prince Cobb)
│
├── 🧭 Cover                        thumbnail + changelog + status
├── 📐 Foundations
│     ├── Color — variables bound to the dark collection (below)
│     ├── Type — 8 text styles (Display / Headings / Text / Mono)
│     ├── Spacing — 4px grid, 6 steps (space-1 … space-8)
│     ├── Radius — radius-md 12 · radius-section 14 · radius-full
│     └── Elevation — the 6-rung surface ladder, documented as frames
├── 🧩 Components                   (atoms → molecules, variants wired)
├── 📄 Patterns                     (organisms: composed, slot-based)
└── 🗺 Templates                    (the dashboard grid + breakpoints)
```

### Variables: one collection, one mode

`color` tokens map 1:1 to a **Core/Color** variable collection with a single `dark` mode (the product is dark-only; adding a light theme later = adding a mode, not renaming variables). Naming mirrors `tokens/investigative-ai.tokens.json` with `/` for grouping: `surface/canvas`, `surface/1`, `text/primary`, `brand/primary`, `state/success`, `badge/open/bg`, `border/default`, `chart/1`. Spacing and radius go in a **Core/Dimension** collection. Never hard-code a hex in a component — every fill, stroke and text color binds to a variable.

## Component library — structural map

Composition is strictly upward; an element may only consume layers below it.

```
TOKENS ──────────────────────────────────────────────────────────────
   │
ATOMS          Button (primary / secondary / ghost · sm / md)
               StatusBadge (open / hold / closed / new / read / info)
               Input (search), Icon, Avatar, Link, Divider
   │
MOLECULES      StatCard        = section-label + stat-value + caption
               NotificationRow = StatusBadge + title + body-sm + mono-meta
               CaseListItem    = title + mono-meta + StatusBadge + Button
               NavItem         = Icon + label (+ nav-selected fill)
   │
ORGANISMS      KPIBand         = 4 × StatCard
               CaseSearchPanel = Input + CaseListItem list
               NotificationsTable = header + NotificationRow list
               LatestCaseworkCard = hero case + action Button group
               Sidebar         = NavItem list on surface-0
   │
TEMPLATE       Dashboard = Sidebar + [PageHeader / KPIBand /
                           (LatestCasework ‖ CaseSearch) / Notifications]
```

Variant properties (Figma): Button → `variant`, `size`, `state`; StatusBadge → `status` (single property drives fill/text/border together — states can never be mixed); StatCard → `trend` (up/down/none); NotificationRow → `read` (boolean).

## Modular layout architecture

The dashboard is a 12-column grid on an 8px macro-rhythm (zones separated by `space-8`, cards by `space-6`):

```
┌────────┬───────────────────────────────────────────────┐
│        │  PageHeader (page-title + primary Button)      │
│        ├───────────┬───────────┬───────────┬───────────┤
│ Sidebar│  StatCard │  StatCard │  StatCard │  StatCard │  3 cols each
│ surface├───────────┴───────────┼───────────┴───────────┤
│   -0   │  Latest Casework (6)  │  Case Search (6)      │
│        ├───────────────────────┴───────────────────────┤
│        │  Notifications table (12)                      │
└────────┴───────────────────────────────────────────────┘
```

Rules: the sidebar is the only `surface-0` region; every zone is a `surface-1` card at `radius-section`; zones reflow 12 → 6 → stacked below 1280/768px, in reading order (header → KPIs → casework → notifications) so the priority order survives every breakpoint.
