# Changelog

## Unreleased
- Crosswalk expanded from 8 domain rows to 40 control rows using CHAI's baseline operational controls (governance.chai.org/introduction, read 2026-10-19); `sources/chai-baseline-controls.csv` added with paraphrased controls and attribution
- `hipaa_nprm` column renamed `hipaa`; cells now cite the current rule where it applies and label NPRM items as proposed
- README: CC BY-NC-ND note on CHAI material
- Corrected: removed the claim that SB 1188 prohibits training AI on patient data; §183.005 imposes only license scope, no other legal bar, TMB-standard review of AI-created records, and patient disclosure
- Corrected: URAC column now uses URAC's seven published evaluation areas instead of invented names; full standards noted as paid
- Corrected: CHAI numbering aligned to CHAI's structure (domains 1–3, subdomains 4.1–4.5) instead of a flat eight
- Split the RUAIH column into `ruaih_guidance_2025` (verified against the seven-element Sept 2025 guidance) and `ruaih_certification` (empty until the certification standards are obtained)
- Replaced the row-level `verified` flag with `crosswalk/verification.csv` (one line per cell: status, source, date, note) and added `SOURCES.md`
- Added `docs/sources-to-acquire.md`
- README rewritten to state what the kit can and cannot do at element level

## v0.0.1 — 2026-10-10
- Repo scaffold: README, MIT license, CITATION
- `crosswalk/crosswalk.csv`: eight CHAI elements mapped first-pass to RUAIH, URAC, NIST AI RMF, ISO/IEC 42001, Texas SB 1188/TRAIGA, HIPAA NPRM. All rows `verified = no`.
- Placeholders for readiness-assessment, vendor-assessment, policy-templates, evidence-map
