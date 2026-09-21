# Sai Harika Gade

**Research Data Analyst** at Mississippi State University · M.S. Computer Science (4.0 GPA)

I build data pipelines and the validation suites that make their numbers trustworthy.
Dimensional models, SQL reporting layers, and integrity checks — because a metric computed over a
fact table that silently duplicated rows in a join is still a number, still plausible, and
completely wrong.

Currently working on genomics pipelines in Dr. Zhang's lab: re-architected one from an HPC cluster to
standard workstations, cut a step's memory footprint from 32 GB to under 1 GB, and root-caused an
intermittent failure that was quietly discarding an hour of compute per run.

---

## Projects

All four run end to end from a clean clone, no Docker. Three generate their own data; the fourth downloads a federal public file itself.

### [hrrp-readmission-analytics](https://github.com/gadesaiharika/hrrp-readmission-analytics)
**30-day readmission analytics and CMS HRRP risk** · PostgreSQL · SQL · Python · [Tableau dashboard](https://public.tableau.com/app/profile/sai.harika.gade/viz/HRRPReadmissionDashboard/Dashboard2)

A Caboodle-style star schema over 11,920 synthetic inpatient encounters, with 30-day all-cause
readmissions flagged by `LAG`/`LEAD` window functions and stratified across the six CMS HRRP
conditions. Slowly Changing Dimension Type 2 on the patient dimension, so an encounter from 2023
stays joined to where that patient lived in 2023.

*25 validation checks.* They caught a defect where the reported day count and the readmission flag
were derived from two different expressions — a 30.4-day gap reported as "30 days" while the flag
read false, so filtering the dashboard returned a different population than the headline rate.

### [revenue-cycle-denials](https://github.com/gadesaiharika/revenue-cycle-denials)
**Denials, AR, and collection analytics** · PostgreSQL · SQL · Python · [Tableau dashboard](https://public.tableau.com/app/profile/sai.harika.gade/viz/RevenueCycleDenialsAR/Dashboard1)

A three-grain billing warehouse over 85,000 synthetic claims and 480,000 remittance postings.
Denial Rate, First-Pass Yield, Net Collection Rate, Days in AR, and charge lag, each defined once in
SQL and enforced by a test.

The distinction it turns on: **CO-45 and PR-1/2/3 are not denials.** They are the contractual
write-down and patient responsibility, and they appear on normally adjudicated claims. Counting
adjustment codes instead of denial codes produces denial rates above 90%.

*39 validation checks*, including the claim-level financial identity asserted to the cent:
`charge = contractual + insurance payment + patient payment + bad debt + write-off + open balance`.

Findings: prior authorization is 50% of denied dollars, 85% of denied dollars were preventable, and
$10.6M sits in denials nobody worked.

### [complaint-resolution-analytics](https://github.com/gadesaiharika/complaint-resolution-analytics)
**Consumer complaint resolution** · PostgreSQL · SQL · Python · **real public data**

17.9 million real complaints from the CFPB Consumer Complaint Database, streamed out of the published
bulk file into a PostgreSQL star schema without ever unpacking it to disk.

The analysis turns on the taxonomy. The CFPB renamed its product categories in 2017 and again in
2023, both hard cutovers. Charted as published, credit reporting appears to collapse to zero in 2024
— the year it tripled — and August 2023 double-counts, because both labels are live that month.

Findings: credit card complaints close with monetary relief 16.71% of the time and credit reporting
complaints 0.05% of the time. Credit reporting is 81.75% of the file, so the 1.28% overall rate is a
statement about product mix rather than company behaviour. The three credit bureaus receive 78% of
all complaints and behave nothing like each other — 59.46% of TransUnion complaints end in some
relief against 12.42% at Experian.

*24 validation checks.* The one the project turns on returns seven broken series against the
published labels and none against the mapped ones. It cannot catch a one-way rename, where a label
dies and never returns; a second check covers that case, and the README says so rather than claiming
one check is enough.

### [hl7-interface-monitor](https://github.com/gadesaiharika/hl7-interface-monitor)
**HL7 v2 parsing, validation, and interface health** · Python, standard library only

Parses HL7 v2 message traffic — segments, fields, repetitions, components, escape sequences — with
the delimiters read from each message's MSH header rather than assumed. 21 validation rules across
structure, header, patient, visit, order, and result, each with a severity that says what actually
happens to the message: rejected outright, or filed anyway with something wrong inside it.

That second category is the point. Critical failures announce themselves; warnings are what turn
into "why does this report look wrong" three months later.

*Verified two ways.* A 173-check self-test requires every injectable fault to trigger its rule, and
120 clean messages to produce zero findings. Separately, the run reconciles the generator against
the detector: **1,047 of 1,047 injected faults detected**. That started at 92.4%, and closing the
gap surfaced three real defects — including a date validator that accepted February 30th, and a
duplicate-detection test that was undetectable by construction because the injector chose its donor
in generation order while messages are written in timestamp order.

---

## What I work with

| | |
|---|---|
| **Databases** | PostgreSQL, SQL Server, T-SQL — CTEs, window functions, query tuning, indexing |
| **Modeling** | Kimball dimensional modeling, star and snowflake schemas, grain definition, SCD Type 1 & 2 |
| **Healthcare data** | Epic's published Clarity/Caboodle data model, ICD-10-CM/PCS, CPT/HCPCS, DRG, CARC/RARC denial codes, HL7 v2 messaging (ADT/ORM/ORU), FHIR, HIPAA Safe Harbor, HRRP/HEDIS/MIPS |
| **Python** | Pandas, NumPy, SQLAlchemy, scikit-learn, matplotlib |
| **BI** | Tableau, Power BI, Excel |
| **Practice** | Data validation and reconciliation, root-cause analysis, technical documentation, Git and pull-request review |

I have studied Epic's publicly documented data model and built against it with synthetic data. I
have not worked in a production Epic environment, and nothing here uses real patient data.

---

## Publication

**Computational Tools for Modeling Respiratory Disease Spread** — *under review*
Li Zhang, Michael E. Navicky, Saikanth Ratnavale, Pavan Dharma Adapa, Sai Harika Gade

Co-authored the predictive data-modeling and computational-parallelization sections.

---

## Reach me

[Portfolio](https://gadesaiharika.github.io) · [LinkedIn](https://linkedin.com/in/saiharikagade) · gadesaiharika@gmail.com · Starkville, MS
