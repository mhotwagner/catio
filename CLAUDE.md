# CLAUDE.md — Catio V2 Build

## What this project is
Interactive 3D HTML models (three.js r128, single-file, units = **inches**) of a real catio in the Hudson Valley, NY, reconstructed from tape measurements. The model is the design document: what gets modeled here gets built in lumber. Four-season climate assumptions apply (snow load on horizontals, freeze/thaw, humid summers, winter wind).

- `index.html` — **V1 as built**, measured & verified. Reference only; do not modify.
- `v2.html` — **V2 expansion draft** (currently r5). This is where the work happens.

**V2 envelope (draft r5):** 88" E–W × 78" N–S, 80" panel height. Roof unresolved — leading candidate is a flat wire ceiling at 80" (rendered amber/"TBD" in the model). North wall is the house side: the window cat-flap exits directly onto the sectional back (~29" high, running the full N wall — effectively a pre-built low catwalk; routes should key off it). Chaise on the W wall. The **apparatus wall is the east face + SE corner**. Door position is an open question (W corner vs middle — toggle in the model).

## Model conventions
- Axes: **+X = east, +Z = south, +Y = up.** Units are inches, real tape-measure values.
- Envelope constants live at the top of `v2.html` (`D1, XE, L, H, TE, TS, RAILS`). Key off them; never hardcode a duplicate of an envelope dimension.
- Panel rails (`RAILS = 0.75 / 24.75 / 55.25 / 79.25`) are the natural mounting datums — V1's platforms mount on rail dividers with diagonal knee braces.
- Model real lumber actuals, not nominal: 2x2 = 1.5×1.5", 1x12 = 11.25×0.75", 2x10 = 9.25×1.5", 4x4 = 3.5×3.5", 5/4x6 = 5.5×1.0".
- Revision convention: superseded drafts are snapshotted to `revisions/catio-v2-rN-<slug>.html`; the live head is `v2.html` with its rN in the HUD title.

## The clients
- **Hobie** (tagged "Hoby" in the model — same cat) — ~20 lb, 6 yrs. Governs load ratings, shelf depth, and door-hole sizes. Treat him as a structural consideration.
- **Zoey** — ~11 lb, 14.5 yrs. Fast and vital today; design for her at 16. Governs route design: every destination reachable by at least one low-effort path. Note the flap→sofa-back entry is already senior-friendly; keep it that way.

## Hard constraints — audit every change against these
| Spec | Value |
|---|---|
| Shelf/perch depth | ≥ 10"; 11.25" (1x12 actual) ideal |
| Perch length / landings | 16–24" / ≥ 12x18" |
| Vertical hop spacing | 12–16" standard; 9–12" on the senior route |
| Horizontal gap between platforms | 12–16" easy; **24" hard max** |
| Catwalk width | 10–12"; ≥ 14" at passing bulges |
| Headroom above top runs | ≥ 14" to roof/mesh |
| Ramp slope | ≤ 20° senior route (~3:1 run:rise); ≤ 30° elsewhere; cleats every 4" |
| Cubby door hole | 6.5–7" diameter |
| Design load, every perch | ≥ 50 lb static |
| Route redundancy | Every high point: ≥ 2 ways on/off, one low-effort |
| Senior route | Continuous floor → highest perch, no required jumps |

Known consequence in this envelope: the **30.5" gap between the mid rails (24.75" → 55.25") exceeds hop spec** — any climbing bay using rail-mounted shelves needs one intermediate step (cleat-mounted between rails, or offset into the adjacent bay). V1's "no walkway — they leap it" platform pair is exactly the pattern these constraints replace.

Two route-design principles: **the higher the platform, the easier the moves to reach it must be**, and **no single-exit perches up high** (they become traps in a two-cat household).

## Session protocol
1. At session start: read `BUILD-LOG.md` (top 2–3 entries) and the relevant section(s) of `docs/catio-v2-apparatus-catalog.md` for whatever is in scope. Don't load the whole catalog for a one-apparatus task.
2. **One apparatus or one focused task per session.** Don't sprawl.
3. Make the change in the model.
4. Run the acceptance audit (below) on everything touched.
5. Append a `BUILD-LOG.md` entry (format is in that file): done / decisions + why / open questions / next.
6. If a draft milestone landed: snapshot the previous head to `revisions/catio-v2-rN-<slug>.html`, bump the rN in the HUD title, and update README's "plan so far" if the plan changed.
7. Commit with a descriptive message. Land the plane before context runs out — a clean stop with a good log entry beats a heroic unfinished push.

## Acceptance audit — run after every change
- [ ] All platform gaps ≤ 24" (≤ 16" preferred)
- [ ] Vertical hops in-spec for their route (12–16" std / 9–12" senior)
- [ ] Top-run headroom ≥ 14" (if the 80" wire ceiling stands, the 55.25" rail datum gives ~24" — use it)
- [ ] Every platform has ≥ 2 exits
- [ ] Senior route still continuous, floor → highest perch
- [ ] Nothing intrudes on the human door swing (either door position) or head-height path
- [ ] Apparatus mounts to panel frames/rails or stands on the floor — never to wire mesh; knock-down property preserved (no permanent cross-panel structure)
- [ ] Envelope constants unchanged unless the physical plan actually changed
- [ ] `index.html` (V1 as-built) untouched
- [ ] Model renders clean (no console errors); nothing z-fighting or floating

