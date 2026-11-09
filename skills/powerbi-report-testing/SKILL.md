---
name: powerbi-report-testing
description: Write repeatable Power BI report test cases, UAT scripts, and regression contracts for PBIP or published report changes. Use when asked to test report formatting, slicer-driven value changes, DAX or KPI business logic, or optional report access and row-level security. For test design, draft checks and expected results from report evidence and approved requirements.
---

# Power BI report testing

Write Markdown regression contracts a person or agent can rerun. Define expected behavior before implementation. For an enhancement, identify the check the current version should fail and the existing behaviors it must preserve. Plan to rerun the affected checks during development and the agreed regression suite before release.

## Gather evidence and scope

- Read the requested change, acceptance criteria, report pages and visuals, semantic model, existing tests, and approved KPI definitions or control totals that are available. Reuse existing test IDs and conventions. In PBIP repositories, keep the contracts in the existing test location or use `tests/powerbi-report-regression.md` beside the project. For other reports, deliver a standalone Markdown test pack.
- Identify the report and model revision, test environment, target audience, high-impact decisions, and affected pages, visuals, measures, or roles. Pin the test snapshot or fixture, refresh/as-of date, time zone or locale when relevant, and starting filter and bookmark state. Use synthetic or approved masked data for shareable examples.
- For connected inspection, prefer Fabric MCP, then Power BI MCP, using the available read-only capabilities. Read PBIP report definitions and TMDL for static checks when available. Use Desktop or the Service consumer view for checks that require rendered visuals, interactions, or real access. A static configuration check and a rendered check are separate evidence.
- Use requirements, a governed extract, an independent SQL calculation, or a worked fixture to establish the expected result. Use DAX query output as the observed measure result when testing that measure. If a rule, fixture, tolerance, or approved baseline is missing, mark only the affected case **Blocked** and name the missing input. Keep existing expectations and tolerances unless an approved business change calls for an update. Record the approval and reason beside the updated test and preserve the previous expectation in Git history.

## Choose coverage by category

Use these categories for the test pack. Use the article's layers to choose where a check belongs: source contract at ingestion, reconciliation against a trusted result, semantic model for DAX and relationships, and report behavior in the consumer view. Add source schema, freshness, or key checks when a report contract depends on them.

| Category | Write checks for |
| --- | --- |
| **Format** (`FMT`) | Labels, number formats, currency, units, display scaling, decimals, dates, conditional colors, and the layout at the agreed viewport. For layout, test the title-to-slicer and slicer-to-visual spacing, alignment, and clipping against the approved design or theme. If a specific spacing target is needed, source it from the design or flag it for confirmation. Capture the rendered evidence with page size, viewport or zoom, and filter state. |
| **Value change** (`VAL`) | How report values respond to a slicer, cross-filter, drill-through, bookmark, or reset. Start from a known state, change one interaction, and specify the expected before and after results for affected visuals and any intentionally unaffected visual. Include relevant combinations, no-data states, and reset behavior. For a refresh-driven change, compare explicitly named snapshots and identify the expected delta or invariants. |
| **Permission** (`PER`, optional) | If requested or access is in scope, separate opening the report or app from the rows a role can see. Test an authorized viewer and an out-of-scope role or identity, using the expected allow/deny and visible/excluded data from the access matrix. For RLS, use an appropriate role preview for model checks and an approved real Viewer identity for actual access checks or when simulation is unsupported. RLS does not apply to workspace Admins, Members, or Contributors. If omitted, mark **Skipped** with the reason. If required for the change but the role, identity, or access matrix is missing, mark **Blocked**. [Microsoft RLS guidance](https://learn.microsoft.com/fabric/security/service-admin-row-level-security) |
| **Business logic** (`BUS`) | What the KPI means and how it is computed. Write a worked calculation or a reconciliation against a trusted source at the same grain, filters, and snapshot. Cover the material rules for exclusions, dates, relationships, totals or subtotals, and blanks or zero denominators. Specify exact results or an approved tolerance with units. For ratios, test the total at its own filter context when the business rule requires recalculating it. |

Use value-change tests to verify that the UI applies the intended context and business-logic tests to verify that the calculation is correct in a fixed context. If both are needed, reuse the fixture and link the case IDs.

## Write the test pack

1. Open with the report and model revision, environment, scope, fixture or approved source, and default report state. Prioritize affected KPIs and user journeys over blanket visual coverage.
2. For each test, record **ID and title**, **requirement/source and purpose**, **priority**, **owner**, **target and method** (manual or automated), **Given** (preconditions, data, identity, filters), **When** (reproducible steps or query), **Then** (observable expected result, units and tolerance when relevant), **evidence to capture**, and **action on failure**. Include a reset step when the test changes state. Use the example in [a sample regression pack](references/sample-regression-pack.md) if a starting structure is needed.
3. Record **Not run** for drafted cases. Record **Pass** or **Fail** only from observed execution. Record **Blocked** with the missing prerequisite and **Skipped** with the reason. End with unresolved rules, coverage gaps, and the decisions needed for sign-off.
4. When asked only to write tests, produce the contracts. Treat test execution, report or model changes, permission changes, and sign-off as separate actions requiring their own authorization.

Method adapted from [Test Driven Power BI for the Agentic AI Era](https://medium.com/@ultraleow/test-driven-power-bi-for-the-agentic-ai-era-903bcbfce7fe). See [Microsoft guidance on validating Power BI content](https://learn.microsoft.com/en-us/power-bi/guidance/powerbi-implementation-planning-content-lifecycle-management-validate), [PBIP project structure](https://learn.microsoft.com/en-us/power-bi/developer/projects/projects-overview), and [visual title and spacing settings](https://learn.microsoft.com/en-us/power-bi/visuals/power-bi-visualization-customize-title-background-and-legend).
