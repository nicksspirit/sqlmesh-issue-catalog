# SQLMesh Pending Issue Catalog & Pillar Alignment

Every open issue in [SQLMesh/sqlmesh](https://github.com/SQLMesh/sqlmesh), assigned to one primary pillar. Charts, filters, and previews are in [`index.html`](./index.html).

| | |
| --- | --- |
| **Source** | [SQLMesh/sqlmesh](https://github.com/SQLMesh/sqlmesh) (Linux Foundation, repo ID `540527604`) |
| **Snapshot** | November 20, 2026 (`2026-11-20T10:08:00-08:00`) |
| **Coverage** | All **211** open issues and all **32** labels. No sampling. |
| **Excluded** | **106** open pull requests |

## Contents

- [1. Executive Summary](#1-executive-summary)
- [2. Scope & Explicit Assumptions](#2-scope--explicit-assumptions)
- [3. Pillar Distribution Summary](#3-pillar-distribution-summary)
- [4. Lab Notebook File Index](#4-lab-notebook-file-index)

## 1. Executive Summary

[SQLMesh/sqlmesh](https://github.com/SQLMesh/sqlmesh) has **211 open issues** as of November 20, 2026. **Reliability is half the backlog (107 issues, 50.7%).** Adoption is 52 (24.6%), Scalability is 34 (16.1%), Extensibility is 15 (7.1%), and 3 issues (1.4%) stay Uncategorized.

**Routing labels do not track those pillars.** 152 of 211 issues (72.0%) have no triage label, and 60 (28.4%) have no label at all. Unlabeled issues rose from **17.3% (24/139)** before the [March 25, 2026 Linux Foundation transfer](https://www.linuxfoundation.org/press/linux-foundation-welcomes-sqlmesh-project) to **50.0% (36/72)** after it, and to **74.4% (32/43)** of issues opened since August 2026.

Eighteen of the 23 routing labels are orthogonal to the pillars. `Engine:*` and `Project:*` name a warehouse adapter or a UI surface. The planner, evaluator, and state-sync engine have no routing label.

### Reliability — 107 issues (50.7%)

Another 17 issues list Reliability as secondary (124 touched, 58.8%). Representative defects:

- Silent data loss in ClickHouse table swaps ([#6087](https://github.com/SQLMesh/sqlmesh/issues/6087))
- ClickHouse quorum overwrites ([#6171](https://github.com/SQLMesh/sqlmesh/issues/6171))
- `UNION BY NAME` schema drops ([#6212](https://github.com/SQLMesh/sqlmesh/issues/6212))
- `SEED` CSV type inference ([#6135](https://github.com/SQLMesh/sqlmesh/issues/6135))
- `at_least_one` audits that false-pass ([#6198](https://github.com/SQLMesh/sqlmesh/issues/6198))

### Adoption — 52 primary (24.6%)

Adoption is also the secondary pillar on 69 issues, so it touches 121 issues (57.3%). The cluster is:

- dbt Jinja and test compatibility ([#5295](https://github.com/SQLMesh/sqlmesh/issues/5295), [#5821](https://github.com/SQLMesh/sqlmesh/issues/5821))
- Enterprise auth and naming friction ([#4741](https://github.com/SQLMesh/sqlmesh/issues/4741), [#5066](https://github.com/SQLMesh/sqlmesh/issues/5066), [#5955](https://github.com/SQLMesh/sqlmesh/issues/5955))
- Missing scenario walkthroughs ([#4108](https://github.com/SQLMesh/sqlmesh/issues/4108), [#5580](https://github.com/SQLMesh/sqlmesh/issues/5580))

### Scalability — 34 issues (16.1%)

The urgent cluster is architectural. Non-prod `plan`, `render`, `test`, and `format` load the full project DAG ([#5905](https://github.com/SQLMesh/sqlmesh/issues/5905), [#5906](https://github.com/SQLMesh/sqlmesh/issues/5906), [#6020](https://github.com/SQLMesh/sqlmesh/issues/6020)). Pure-Python virtual-layer promotion scales at `O(N²)` and stalls 2,400-view BigQuery environments for 40–50 minutes ([#6014](https://github.com/SQLMesh/sqlmesh/issues/6014)).

Another 31 issues list Scalability as secondary (65 touched, 30.8%).

### Extensibility — 15 issues (7.1%)

- SQL-native macros ([#2505](https://github.com/SQLMesh/sqlmesh/issues/2505))
- Ossie semantic-layer generation ([#5997](https://github.com/SQLMesh/sqlmesh/issues/5997))
- ADBC drivers ([#5814](https://github.com/SQLMesh/sqlmesh/issues/5814))
- Config hooks that remove the need to subclass or fork `EngineAdapter` ([#5600](https://github.com/SQLMesh/sqlmesh/issues/5600), [#5623](https://github.com/SQLMesh/sqlmesh/issues/5623))

Another 20 issues list Extensibility as secondary (35 touched, 16.6%).

### Uncategorized — 3 issues (1.4%)

Internal maintenance or rationale questions: [#3451](https://github.com/SQLMesh/sqlmesh/issues/3451), [#5581](https://github.com/SQLMesh/sqlmesh/issues/5581), [#6147](https://github.com/SQLMesh/sqlmesh/issues/6147).

### What to change

Keep component labels, and add pillar routing on intake:

1. Require a 4-option **Primary Pillar** field.
2. Retire four dead or vague labels: `Project: Ibis`, `Scheduler: Airflow`, `UX`, `codex`.
3. Add three triage labels: `data-correctness`, `core: planner-state`, `ecosystem-integration`.

## 2. Scope & Explicit Assumptions

### 1. Pending means open

An issue counts as pending when GitHub reports `state: "open"`. That set is **211** issues. Closed issues are excluded.

GitHub's `/issues` endpoint also returns **106** open pull requests (`211 + 106 = 317` total `open_issues_count`). Those pull requests stay out of the catalog unless an open issue cites one for triage context. Examples: PR [#5033](https://github.com/SQLMesh/sqlmesh/pull/5033) inside [#5875](https://github.com/SQLMesh/sqlmesh/issues/5875), and PR `#5551` inside [#5629](https://github.com/SQLMesh/sqlmesh/issues/5629).

If "pending" means a status column on a GitHub Project board, that is Open Question 1 in [`04-alignment-and-recommendations.md`](./04-alignment-and-recommendations.md#decisions-needed).

### 2. Canonical repository

`https://api.github.com/repos/sqlmesh-org/sqlmesh` returns **HTTP 404**. After the [March 25, 2026 transfer to the Linux Foundation](https://www.linuxfoundation.org/press/linux-foundation-welcomes-sqlmesh-project), the upstream repository is **[SQLMesh/sqlmesh](https://github.com/SQLMesh/sqlmesh)**:

- `owner.login = "SQLMesh"`
- `owner.type = "Organization"`
- `fork = false`
- ID `540527604`

`TobikoData/sqlmesh` URLs HTTP-301 redirect to `SQLMesh/sqlmesh`. Every fetch used `SQLMesh/sqlmesh`.

### 3. Triage labels vs metadata labels

The repository has no `priority/*`, `severity/*`, or `status/*` workflow labels.

Maintainers route work with **23** subsystem labels: `Engine:*`, `Project:*`, `Scheduler:*`, `Table Format: Iceberg`, `Documentation`, and `UX`. They also apply **9** type or automation tags: `Bug`, `Improvement`, `Feature`, `Tech Debt`, `Good First Issue`, `dependencies`, `github_actions`, `javascript`, and `codex`.

`Engine:*` and `Table Format:*` can also be read as area metadata. [`01-label-taxonomy.md`](./01-label-taxonomy.md#per-issue-distribution) reports the per-issue distribution under both readings.

### 4. Which pillar wins when cues collide

**Reliability** is primary when documented or expected behavior crashes, corrupts data, races, or regresses. That stays true for a dbt migration (secondary: Adoption) and for high concurrency (secondary: Scalability). The brief calls these out as always prioritized: bug fixes, performance degradations, and correctness issues.

**Scalability** is primary when current behavior works on small projects and the limit is architectural. That covers `O(N²)` promotion, full-DAG loading, and missing partition-copy or table-swap strategies.

### 5. Exhaustive pass

All 211 open issues, all 32 label definitions, and comment threads on issues whose body is under 120 characters came from the GitHub REST API (`2022-11-28`). No rate-limit drops. Sampling margin of error is **0%**.

## 3. Pillar Distribution Summary

Primary is the pillar used for sorting. Secondary mentions are issues that name the pillar second. Touch count is primary plus secondary.

| Primary Pillar | Open Issues | % of Backlog (211) | Secondary Mentions | Total Touch Count (%) | High / Med / Low Confidence |
| --- | ---: | ---: | ---: | ---: | ---: |
| **[Reliability](./03-pillar-catalog.md#pillar-reliability)** | 107 | 50.7% | 17 | 124 (58.8%) | 105 / 2 / 0 |
| **[Adoption](./03-pillar-catalog.md#pillar-adoption)** | 52 | 24.6% | 69 | 121 (57.3%) | 40 / 5 / 7 |
| **[Scalability](./03-pillar-catalog.md#pillar-scalability)** | 34 | 16.1% | 31 | 65 (30.8%) | 33 / 1 / 0 |
| **[Extensibility](./03-pillar-catalog.md#pillar-extensibility)** | 15 | 7.1% | 20 | 35 (16.6%) | 14 / 0 / 1 |
| **[Uncategorized](./03-pillar-catalog.md#pillar-uncategorized)** | 3 | 1.4% | — | 3 (1.4%) | 2 / 1 / 0 |
| **Total** | **211** | **100.0%** | **137** | — | **194 / 9 / 8** |

## 4. Lab Notebook File Index

| File | Output Sections Covered | Purpose |
| --- | --- | --- |
| [`index.html`](./index.html) | Interactive stakeholder dashboard | Charts, multi-filter table, pillar and label views, inline markdown inspector. |
| [`README.md`](./README.md) | (1) Executive summary, (2) Assumptions, (4) Pillar summary | Entry point, methodology, scope, and top-level metrics. |
| [`01-label-taxonomy.md`](./01-label-taxonomy.md) | (3) Triage label taxonomy and classification | All 32 labels, triage vs metadata, borderline cases, and `0 / 1 / 2+` distributions. |
| [`02-cue-rubric.md`](./02-cue-rubric.md) | (6) Reusable cue-sensing rubric | Cues taken from SQLMesh issues, tie-breaks, and confidence rules. |
| [`03-pillar-catalog.md`](./03-pillar-catalog.md) | (4) Pillar counts and (5) Per-issue catalog | All 211 issues, sorted by primary pillar then issue number, with verbatim cues. |
| [`04-alignment-and-recommendations.md`](./04-alignment-and-recommendations.md) | (7) Label–pillar alignment and (8) Gaps and decisions | Verdict per triage label, keep / rename / merge / retire, three proposed labels, and open decisions. |