## Architecture (data layer landed in r6)
Envelope constants and geometry helpers (`box, addBox, newPanel, peg, diagXY…`) are clean; panel-building code stays as it is. The interior is data-driven: the `APPARATUS` config array — entries `{id, type, name, pos, dims, phase, built}`, where `pos` is the west/floor/north min corner in world inches and `dims` is `{w: E–W, d: N–S, h: height}` — is consumed by per-type builder functions (`buildSectional`, `buildChaise`, …) via the `BUILDERS` dispatch map. Adding apparatus means appending a config entry, plus a builder if the type is new; the array doubles as the manifest of planned (`built: false`) vs. built. `phase` 0 = furniture/layout, 1–3 = catalog build order. Key config values off the envelope constants — never hardcode a duplicate of an envelope dimension. Scenery (stowed V1 interior, residents, compass, dimension labels) stays inline in the "V2 INTERIOR — scenery" section and is not apparatus.

## Repo map
- `v2.html` — active V2 draft
- `index.html` — V1 as measured/verified; reference only
- `revisions/` — every prior draft (V1 rA–rH, V2 r1+)
- `docs/catio-v2-apparatus-catalog.md` — apparatus source of truth: specs, build notes, materials, placement. Cite sections rather than inventing specs.
- `BUILD-LOG.md` — session journal, newest first
- `README.md` — human-facing summary; keep "The plan so far" current at milestones

## Domain notes that override cleverness
- **Knock-down is sacred:** the whole structure is half-lapped, pegged, doweled panels that come apart flat (the Explode toggle is not a joke). Apparatus must be panel-mounted, bolted, or freestanding — never permanent cross-panel structure.
- **Load path:** frames are 2x2 pine. Rail-mounted shelves with knee braces are proven (V1 precedent), but heavy or dynamic items — skywalk runs, the branch tree — should carry load to the floor or spread across multiple rails. Don't cantilever heavy things off a single 2x2.
- **Roof is TBD:** anything ceiling-dependent (branch-tree top anchor, hanging items, final headroom numbers) is blocked on the roof call. Design floor- or rail-anchored until it lands; flag the dependency in the log rather than assuming the 80" wire ceiling.
- Orientation is known (compass axes above): S/SE = winter sun (sun-deck candidates), N = house/sofa, apparatus wall = E + SE.
- Cedar for cat-contact surfaces; pressure-treated only for structure/ground contact.
- No food fixtures anywhere in the design — this is bear and raccoon country.
- When apparatus specifics are ambiguous, the catalog wins over improvisation. If the catalog is silent, log the question instead of guessing.
