# Power BI Report Testing

**The last skill you need in your Power BI workflow.** Once your report is built, use this skill to write the QA checks that tell you whether it’s ready to ship.

A report can look right while a slicer shows the wrong total or a KPI includes cancelled sales. Give the skill your report or PBIP diff and the expected behavior. It writes a Markdown test pack with steps, expected results, and evidence to capture. Keep the pack next to your PBIP project and rerun it when the report changes.

## What it checks

| Test category | What it checks |
| --- | --- |
| Format | Layout, labels, dates, currency, units, and decimal places |
| Value change | What changes or stays stable after filtering, drill-through, or reset |
| Business logic | Whether a KPI matches a worked calculation or approved control total |
| Permission (optional) | Whether an intended viewer can open the report and see only the allowed rows |

In the [sample test pack](skills/powerbi-report-testing/references/sample-regression-pack.md), Net Sales should show RM100.00 for all regions, RM70.00 after selecting North, and RM100.00 after reset. A separate business logic check reconciles those amounts against three synthetic transactions, including a cancelled sale.

## Use it

Clone this repository, then choose your agent:

```sh
git clone https://github.com/leowjs/powerbi-report-testing.git
```

- **Codex** — copy `skills/powerbi-report-testing` into `.agents/skills/` in your project or `~/.agents/skills/` for your user. In Codex, ask: `Use $powerbi-report-testing to write regression tests for this PBIP change. Skip permission testing.`
- **Claude Code** — start a session with `claude --plugin-dir ./powerbi-report-testing` from the parent directory. Ask: `/powerbi-report-testing:powerbi-report-testing Write regression tests for this PBIP change. Skip permission testing.`

Include the affected pages and KPIs and any approved definitions or fixtures. Fabric and Power BI MCP connections can supply additional read-only evidence when available.

Adapted from [Test Driven Power BI for the Agentic AI Era](https://medium.com/@ultraleow/test-driven-power-bi-for-the-agentic-ai-era-903bcbfce7fe) by AI Lah. See the [MIT license](LICENSE).

## Star History

If this skill helps with your report QA, give it a star.

<a href="https://www.star-history.com/#leowjs/powerbi-report-testing&amp;Date">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=leowjs/powerbi-report-testing&amp;type=Date&amp;theme=dark">
    <img alt="GitHub star history for Power BI Report Testing" src="https://api.star-history.com/svg?repos=leowjs/powerbi-report-testing&amp;type=Date">
  </picture>
</a>
