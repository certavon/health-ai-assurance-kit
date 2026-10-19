# Crosswalk

`crosswalk.csv` maps each of CHAI's 40 baseline operational controls to the instruments a US health system is answering to. One row per control, numbered as CHAI numbers them (1.1–1.3, 2.1–2.4, 3.1–3.10, 4.1.1–4.5.6).

Columns:

| Column | Content |
| --- | --- |
| `chai_control_id`, `chai_domain`, `chai_control` | CHAI's ID and domain, with the control paraphrased by the author (S1; CHAI's own text is CC BY-NC-ND and not reproduced) |
| `ruaih_guidance_2025` | Element(s) of the seven-element Sept 2025 guidance (S2) |
| `ruaih_certification` | Empty until the June 2026 certification standards are obtained (S3) |
| `urac_health_ai` | URAC's seven public evaluation areas (S4) |
| `nist_ai_rmf` | Subcategory IDs (S5) |
| `iso_42001` | Clause and Annex A control references (S6) |
| `texas_sb1188_traiga` | Statutory obligations under SB 1188 (Health & Safety Code ch. 183) and HB 149 (S7, S8) |
| `hipaa` | Current Security/Privacy Rule citations and proposed NPRM items, labeled (S9) |
| `evidence_artifact` | The author's proposed proof for the row |

Source IDs are defined in `../SOURCES.md`. `verification.csv` has one line per cell: `status` is `primary`, `secondary`, `unverified`, or `author`, with source, date and note. Where a cell names an instrument's element and the status is `primary`, that means the element exists as named; the *assignment* of that element to this control is the author's judgment and is open to challenge by pull request.

## Open items before v0.2

- [x] Expand from 8 domain rows to 40 control rows (CHAI baseline controls, Oct 2026)
- [ ] Obtain the Joint Commission certification standards; fill `ruaih_certification`
- [ ] Check the NIST column against AI RMF 1.0 subcategory text
- [ ] Purchase ISO/IEC 42001 and check the ISO column
- [ ] Read the HIPAA NPRM text; move NPRM cells to primary
- [ ] Add CHAI's primary/secondary/tertiary control tier once the per-domain playbooks are read
- [ ] Confirm Texas Medical Board standards referenced in §183.005(a)(3) exist in final form
- [ ] Decide on a separate FDA PCCP/TPLC overlay for manufacturers
