# Changelog — AIWIZN CNA Onboarding Engine

All notable changes to the engine, most recent first. This file also serves as
the hand-off record for the planned Next.js integration into the AIWIZN app.

---

## v1.4 — Renamed to Patient Care Onboarding Engine

- Product renamed **CNA Onboarding Engine -> Patient Care Onboarding Engine**
  to reflect the true audience (CNAs, PCTs, PCAs), not CNAs alone.
- Metric renamed **CRI / CNA Readiness Index -> PCRI / Patient Care Readiness
  Index**. Internal function `computeCRI` -> `computePCRI`.
- Dashboard, modal, READINESS terminal, and SVG labels updated to neutral
  "patient care staff" / "staff" wording.
- Scenario teaching text that refers to a CNA's specific scope of practice is
  intentionally left as "CNA" — that is a clinical accuracy point, not a label.
- **Supabase**: table `cna_onboarding_records` dropped and recreated as
  `pco_onboarding_records` (was empty; zero data loss). Added `staff_role`
  column (CNA/PCT/PCA); `cri` column renamed `pcri`. RLS and policies recreated.

## v1.3 — Reviewer feedback & positioning

Clinical / content revisions from hospital-HR reviewer feedback:

- **Audience terminology** made consistent — "patient care staff" used as the
  umbrella term (CNAs, PCTs, PCAs); role-specific items are now explicitly
  labelled rather than defaulting to "CNA".
- **Element 1 (Credentialing)** restructured into two labelled lanes —
  Stage A (HR-owned screening) and Stage B (Occupational Health-owned
  clearance) — matching real hospital hiring flow. Nurse aide certification
  tagged CNA-SPECIFIC; references split into their own checkpoint (now 9 items).
- **Element 2 (Orientation)** — added a note that mission/conduct/attendance/HR
  items may be satisfied via Employee Handbook attestation.
- **Element 3** retitled "Safety, Compliance & Regulatory Training"; split into
  a Safety group and a Compliance/Regulatory group (HIPAA and abuse/neglect
  reporting moved to Compliance). Added a "Safe lifting & workplace-injury
  prevention" checkpoint covering de-escalation (now 7 items). HIPAA item now
  references applicable state privacy statutes.
- **Elements 4 & 5** — added "Facility-Specific" notes stating the engine
  validates against each hospital's own policies and procedures.
- **Element 6** — "Dementia/Geriatrics" unit renamed "Geriatrics" and reframed
  to avoid implying most older adults are cognitively impaired; intro now
  acknowledges PCTs/PCAs and that aide responsibilities differ by unit.
- "Fit-for-duty" evaluation marked "where required by facility policy".
- Checkpoint totals corrected: now 37 checkpoints, 12 decision nodes
  (the previous "11 decision nodes" label was inaccurate).
- **Overview** — added a first-90-day retention framing callout positioning
  the engine within an onboarding -> upskilling -> retention continuum.

## v1.2 — Code review & clinical accuracy fixes

- **E7-01 scenario** — replaced the blood-glucose finger-stick (a state/
  facility-dependent delegated task) with a mechanical (Hoyer) lift transfer,
  which is unambiguously within CNA scope everywhere.
- **Readiness staging** — removed the RN-specific Benner labels
  (Novice / Advanced Beginner / Competent); replaced with CNA-native
  Stage 1 Foundational -> Stage 4 Independent. Function renamed `readinessStage`.
- **localStorage save** — `saveDB()` now returns success/failure; the save
  toast reflects the real result instead of always showing success.
- **Modal** — pressing Enter in the name/unit fields now starts the session.

## v1.0 — Initial build

- Single-file HTML application — AIWIZN CNA Onboarding Engine.
- Eight-element onboarding pathway with immersive SVG environments.
- 12 applied decision nodes with expert/developing/gap scoring.
- READINESS profile with the CNA Readiness Index (CRI) and a 7-domain
  weighting model.
- LUMINA dashboard with cohort analytics (currently localStorage-backed).

---

## Planned — Next.js integration (not yet in this repo)

The engine is to be integrated into the AIWIZN app (`aiwizn-clinical-engine`
repo) as a dashboard page, replacing localStorage with Supabase:

- New Supabase table `public.cna_onboarding_records` (RLS, owner-scoped) is
  already created in the `aiwizn` Supabase project.
- `saveSession` / `loadDB` / `clearData` to be re-pointed at Supabase;
  the in-app scoring engine stays as-is.
- This standalone repo remains the source-of-truth demo until that work lands.
