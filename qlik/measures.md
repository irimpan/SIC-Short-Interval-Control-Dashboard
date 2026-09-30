# SIC – KPI definitions (Qlik master measures)

These are the measures behind the line-side SIC screens. The same expressions are created as variables at the end of `SIC_load_script.qvs`.

| KPI | Qlik expression | What it means |
|---|---|---|
| Good packs | `Sum(GoodPacks)` | Packs counted by the line counter |
| Target packs | `Sum(TargetPacks)` | Standard rate (from ERP) × hours in the selection |
| Attainment % | `Sum(GoodPacks) / Sum(TargetPacks)` | Output against target for the product being run |
| Downtime (mins) | `Sum(DowntimeMins)` | Stop time from machine sensors, split by hour |
| Availability % | `1 - Sum(DowntimeMins) / (Count(DISTINCT %LineHourKey) * 60)` | Share of the hour the line was running |
| Number of stops | `Count(DISTINCT EventID)` | Stops recorded by the sensors |
| Waste % | `Sum(RejectedPacks) / (Sum(GoodPacks) + Sum(RejectedPacks))` | Rejected packs as a share of everything produced |
| Waste cost (£) | `Sum(WasteCost_GBP)` | Rejected packs × cost per pack (ERP) |
| Output value (£) | `Sum(GoodValue_GBP)` | Good packs × cost per pack |
| Packs per labour hour | `Sum(GoodPacks) / Sum(Aggr(Only(CrewSize), %LineHourKey))` | Productivity: output per person per hour |

## Screens

- **Line view (on the screen next to each line):** current hour against target, hour-by-hour bar chart for the shift, downtime minutes and top stop reasons, waste %, attainment %. Colour turns red when an hour falls below 85% attainment.
- **Shift summary:** all lines side by side for the current shift, compared with the previous three shifts. This is the basis for the shift-end KPI email.
- **Day view:** production day (06:00–06:00) totals by line, shift and product. This is the basis for the end-of-day report.

## Colour rules used on the line screens

| Attainment | Colour |
|---|---|
| 95% and above | Green |
| 85–95% | Amber |
| Below 85% | Red |
