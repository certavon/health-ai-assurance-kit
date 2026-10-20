# health-ai-assurance-kit

An open toolkit for US healthcare organizations preparing for AI governance certification or accreditation with no dedicated AI risk staff.

It maps the Coalition for Health AI (CHAI) governance framework — four domains, with domain 4 split into five subdomains — to the instruments a health system is answering to: the Joint Commission / CHAI Responsible Use of AI in Healthcare (RUAIH) guidance, URAC Health Care AI Accreditation, NIST AI RMF, ISO/IEC 42001, Texas SB 1188 and HB 149 (TRAIGA), and the proposed HIPAA Security Rule. One control set, one evidence list.

## Who this is for

- Hospital and health-system CISOs, CMIOs, compliance and quality leaders preparing for RUAIH certification or URAC accreditation
- Health-AI vendors answering customer due-diligence questionnaires
- Anyone running an AI governance committee that has policies but no operating system

## What's here

| Folder | What it is | Status |
| --- | --- | --- |
| `crosswalk/` | CHAI's 40 baseline controls × instruments, with a per-cell verification record | control level (40 rows); NIST and ISO columns unverified |
| `sources/` | CHAI control IDs and Joint Commission RUAIH standard/EP IDs, each with the author's paraphrase and source | current |
| `SOURCES.md` | Every instrument cited, with access type and the date it was last checked | current |
| `docs/sources-to-acquire.md` | What still has to be obtained, and what each one unblocks | current |
| `readiness-assessment/` | RUAIH readiness questionnaire and scorer | planned |
| `vendor-assessment/` | Health-AI vendor due-diligence template | planned |
| `policy-templates/` | AI policy, intake form, clinician disclosure script, human-review attestation | planned |
| `evidence-map/` | Which artifact proves each control, owner, cadence | planned |

## What it can do today

The crosswalk is at control level: 40 rows, one per CHAI baseline operational control, each mapped to the RUAIH guidance elements, the RUAIH certification standards and elements of performance, URAC's evaluation areas, NIST AI RMF subcategories, ISO/IEC 42001 clauses, Texas statute, HIPAA, and a proposed evidence artifact. That is enough to build an evidence list and find the gaps in a program. It is not yet enough to claim conformance to anything: the NIST and ISO columns are unverified, and the certification manual mapped is the final draft dated May 29, 2026.

To use it now: open `crosswalk/crosswalk.csv`, drop the columns for instruments that don't apply to you, and for each of the 40 rows mark whether the evidence artifact exists, exists but is stale, or doesn't exist. Roll up by CHAI domain; the domain with the most gaps is the first remediation sprint.

## What it is not yet

- Not tiered. CHAI ranks its controls primary/secondary/tertiary by consensus; that tier is not yet in the file.
- Not a complete map in either direction. Four certification EPs have no CHAI control (RUAIH.03.01.01 EP3 and EP4, RUAIH.04.01.01 EP4, RUAIH.05.01.01 EP3) and three CHAI controls have no EP of their own (4.2.5, 4.3.1, 4.3.5). Both lists are in `crosswalk/README.md`.
- Not verified end to end. `crosswalk/verification.csv` records, per cell, whether the mapping was checked against primary text, a secondary summary, or not at all. Read it before relying on any cell.
- Not legal or regulatory advice, and not endorsed by Joint Commission, CHAI, URAC, NIST, ISO, FDA, or the State of Texas.

## License notes

The Joint Commission certification manual is copyright Joint Commission; this repo cites standard and EP identifiers and paraphrases. CHAI's playbooks are CC BY-NC-ND 4.0. This repo cites control IDs as facts and paraphrases each control in the author's words; it does not reproduce CHAI's text. If you use the playbooks themselves commercially, check CHAI's terms.

## Verification discipline

A cell moves to `primary` only when checked against the instrument's own text, and the change is logged in `CHANGELOG.md`. Clause numbers and standard names are mapped freely; text from paid standards is never reproduced.

## Citing

See `CITATION.cff`.

## License

MIT. Use it, fork it, improve it. Pull requests welcome, especially from clinicians and compliance officers who find a mapping wrong.
