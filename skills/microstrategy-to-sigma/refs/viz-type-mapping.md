# Dossier visualization types → Sigma element kinds

What the dossier API exposes per visualization is **shallow**: only
`visualizationType` + `definition.grid` (`rows` / `columns` / `metrics` /
`crossTab` / `metricsPosition`). There are no slot/encoding details beyond
which attributes and metrics sit on which grid axis. The converter therefore
maps `visualizationType` → a Sigma element kind via this lookup, feeds the
grid axes into the closest Sigma slots, and falls back to a **flagged table**
(data preserved, loud warning) for anything unmapped — never silently wrong.

**Status legend** — *extraction validated*: the type was pulled from real
demo-content dossiers and its `definition.grid` parsed as documented.
*Build*: whether the Sigma-element emission is part of the validated path
(the live-validated fixture used grid reports; chart emission is roadmap).

| MSTR `visualizationType` | Sigma element kind | Extraction | Build |
|---|---|---|---|
| `grid` | `table` (pivot when `crossTab: true`) | validated | **validated (parity-verified)** |
| `kpi` | `kpi-chart` | validated | roadmap |
| `multi_metric_kpi` | one `kpi-chart` per metric | validated | roadmap |
| `bar_chart` | `bar` | validated | roadmap |
| `line_chart` | `line` | validated | roadmap |
| `combo_chart` | `combo` | validated | roadmap |
| `heat_map` | flagged `table` (no clean Sigma analog for MSTR's size+color tile grid) | validated | roadmap |
| `microcharts` | `table` + flagged sparkline columns | validated | roadmap |
| *anything else* (scatter, bubble, histogram, box plot, waterfall, map, sankey, network, funnel, word cloud, …) | flagged `table` fallback | — | fallback |

Free-form page `fields` (text boxes / images) → Sigma `text` elements (text
content carries over; images are flagged).

`fixtures/BUILD_SPEC.md` is the drag-and-drop spec for an "every viz type"
fixture dossier (REST cannot author visualizations — Library UI only); use it
to extend the validated set.

Keep `VIZ_MAPPED` in `../../microstrategy-assessment/scripts/assess.py` in
sync with this table — the assessment classifies estates against it.
