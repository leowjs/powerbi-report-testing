# Power BI Report Testing

An agent skill for writing repeatable Power BI regression tests before changing a report or semantic model. Keep the Markdown test pack alongside your PBIP project and rerun it when a KPI, visual, or filter changes.

In the [sample test pack](skills/powerbi-report-testing/references/sample-regression-pack.md), Net Sales is RM100.00 for all regions, RM70.00 after selecting North, and RM100.00 after reset. A separate business-logic check reconciles those amounts against three synthetic transactions, including a cancelled sale.

| Test category | What it checks |
| --- | --- |
| Format | Layout, labels, dates, currency, units, and decimal places |
| Value change | What changes or stays stable after filtering, drill-through, or reset |
| Business logic | Whether a KPI matches a worked calculation or approved control total |
| Permission (optional) | Whether an intended viewer can open the report and see only the allowed rows |

## Use it

Clone this repository, then choose your agent:

```sh
git clone https://github.com/leowjs/powerbi-report-testing.git
```

- **Codex** — copy `skills/powerbi-report-testing` into `.agents/skills/` in your project or `~/.agents/skills/` for your user. In Codex, ask: `Use $powerbi-report-testing to write regression tests for this PBIP change. Skip permission testing.`
- **Claude Code** — start a session with `claude --plugin-dir ./powerbi-report-testing` from the parent directory. Ask: `/powerbi-report-testing:powerbi-report-testing Write regression tests for this PBIP change. Skip permission testing.`

Provide the report or PBIP diff, the affected pages and KPIs, and any approved definitions or fixtures. The skill writes test cases with observable expected results and evidence to capture. It can draft cases from local project files or supplied requirements. Fabric or Power BI MCP connections are optional sources of read-only evidence when available.

Adapted from [Test Driven Power BI for the Agentic AI Era](https://medium.com/@ultraleow/test-driven-power-bi-for-the-agentic-ai-era-903bcbfce7fe) by AI Lah. See the [MIT license](LICENSE).
