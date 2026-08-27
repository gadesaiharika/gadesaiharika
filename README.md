# Sai Harika Gade

**Research Data Analyst** at Mississippi State University · M.S. Computer Science (4.0 GPA)

I build healthcare data pipelines and the validation suites that make their numbers trustworthy.
Dimensional models, SQL reporting layers, and integrity checks — because a metric computed over a
fact table that silently duplicated rows in a join is still a number, still plausible, and
completely wrong.

Currently working on genomics pipelines in Dr. Li's lab: re-architected one from an HPC cluster to
standard workstations, cut a step's memory footprint from 32 GB to under 1 GB, and root-caused an
intermittent failure that was quietly discarding an hour of compute per run.

---

## Projects

Both run end to end from a clean clone. No manual data downloads, no Docker.

### [hrrp-readmission-analytics](https://github.com/gadesaiharika/hrrp-readmission-analytics)
**30-day readmission analytics and CMS HRRP risk** · PostgreSQL · SQL · Python · Tableau

A Caboodle-style star schema over 12,000 synthetic inpatient encounters, with 30-day all-cause
readmissions flagged by `LAG`/`LEAD` window functions and stratified across the six CMS HRRP
conditions. Slowly Changing Dimension Type 2 on the patient dimension, so an encounter from 2023
stays joined to where that patient lived in 2023.

*25 validation checks.* They caught a defect where the reported day count and the readmission flag
were derived from two different expressions — a 30.4-day gap reported as "30 days" while the flag
read false, so filtering the dashboard returned a different population than the headline rate.

### [revenue-cycle-denials](https://github.com/gadesaiharika/revenue-cycle-denials)
**Denials, AR, and collection analytics** · PostgreSQL · SQL · Python

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

---

## What I work with

| | |
|---|---|
| **Databases** | PostgreSQL, SQL Server, T-SQL — CTEs, window functions, query tuning, indexing |
| **Modeling** | Kimball dimensional modeling, star and snowflake schemas, grain definition, SCD Type 1 & 2 |
| **Healthcare data** | Epic's published Clarity/Caboodle data model, ICD-10-CM/PCS, CPT/HCPCS, DRG, CARC/RARC, HL7 v2, FHIR, HIPAA Safe Harbor, HRRP/HEDIS/MIPS |
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

[LinkedIn](https://linkedin.com/in/saiharikagade) · gadesaiharika@gmail.com · Starkville, MS
