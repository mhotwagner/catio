# Catio V2 — Build Log

Session journal. **Newest entries at the top.** One entry per working session, appended at session end per the protocol in `CLAUDE.md`. Read the top 2–3 entries at session start before touching anything.

## Entry format
```
### YYYY-MM-DD — Short title
**Done:** what changed in the model/repo this session.
**Decisions:** choices made and the reasoning (this is the part future sessions actually need).
**Open questions:** anything unresolved, ambiguous, or awaiting a human answer.
**Next:** the single best next task, specific enough to be a session prompt.
```

---

### 2026-07-16 — r6: APPARATUS data layer (behavior-preserving refactor)
**Done:** Interior is now data-driven per the architecture target: `APPARATUS` config array (`{id, type, name, pos, dims, phase, built}`) + per-type builders (`buildSectional`, `buildChaise`) + a `BUILDERS` dispatch loop. First entries: `sectional-north` and `chaise-west` (phase 0, `built: false`), migrated out of the inline "V2 INTERIOR" block; their three materials moved into the shared `M` dict (`secFrame`/`secCushion`/`secBack`). Scenery (stowed V1 interior, cats, compass, dimension labels) stays inline. Housekeeping: catalog moved to `docs/catio-v2-apparatus-catalog.md` (repo map said `docs/`, file sat at root); snapshot `revisions/catio-v2-r5-full-north-lounge.html`; HUD → DRAFT R6; CLAUDE.md architecture section rewritten to the landed state; README revision list → r1–r5. Verification (headless Chrome/SwiftShader, pre vs post): scene-graph dumps — 274 objects, world transforms + geometry params + colors + shadow flags — byte-identical at rest *and* mid-animation with the door open; 15 toggle/view screenshot states all identical except the intended R5→R6 HUD digit, with two explained deltas: flap = capture jitter (its sine wobble never settles; same-file reruns jitter equally) and door-open = a 38×193 px draw-order sliver, bisect-proven to come solely from material creation order (three.js sorts opaque draws by material id) flipping the depth-tie winner where the open leaf interpenetrates the south house-wall plane — an artifact already present in r5. `node --check` clean; zero page console output.
**Decisions:** (1) Two manifest entries, not one L-shaped blob — the W-wall back (z 2–56) belongs to `chaise-west` and shares the NW corner behind the sectional's corner seat; the split is bookkeeping, geometry unchanged. (2) `pos` = west/floor/north min corner, `dims` = `{w, d, h}` bounding box, world inches — matches tape-measure thinking. (3) Envelope-derived config only where the envelope is the constraint: sectional `w: XE - 4` (full N wall less 2" clearance per end), chaise `d: L - 24` so the foot keeps the documented 22" gap to the S-wall door. (4) `phase: 0` = furniture/layout; catalog phases 1–3 reserved for apparatus. (5) `built: false` renders solid for now — ghosting planned-vs-built is a future display decision; zero-visual-change ruled it out this session.
**Open questions:** Carried: roof call; door position W corner vs middle. New (cosmetic, pre-existing in r5): the W-corner door leaf opens −1.9 rad (≈109°), swinging through the house's south-wall plane at full open — cap the swing near −1.5 rad or add a doorstop in a future session if it grates.
**Next:** The apparatus wall (E face + SE corner) as APPARATUS entries: catalog §1.1 cedar shelf runs + §1.5 senior switchback ramp. Constraint to design around: the 30.5" gap between the mid rails (24.75 → 55.25) exceeds hop spec — every climbing bay needs one intermediate step (cleat-mounted between rails, or offset into the adjacent bay).

### 2026-07-16 — Docs added; repo reviewed
**Done:** Added `CLAUDE.md`, `docs/catio-v2-apparatus-catalog.md`, and this log. Reviewed `v2.html` (draft r5) and README; CLAUDE.md's repo map, model conventions, and envelope facts were filled in from source. Layout/structure decisions before this log live in commit history and `revisions/`.

**Decisions:** Session protocol per `CLAUDE.md` (one task / audit / log / snapshot / commit). Apparatus specs defer to the catalog. The `APPARATUS` data-layer refactor is required **before** apparatus-wall work begins — the interior is currently inline `addBox` calls (see ~line 557).

**Open questions:** (1) Roof call — flat wire ceiling at 80" is the leading candidate; blocks ceiling-dependent apparatus (branch-tree top anchor, hanging items). (2) Door position, W corner vs middle — affects SE-corner apparatus clearances; the chaise foot already constrains a 22" gap to the door. (3) Confirm the skywalk rides the 55.25" rail datum (~24" headroom under an 80" ceiling) once the roof lands.

**Next:** `APPARATUS` refactor session — introduce the config array + per-type builder functions per CLAUDE.md's Architecture target; migrate the sectional/chaise into it as the first entries; run the audit; snapshot as r6 if it lands clean. After that: the apparatus wall (catalog §1.1 shelf runs + §1.5 senior ramp), remembering the 30.5" mid-rail gap needs one intermediate step per climbing bay.
