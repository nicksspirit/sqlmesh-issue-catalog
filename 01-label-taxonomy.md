# Phase 1 — Triage Label Taxonomy & Classification

[← Back to Lab Notebook Index](./README.md) | [Open Interactive Dashboard](./index.html)

## Headline Counts

| Metric | Value | Notes |
| :--- | ---: | :--- |
| **Open Issues Total (`pending = open`)** | **211** | Excludes `106` open pull requests (`317` combined GitHub open items). |
| **Distinct Repository Labels** | **32** | Retrieved from `GET /repos/SQLMesh/sqlmesh/labels` (`6` labels currently have `0` open issues). |
| **Distinct Triage Labels (Classify / Route)** | **23** | `13` `Engine:*`, `5` `Project:*`, `2` `Scheduler:*`, `1` `Table Format:*`, `Documentation`, `UX` (`20` active, `3` unused). |
| **Distinct Metadata Labels** | **9** | `Bug`, `Improvement`, `Feature`, `Tech Debt`, `Good First Issue`, `dependencies`, `github_actions`, `javascript`, `codex`. |

<a id="per-issue-distribution"></a>
## Per-Issue Label Coverage Distribution (`0 / 1 / 2+`)

Because `SQLMesh/sqlmesh` has no `priority/*` or `status/*` labels, we report per-issue triage-label counts under our primary classification (**23 domain/adapter routing labels**) alongside two sensitivity checks: a **strict workstream-only** view (treating `Engine:*` and `Table Format:*` as metadata area tags, leaving 9 workstream labels) and **all 32 labels combined**.

| Bucket | Primary Triage Labels (23 labels) | Strict Workstream Triage Only (9 labels) | All Repository Labels Combined (32 labels) |
| :--- | ---: | ---: | ---: |
| **`0` labels on issue** | **152** (72.0%) | **190** (90.0%) | **60** (28.4%) |
| **`1` label on issue** | **57** (27.0%) | **21** (10.0%) | **88** (41.7%) |
| **`2+` labels on issue** | **2** (0.9%) | **0** (0.0%) | **63** (29.9%) |
| **Total Open Issues** | **211 (100.0%)** | **211 (100.0%)** | **211 (100.0%)** |

