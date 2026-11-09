Week 1 started Nov 1
# Prompt Log: Medicare Anti-Amyloid Uptake Project

How I use AI: it writes code from my plain-English requests; I run every step,
check numbers by hand, and record what it got wrong here.

---

## Nov 8, 2026 — Loading the data

**Goal:** Load CMS Part B Spending by Drug (2024 file) and pull rows for
J0172 (aducanumab), J0174 (lecanemab), J0175 (donanemab).

**Prompt:** "I'm using DuckDB in Colab. Here are my columns: [...]. Write a query
that keeps only J0172, J0174 and J0175 and shows drug name, year, spending,
claims and beneficiaries. Explain each line."

**What went wrong / what I caught:**
1. File-not-found error: I uploaded the CSV to the root folder, not /content.
   Fix: re-uploaded into /content and used a wildcard path ('/content/DSD_PTB_RY26*.csv').
2. AI asked for a table name: a CSV isn't a table until you create one.
   Fix: CREATE TABLE partb AS SELECT * FROM read_csv_auto(...).
3. AI labeled blank rows "Not marketed / no Part B claims." Wrong: CMS
   suppresses counts under 11 claims for privacy, so a blank can mean
   "too few to report," not "not marketed."
   Fix: relabeled as "No data or suppressed (<11)."

**Checked by hand:** [Fill in: one J0174 number from the CSV matched the query output]

**Learned:** Wide vs. long data (UNPIVOT turns year columns into rows);
always check how the source defines blanks before interpreting them.
