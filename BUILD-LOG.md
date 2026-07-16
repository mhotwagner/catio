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

### 2026-07-16 — Docs added; repo reviewed
**Done:** Added `CLAUDE.md`, `docs/catio-v2-apparatus-catalog.md`, and this log. Reviewed `v2.html` (draft r5) and README; CLAUDE.md's repo map, model conventions, and envelope facts were filled in from source. Layout/structure decisions before this log live in commit history and `revisions/`.

**Decisions:** Session protocol per `CLAUDE.md` (one task / audit / log / snapshot / commit). Apparatus specs defer to the catalog. The `APPARATUS` data-layer refactor is required **before** apparatus-wall work begins — the interior is currently inline `addBox` calls (see ~line 557).

**Open questions:** (1) Roof call — flat wire ceiling at 80" is the leading candidate; blocks ceiling-dependent apparatus (branch-tree top anchor, hanging items). (2) Door position, W corner vs middle — affects SE-corner apparatus clearances; the chaise foot already constrains a 22" gap to the door. (3) Confirm the skywalk rides the 55.25" rail datum (~24" headroom under an 80" ceiling) once the roof lands.

**Next:** `APPARATUS` refactor session — introduce the config array + per-type builder functions per CLAUDE.md's Architecture target; migrate the sectional/chaise into it as the first entries; run the audit; snapshot as r6 if it lands clean. After that: the apparatus wall (catalog §1.1 shelf runs + §1.5 senior ramp), remembering the 30.5" mid-rail gap needs one intermediate step per climbing bay.