**Key operational takeaway:** Nearly three-quarters (**72.0%**) of open issues carry zero routing labels, and **60 issues (28.4%)** have no labels at all. Only 2 issues carry `2+` triage routing labels: [#4724](https://github.com/SQLMesh/sqlmesh/issues/4724) (*Support S3Tables*, tagged with 4 `Engine:*` labels) and [#5392](https://github.com/SQLMesh/sqlmesh/issues/5392) (*Iceberg Catalog Type Mismatch*, tagged `Engine: Spark` + `Table Format: Iceberg`). Meanwhile, post-[Linux Foundation transfer](https://www.linuxfoundation.org/press/linux-foundation-welcomes-sqlmesh-project) (`>= 2026-03-25`), completely unlabeled open issues rose from **17.3% (24/139)** to **50.0% (36/72)**.

## Reasoning on Borderline Label Cases

1. **`Engine:*` (13 labels) and `Table Format: Iceberg` (1 label) — Classified as `Triage` (Borderline Area vs. Routing):** In repositories that maintain dedicated team/queue labels, warehouse dialect tags act as passive `area` metadata. In `SQLMesh/sqlmesh`, however, every `Engine: <Name>` maps directly to an isolated `EngineAdapter` module (`sqlmesh/core/engine_adapter/<engine>.py`) and dedicated CI test workflow ([#5970](https://github.com/SQLMesh/sqlmesh/issues/5970), [#5973](https://github.com/SQLMesh/sqlmesh/issues/5973)), serving as the repo's primary specialist-routing mechanism. We classify them as **Triage (route)** while flagging in Phase 3 that they are **orthogonal** to strategic pillar prioritization.
2. **`Bug` (51), `Improvement` (66), `Feature` (37), and `Tech Debt` (1) — Classified as `Metadata` (Borderline Nature vs. Priority Proxy):** Per brief (`metadata (bug, enhancement, good first issue, version/area tags)`), issue-kind tags describe *what kind of code change* is requested rather than routing ownership. Although `Bug` correlates strongly with Reliability (`47/51 = 92.2%`), maintainers co-apply `Feature` + `Improvement` together on `11` issues (including regression [#5629](https://github.com/SQLMesh/sqlmesh/issues/5629)) and `Bug` + `Improvement` on `4` issues ([#3260](https://github.com/SQLMesh/sqlmesh/issues/3260), [#5612](https://github.com/SQLMesh/sqlmesh/issues/5612), [#5638](https://github.com/SQLMesh/sqlmesh/issues/5638), [#5770](https://github.com/SQLMesh/sqlmesh/issues/5770)), proving they operate as non-exclusive descriptive tags rather than clean triage queues.
3. **`UX` (1) and `Documentation` (5) — Classified as `Triage`:** `Documentation` routes issues out of core Python engineering into `docs/` authoring. `UX` (`Issues related to user experience such as config setting, cli output, debugging`) was designed as a cross-cutting triage lens, though with just 1 tagged open issue ([#4996](https://github.com/SQLMesh/sqlmesh/issues/4996)) it is effectively abandoned in practice.
4. **`dependencies` (1 issue, 5 PRs) and `codex` (0 issues, 0 PRs) — Classified as `Metadata`:** `dependencies`, `github_actions`, and `javascript` are auto-managed by Dependabot for PRs (`dependencies` was manually attached once to DuckDB `MERGE` issue [#4985](https://github.com/SQLMesh/sqlmesh/issues/4985)). `codex` has no description and zero open items, functioning as an AI-agent provenance tag.

## Complete 32-Label Inventory & Taxonomy

| Label Name | Hex Color | Repo Description | Open Issues | Open PRs | Class | Classification Reasoning |
| :--- | :--- | :--- | ---: | ---: | :--- | :--- |
| <a id="label-improvement"></a>`Improvement` | `#0E8A16` | Improves existing functionality | 66 | 0 | **Metadata** | Enhancement-nature tag (`Improves existing functionality`). Equivalent to GitHub's default `enhancement` metadata tag; frequently co-applied alongside `Feature` (10 issues) and `Bug` (4 issues), confirming non-exclusive descriptive usage. |
| <a id="label-bug"></a>`Bug` | `#D73A4A` | Something isn't working | 51 | 0 | **Metadata** | Issue-nature tag (`Something isn't working`). Explicitly categorized as metadata per prompt (`bug, enhancement, good first issue, version/area tags`), though borderline in practice because 92.2% (47/51) of `Bug` issues map to Reliability. |
| <a id="label-feature"></a>`Feature` | `#47B494` | Adds new functionality | 37 | 0 | **Metadata** | Enhancement-nature tag (`Adds new functionality`). Tracks change type rather than team routing or strategic priority; co-occurs with `Improvement` on 10 open issues (including regression `#5629`). |
| <a id="label-engine-duckdb"></a>`Engine: DuckDB` | `#9A55A9` | *(none)* | 7 | 0 | **Triage** | Borderline (`Engine:*` adapter routing vs area tag): SQLMesh lacks `team/*` or `priority/*` labels, so `Engine:*` routes issues to `sqlmesh/core/engine_adapter/duckdb.py` maintainers. |
| <a id="label-project-vscode-extension"></a>`Project: VSCode Extension` | `#E0C6F5` | *(none)* | 6 | 0 | **Triage** | Sub-project routing label directing TypeScript IDE/LSP issues (`vscode/`, `sqlmesh/lsp/`) to extension maintainers. |
| <a id="label-documentation"></a>`Documentation` | `#0075CA` | Improvements or additions to documentation | 5 | 0 | **Triage** | Workstream routing label directing conceptual guides and walkthroughs (`docs/`) to documentation/DevRel authors. |
| <a id="label-engine-clickhouse"></a>`Engine: Clickhouse` | `#9A55A9` | Issues related to Clickhouse | 5 | 0 | **Triage** | Borderline (`Engine:*` adapter routing): routes issues to ClickHouse adapter & cluster/Cloud mode specialists. |
| <a id="label-engine-redshift"></a>`Engine: Redshift` | `#9A55A9` | Issues related to Redshift | 5 | 0 | **Triage** | Borderline (`Engine:*` adapter routing): routes issues to Amazon Redshift adapter maintainers. |
| <a id="label-engine-databricks"></a>`Engine: Databricks` | `#9A55A9` | Issues related to Databricks | 4 | 0 | **Triage** | Borderline (`Engine:*` adapter routing): routes Databricks SQL / Databricks Connect / Snowpark-equivalent DF issues. |
| <a id="label-engine-postgres"></a>`Engine: Postgres` | `#9A55A9` | *(none)* | 4 | 0 | **Triage** | Borderline (`Engine:*` adapter routing): routes PostgreSQL adapter & state-backend issues. |
| <a id="label-engine-snowflake"></a>`Engine: Snowflake` | `#9A55A9` | Issues related to Snowflake | 4 | 0 | **Triage** | Borderline (`Engine:*` adapter routing): routes Snowflake / Snowpark / Iceberg-on-Snowflake issues. |
| <a id="label-engine-sql-server"></a>`Engine: SQL Server` | `#9A55A9` | *(none)* | 4 | 0 | **Triage** | Borderline (`Engine:*` adapter routing): routes MSSQL / Azure SQL / T-SQL dialect & `pymssql`/`pyodbc` issues. |
| <a id="label-project-dbt"></a>`Project: dbt` | `#1D76DB` | Work related to supporting dbt interface | 3 | 0 | **Triage** | Workstream routing label (`Work related to supporting dbt interface`) directing `sqlmesh_dbt` adapter and Jinja compatibility issues. |
| <a id="label-project-ui"></a>`Project: UI` | `#FBCA04` | Issues related to creating the UI | 3 | 0 | **Triage** | Sub-project routing label (`Issues related to creating the UI`) for the legacy/local `web/` browser interface. |
| <a id="label-engine-athena"></a>`Engine: Athena` | `#9A55A9` | *(none)* | 2 | 0 | **Triage** | Borderline (`Engine:*` adapter routing): routes AWS Athena / Glue catalog adapter issues. |
| <a id="label-engine-fabric"></a>`Engine: Fabric` | `#9A55A9` | Issues related to Microsoft Fabric | 2 | 0 | **Triage** | Borderline (`Engine:*` adapter routing): routes Microsoft Fabric warehouse & T-SQL issues. |
| <a id="label-engine-trino"></a>`Engine: Trino` | `#9A55A9` | *(none)* | 2 | 0 | **Triage** | Borderline (`Engine:*` adapter routing): routes Trino adapter and dialect issues. |
| <a id="label-good-first-issue"></a>`Good First Issue` | `#E8C6AB` | *(none)* | 2 | 0 | **Metadata** | Contributor-onboarding suitability tag (`2` open issues). Explicitly categorized as metadata in prompt (`good first issue`). |
| <a id="label-project-cicd-bot"></a>`Project: CICD Bot` | `#A59F86` | *(none)* | 2 | 0 | **Triage** | Sub-project routing label directing GitHub Actions CI/CD bot workflow issues (`sqlmesh/integrations/github/`). |
| <a id="label-dependencies"></a>`dependencies` | `#0366D6` | Pull requests that update a dependency file | 1 | 5 | **Metadata** | Automated Dependabot PR metadata tag (`Pull requests that update a dependency file`; `5` open PRs, loosely applied to `1` issue `#4985`). |
| <a id="label-engine-bigquery"></a>`Engine: BigQuery` | `#9A55A9` | issues related to BigQuery | 1 | 0 | **Triage** | Borderline (`Engine:*` adapter routing): routes Google BigQuery adapter issues (severely under-applied: only 1 of 9 BigQuery issues carries it). |
| <a id="label-engine-spark"></a>`Engine: Spark` | `#9A55A9` | *(none)* | 1 | 0 | **Triage** | Borderline (`Engine:*` adapter routing): routes PySpark / Spark SQL / HMS catalog issues. |
| <a id="label-scheduler-built-in"></a>`Scheduler: Built-in` | `#BDF691` | *(none)* | 1 | 0 | **Triage** | Subsystem routing label for SQLMesh's built-in cron/interval execution scheduler (`1` open issue `#5843`). |
| <a id="label-table-format-iceberg"></a>`Table Format: Iceberg` | `#86C1CB` | *(none)* | 1 | 0 | **Triage** | Borderline (`Table Format:*` routing): routes Apache Iceberg catalog/DDL integration issues across Spark, Snowflake, S3 Tables, and `pg_lake`. |
| <a id="label-tech-debt"></a>`Tech Debt` | `#D94D0E` | *(none)* | 1 | 0 | **Metadata** | Internal code-health/refactor descriptor (`1` open issue: `#6147` e2e test sharing). Describes engineering hygiene nature rather than intake routing. |
| <a id="label-ux"></a>`UX` | `#1D76DB` | Issues related to user experience such as config setting, cli output, debugging | 1 | 0 | **Triage** | Borderline cross-cutting triage label (`config setting, cli output, debugging`) with only `1` open issue (`#4996`). |
| <a id="label-codex"></a>`codex` | `#EDEDED` | *(none)* | 0 | 0 | **Metadata** | Automated AI-agent provenance marker (`0` open issues, `0` open PRs, empty description). Borderline due to blank description, but functions as bot provenance metadata. |
| <a id="label-engine-mysql"></a>`Engine: MySQL` | `#9A55A9` | *(none)* | 0 | 0 | **Triage** | Borderline (`Engine:*` adapter routing): MySQL engine label with `0` open issues. |
| <a id="label-github-actions"></a>`github_actions` | `#000000` | Pull requests that update GitHub Actions code | 0 | 0 | **Metadata** | Automated Dependabot PR metadata tag (`Pull requests that update GitHub Actions code`; `0` open issues, `0` open PRs). |
| <a id="label-javascript"></a>`javascript` | `#168700` | Pull requests that update javascript code | 0 | 5 | **Metadata** | Automated Dependabot PR metadata tag (`Pull requests that update javascript code`; `0` open issues, `5` open PRs). |
| <a id="label-project-ibis"></a>`Project: Ibis` | `#326978` | *(none)* | 0 | 0 | **Triage** | Dormant sub-project routing label with `0` open issues and empty description. |
| <a id="label-scheduler-airflow"></a>`Scheduler: Airflow` | `#BDF691` | *(none)* | 0 | 0 | **Triage** | Legacy scheduler routing label with `0` open issues (Airflow state scheduler was deprecated/removed). |
