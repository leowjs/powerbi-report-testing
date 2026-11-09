# Sample Power BI report regression pack

This synthetic example shows the level of detail needed to rerun a contract. Replace the sample titles, requirements, owners, and expectations with those of the report under test.

## Setup

- **Target** Sales Overview page and Net Sales card in the test workspace. Record the actual report and model revision with the evidence.
- **Fixture** `sales-v1`, loaded into the test semantic model. Fix the page period to May 2026, start with all regions, and use the default bookmark with no other report selections.
- **Rules** `REQ-01` displays Net Sales in RM to two decimals with no scaling. `REQ-02` applies the Region slicer to Net Sales and provides a reset. `REQ-03` sums posted sales less their discounts and excludes cancelled transactions. All amounts in the fixture are exact to the cent.

| ID | Month | Region | Status | Gross sales (RM) | Discount (RM) |
| --- | --- | --- | --- | ---: | ---: |
| S1 | 2026-05 | North | Posted | 80.00 | 10.00 |
| S2 | 2026-05 | South | Posted | 35.00 | 5.00 |
| S3 | 2026-05 | North | Cancelled | 25.00 | 0.00 |

## FMT-01 · Display Net Sales in RM

- **Source and purpose** `REQ-01`, so readers can interpret the amount. **Priority** Medium. **Owner** Report owner. **Method** Manual, Sales Overview page in the test Service reading view.
- **Given** `sales-v1`, default filters, and the agreed viewport. **When** Open the page and read the Net Sales card. **Then** The title reads `Net Sales`, and the value displays in RM with two decimal places and no scaling suffix. The value itself is asserted by `VAL-01` and `BUS-01`.
- **Evidence** Screenshot with the card and filter state visible, plus viewport or zoom. **On failure** Check the card's display units and format override, then the measure format string. **Status** Not run.

## VAL-01 · Filter and reset Net Sales

- **Source and purpose** `REQ-02`, using the `BUS-01` calculation. **Priority** High. **Owner** Report owner. **Method** Manual, Sales Overview page in the test Service reading view.
- **Given** `sales-v1`, May 2026, all regions. **When** Record the starting Net Sales, select North in the Region slicer, and then reset to the default bookmark. **Then** The three states display RM100.00, RM70.00, and RM100.00 respectively. The May selection remains in effect throughout.
- **Evidence** The card and slicer state at all three steps. **On failure** Check the visual interaction, effective filter context, and saved bookmark state. **Status** Not run.

## BUS-01 · Exclude cancelled sales and subtract discounts

- **Source and purpose** `REQ-03`, using the fixture rows as the independent calculation. **Priority** High. **Owner** KPI owner. **Method** Read-only DAX query against the test semantic model, or manual reconciliation in a test visual at the same filters.
- **Given** `sales-v1` and a fixed May 2026 filter. **When** Evaluate the Net Sales measure for all regions, North, and South. **Then** The results are exactly RM100.00 (`80 − 10 + 35 − 5`), RM70.00 (`80 − 10`), and RM30.00 (`35 − 5`). S3 contributes zero because it is cancelled. Compare numeric values before display formatting, with RM0.00 tolerance for this fixture.
- **Evidence** The query and results with filter context, plus the calculation above. **On failure** Investigate the status predicate, discount handling, and filter propagation. **Status** Not run.

## Permission coverage

**Skipped** for this example. The sample change has no access requirements. For a change that affects access, add separate cases for opening the report and for visible and excluded rows, as applicable, with the expected results taken from the approved access matrix.
