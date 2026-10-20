# impact-assessment

A method for assessing one clinical AI system before launch and at every material change, worked through on an inpatient sepsis early-warning model.

Status: **author** method (see `SOURCES.md` for status values). The regulatory and standards references it relies on are listed under [References](#references) with their verification status. Example values are placeholders in square brackets; replace them with local validation data. This is not legal, regulatory or clinical advice.

The method is designed to plug into a hospital's existing clinical safety process, not replace it. It adds what that process usually does not check for software: who pays for each kind of error, error rates by group, whether human review is real, and AI-specific reassessment triggers.

## Step 0 — Declare which assessment this is

Every assessment declares one primary purpose on its first page. The four purposes ask different questions of different evidence, so a finished one never stands in for another. A privacy review in particular says nothing about whether the model harms patients when it is wrong.

| Assessment | Question it answers | Basis | Required when | Can it substitute for the others? |
| --- | --- | --- | --- | --- |
| 42001 AIMS impact assessment | What are the consequences of this AI system for individuals, groups and society, and are they acceptable? | ISO/IEC 42001 clause 6.1.4, with the Annex A.5 controls (including A.5.2, the impact assessment process); guidance in ISO/IEC 42005:2025 (S6, S10) | The organization operates or is certifying an AI management system | No. It covers impacts but does not test HIPAA safeguards or EU fundamental-rights duties. |
| EU AI Act fundamental rights impact assessment (FRIA) | What risks does this high-risk system pose to the fundamental rights of the people it is used on, and how are they governed? | AI Act Article 27 (S11) | A public-law body or public-service provider deploys a stand-alone (Annex III) high-risk system in the EU, such as emergency patient triage. Medical-device AI is high-risk through product law instead, and Article 27 does not cover it. Annex III obligations apply from 2 December 2027 under the AI Omnibus, Regulation (EU) 2026/1744 (S12). | No. It is rights-focused and EU-only. |
| HIPAA / privacy review | Is protected health information used, disclosed and secured lawfully? | HIPAA Security Rule risk analysis (45 CFR 164.308(a)(1)(ii)(A)); Privacy Rule minimum-necessary standard | Any system that creates, receives, stores or transmits PHI | No. It says nothing about clinical error, bias or oversight. |
| Internal launch gate | Is this system safe and fit enough to switch on for these patients, in this workflow, today? | The organization's own AI governance policy and clinical decision support committee; informed by the Joint Commission / CHAI RUAIH guidance (S2) | Before go-live and at every reassessment trigger (Step 6) | No. It is local and does not satisfy external obligations. |

U.S. covered entities should also note the Section 1557 rule on patient care decision support tools, 45 CFR 92.210 (S13). Since May 1, 2025, it has required reasonable efforts to identify tools whose input variables measure race, color, national origin, sex, age or disability, and to mitigate the discrimination risk from their use. The sepsis model uses age, so it is in scope, and Step 3 produces the evidence. Check the current enforcement posture with counsel; the rule's text is in force.

**Worked example declaration:** Internal launch gate (primary) for a U.S. hospital, with its records reused as evidence for the hospital's 42001 A.5 controls. The HIPAA risk analysis is done separately. A FRIA is not applicable, because the system is not deployed in the EU.

## Step 1 — Profile the system

One row per field. Any field left blank becomes an open item in Step 5. The assessment covers exactly the version, setting and population written here and nothing else.

| Field | Worked example: inpatient sepsis early-warning model |
| --- | --- |
| System and version | Vendor sepsis risk model, version [x.y], embedded in the EHR |
| Intended use | Flags adult inpatients at elevated risk of sepsis so a clinician evaluates them sooner |
| Decision it influences | Whether and when a rapid-response nurse evaluates the patient, and whether a sepsis bundle starts |
| What it does not decide | Diagnosis, antibiotic choice, ICU transfer |
| Users | Rapid-response nurses (receive alerts); attending physicians (receive escalations) |
| Setting | Adult medical-surgical wards at [sites]; excludes ICU, ED, pediatrics, obstetrics |
| Population | Adults 18+ admitted to included wards; about [n] admissions a year |
| Output | Risk score from 0 to 100; alert fires at a score of [threshold] or above |
| Autonomy | Advisory. A human evaluates every alert, and nothing happens to the patient automatically. |
| Inputs | Vital signs, labs, nursing assessments, recorded at the EHR's normal charting intervals |
| Training data | Vendor-trained on [description]; not trained on this hospital's data |
| Local validation | [Silent-mode retrospective or prospective study, dates, n] — see Step 1c |
| Regulatory status | A medical device. FDA's January 2026 clinical decision support guidance (Example 11) treats software that detects sepsis and sounds an alarm as a device function, because it gives a specific output for a time-critical decision (S14). Confirm the vendor's FDA clearance for this version and intended use. |
| System owner | [Named person, title] |
| Accountable executive | [Named person, title, e.g. Chief Medical Officer] |

## Step 1b — Clinical hazard analysis (required input)

**Gate: don't start Step 2 until the hospital's hazard analysis for this system exists.** Use the hospital's own process: its failure mode and effects analysis, hazard log or proactive risk assessment. This method doesn't replace that process; it adds the AI-specific questions on top. Error rates (Step 3) don't capture the failures that happen around the model, so the hazard log has to cover at least these:

| Hazard | Example in the sepsis system |
| --- | --- |
| Wrong patient or encounter | Alert fires on a transferred patient's old encounter |
| Wrong recipient | Alert routes to an off-shift nurse, or to no one at shift change |
| Duplicate or suppressed alerts | Repeated alerts for a patient already being treated; alerts silenced after a dismissal |
| Stale or missing inputs | Vital signs charted hours late, so the score reflects the past |
| Downtime | EHR, interface or vendor outage; no score, and no one notices |
| Misreading the display | The score is read as a probability, or a missing score is read as "low risk" |
| Automation bias | Clinicians stop screening patients the model doesn't flag |

Each hazard is rated for severity, likelihood and detectability, gets a control and a named owner, and its control is carried into the Step 5 register.

Two controls are mandatory, whatever the hazard ratings show:

1. **Downtime procedure:** a written fallback, such as the existing nursing sepsis screen, saying who switches to it, how they're notified of the outage, and when it was last tested.
2. **Usability test:** at least [5] representative users on the actual EHR build, before go-live, with every misread recorded and fixed or logged as a risk.

## Step 1c — Validation protocol

The protocol is written and signed by the clinical lead **before anyone looks at the data**, so the numbers in Step 3 can't be shaped after the fact.

1. **Outcome definition, fixed in writing.** Choose one primary definition of sepsis, such as a clinical surveillance definition with clinician adjudication, and say why. Report one alternative definition, such as billing codes, as a sensitivity analysis. The choice can change measured sensitivity more than any group gap, so it's never left implicit.
2. **What counts as a true early warning.** An alert counts only if it fires at least [x] hours before onset and before treatment has already started. Alerts on patients already receiving antibiotics are counted separately; they warn no one.
3. **Design and size.** A silent-mode run, where the model scores live patients but no one sees the alerts, for at least [8–12] weeks, or a retrospective cohort of at least [12] months. Either must be big enough to give every Step 3 group at least [50] positive cases, or that group is reported as insufficient data.
4. **Baseline.** Measure the current practice, such as the existing nursing screen, over the same period. The model is judged against what the hospital does today, not against nothing.
5. **Clinical endpoints.** Besides model metrics, track time to antibiotics, ICU transfers and in-hospital mortality from go-live, against the baseline.
6. **How trigger values are set.** Each Step 5 trigger value comes from the silent run, for example its baseline plus or minus [2] standard deviations of monthly variation, with the clinical lead's rationale written next to it. No trigger value is a guess.

## Step 2 — Stop condition: who pays for each error

**If you cannot name who bears the cost of a false positive and who bears the cost of a false negative, stop.** The assessment cannot continue, because the threshold, the oversight design and the mitigations in Steps 3–5 all depend on it.

Fill every cost cell. A cell marked "unknown" or "n/a" without a written reason counts as blank.

| Error | Who bears the cost | What the cost is | Severity |
| --- | --- | --- | --- |
| False negative (septic patient not flagged) | **Patient** | Delayed evaluation and antibiotics; organ failure; death | Catastrophic |
| False negative | Clinician | Missed deterioration on their watch; event review | Moderate |
| False negative | Payer / hospital | Longer stay, ICU days, readmission | Major |
| False positive (patient flagged without sepsis) | **Clinician** | Evaluation time; alert fatigue, which lowers response to true alerts | Moderate on its own, major once alerts get ignored |
| False positive | Patient | Unnecessary blood cultures, lactate draws and antibiotics; *C. difficile* and resistance risk | Minor to moderate |
| False positive | Payer / hospital | Tests, antibiotics, staff time | Minor |

**Reading for the example:** the false-negative cost falls mainly on the patient and is catastrophic. The false-positive cost falls mainly on clinicians and grows through alert fatigue, which then raises the false-negative cost. The threshold has to be set with both in view, and Step 4 has to show that reviewers are not drowning in alerts.

## Step 3 — Errors in each direction, by group

Report both error directions for the whole population and for every group the data allows, from local validation data (Step 1c), not the vendor's brochure. Each number carries its sample size and a 95% confidence interval.

| Group | Positive cases (n) | Sensitivity (missed cases = 1 − sensitivity) | False-positive rate | Alerts per 100 patient-days | PPV | Calibration (observed vs predicted) |
| --- | --- | --- | --- | --- | --- | --- |
| All included patients | [n] | [ ] | [ ] | [ ] | [ ] | [ ] |
| Age 18–64 / 65–79 / 80+ | | | | | | |
| Sex | | | | | | |
| Race and ethnicity, as recorded | | | | | | |
| Preferred language: English / other | | | | | | |
| Payer: commercial / Medicare / Medicaid / uninsured | | | | | | |
| Site and ward | | | | | | |
| Record completeness: group field missing | | | | | | |

**Rules:**

1. **Missing group data is an open risk, never evidence of equal outcomes.** If a group field is missing for more than [10]% of patients, or a group has fewer than [50] positive cases, write "insufficient data" in that row and log it as a risk in Step 5 with an owner and a date to close it. Never write "no difference observed" for a group you could not measure.
2. **Report the missing-data row itself.** Patients with no recorded race or language are often not a random sample, so their error rates are a finding in their own right.
3. **Gap threshold.** A difference in sensitivity of more than [5] percentage points between any group and the overall population, with confidence intervals that do not overlap, goes to the risk register. So does a group whose alert rate is more than [1.5] times the overall rate.
4. **Both directions, always.** A group with good sensitivity and twice the false-positive rate is not a pass; its clinicians are the ones who will start ignoring alerts.
5. **Proxies are not measurements.** Payer and language stand in for socioeconomic status and access. Label them as proxies, and don't claim fairness for a group the data doesn't record.

## Step 4 — Human oversight test

"Human in the loop" counts as a mitigation only when all four of these are written down and measured. If any one is missing, oversight earns no credit, and the risk ratings in Step 5 are scored as if no human were there.

| Requirement | What to record | Worked example |
| --- | --- | --- |
| Named reviewer role | The job title that reviews each output, and how many people hold it on each shift | Rapid-response nurse, [2] per shift per site |
| Volume | Items per reviewer per hour at the median and the peak, measured from logs, not estimated | Median [3] alerts per hour, peak [8] |
| Time per item | The time the review actually needs (chart review, bedside check) compared with the time available | Bedside evaluation takes [15–20] minutes |
| Authority to overrule | Whether the reviewer can dismiss or escalate without asking anyone, and where that decision is documented | Can dismiss with a documented reason, or escalate to the attending; both are logged in the EHR |

**When oversight is a rubber stamp:** if the peak volume times the time each item needs exceeds the reviewer's available time, the review is not happening. Volumes in the hundreds per hour are never real review. In the example, a peak of [8] alerts an hour at [15] minutes each is two hours of work in every hour, so the design fails at peak and the risk register needs an entry for it.

**Watch the override rate after go-live.** A dismissal rate near 0% suggests the alerts are rubber-stamped. A rate near 100% suggests clinicians have stopped trusting them. Either one is a reassessment trigger (Step 6).

## Step 5 — Risk register: every mitigation has an owner, a measure and a trigger

A mitigation without all four of owner, measure, trigger value and action is a wish, not a control, and does not lower the risk rating. Owners are named roles here, and named people in the signed copy. Trigger values are set by the method in Step 1c, item 6.

| Risk | Who it hurts | Mitigation | Owner | Measure | Trigger value | Action when triggered |
| --- | --- | --- | --- | --- | --- | --- |
| Missed sepsis in a group where sensitivity is lower (Step 3) | Patient | Group-specific monitoring; threshold review | Sepsis program clinical lead | Sensitivity by group, monthly | Gap of more than [5] points vs overall for 2 consecutive months | Clinical decision support committee reviews within 30 days; adjust threshold or restrict use |
| Reviewer overload at peak (Step 4) | Patient, clinician | Peak-hour staffing, or alert suppression for patients already under evaluation | Nursing director, rapid response | Peak alerts per reviewer per hour | More than [4] per hour on more than [3] shifts a month | Staffing review; repeat the human oversight test |
| Alert fatigue lowers response to true alerts | Patient, clinician | Monitor time-to-evaluation and dismissal rate | Sepsis program clinical lead | Median minutes from alert to evaluation; dismissal rate | Median above [30] minutes, or dismissal rate above [70]% | Reassessment (Step 6) |
| Race and language data missing, so equity is unmeasured | Patient groups not recorded | Improve registration capture; audit the sample by hand | Health equity officer | Share of records missing race or language | Above [10]% at the quarterly check | Equity claims stay open and cannot be reported as "no difference" |
| Model drift after EHR or vendor updates | Patient | Monthly performance report against the baseline | System owner | Overall sensitivity and alert rate vs baseline | Sensitivity down more than [5] points, or alert rate changed by more than [25]% | Reassessment; suspend if the clinical lead judges it unsafe |
| Input data delayed (vital signs charted late) | Patient | Track charting latency on included wards | Nursing informatics lead | Median minutes from vital sign taken to charted | Above [60] minutes on any ward | Ward-level remediation; that ward is excluded until fixed |

## Step 6 — Reassessment triggers

Any one of these reopens the assessment from Step 1. The system owner is responsible for noticing them, and "we didn't know" doesn't count as an exception.

1. **New model version**, including a vendor retrain, a threshold change or a change of input fields, even when the vendor calls it minor.
2. **New population**: a new site, ward, age group, or a change in who is admitted to included wards.
3. **New purpose**: any use beyond Step 1's intended use, such as using the score for staffing, billing or ICU triage.
4. **Metric breach**: any trigger value in Step 5 hit.
5. **Incident or complaint pattern**: one serious safety event involving the system, or [3] or more related reports in a quarter from patients, clinicians or the patient safety reporting system. Every safety event involving the system goes into the hospital's existing patient safety event system, tagged as AI-involved, and the system owner takes part in its root cause analysis.
6. **Upstream change**: an EHR upgrade, a lab interface change or a charting workflow change that alters the model's inputs.
7. **Calendar**: if nothing else triggers it, reassess every [12] months.

## Step 7 — Decision and sign-off

The decision is one of three: **approve**, **approve with conditions** (each condition is a Step 5 row with a date), or **do not approve**. An assessment stopped at Step 2 is recorded as "not assessable" and cannot be approved.

| Role | Signs for | Name | Date |
| --- | --- | --- | --- |
| System owner | Accuracy of the profile and evidence, and running the risk register | | |
| Accountable executive | Accepting the remaining risk on behalf of the organization | | |
| Licensed clinical approver (CMIO, or chair of the clinical decision support committee) | Clinical safety: hazard analysis, validation and oversight are adequate for use on patients | | |
| Assessor (preparer only) | Having followed the method; does not approve | | |

**The signature belongs to people, never to the tool.** This method, any software or a vendor's model card can prepare the record, but none of them can approve it. Approval needs all three of the system owner, the accountable executive and the licensed clinical approver; any one of them can block.

## Known limits

- Every trigger value and threshold is a placeholder until set from local data by the Step 1c method.
- The hazard list in Step 1b is a minimum, not a full hazard analysis; the hospital's own process supplies the rest.
- The worked example is one system type. A generative or ambient-documentation tool needs different error definitions in Step 2 and different metrics in Step 3.

## References

IDs refer to `SOURCES.md`. Status follows the repo's convention.

| ID | Used for | Status |
| --- | --- | --- |
| S2 | RUAIH guidance as an input to the internal launch gate | primary |
| S6 | ISO/IEC 42001 clause 6.1.4 and Annex A.5 / A.5.2 | secondary — checked against a published requirements summary; verify wording against the purchased standard before quoting |
| S10 | ISO/IEC 42005:2025 exists as impact assessment guidance | secondary |
| S11 | EU AI Act Article 27 scope and contents | primary |
| S12 | AI Omnibus application dates | secondary |
| S13 | 45 CFR 92.210 duties and compliance date | primary for text; secondary for the May 1, 2025 compliance date |
| S14 | FDA CDS guidance (January 2026), Example 11 | secondary |
