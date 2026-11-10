# Construclytic formula dataset

An open export of the published calculator formulas used on [construclytic.com](https://construclytic.com). Each record is taken from the site’s public [formula registry](https://construclytic.com/formula-registry) and from the same TypeScript engine that runs the calculators: the formula identifier and version, the expression, the input variables and their units, the default waste or coverage assumptions, any source citations already attached to that calculator, and the live calculator URL.

This file set is a dataset, not a design manual. The formulas are planning takeoffs. They are not a structural design, a building-code approval, a mix design or a supplier quote. Source lists are copied only where the engine already cites a reference; 91 of the 137 records have no attached citation, and none is added here.

## Cite this dataset

> Construclytic formula dataset, construclytic.com

Also see `CITATION.cff`. The data are licensed under [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) (`LICENSE`). The engine and export script live in the [site repository](https://github.com/FerozBhai01/construction-intelligence-platform) and follow that repository’s license.

## Files

| File | Contents |
| --- | --- |
| `formulas.json` | Canonical machine-readable export (137 formulas). |
| `formulas.csv` | The same records, one row per formula, with variables, assumptions and sources flattened. |
| `CITATION.cff` | Citation metadata. |
| `LICENSE` | CC BY 4.0. |

The export was generated from the published engine on 10 November 2026 (Asia/Qatar).

## Schema

Each object in `formulas.json` under `formulas` has:

| Field | Meaning |
| --- | --- |
| `id` | Stable formula identifier, for example `CONC-SLAB-001`. |
| `version` | Formula version string from the engine. |
| `calculator_id` | Calculator slug, for example `concrete-slab`. |
| `name` | Public calculator title. |
| `category` | Trade cluster (Concrete, Masonry, Roofing, and so on). |
| `description` | Short description from the calculator definition. |
| `formula` | Formula expression returned by the engine for that calculator. |
| `variables` | Inputs: `key`, `label`, `hint`, `unit_imperial`, `unit_metric`, `default_imperial`, `default_metric`, limits, and any options or visibility rules. Stored defaults use the engine’s imperial base except where a unit is fixed (the two unit fields then match). |
| `default_waste_percent` | Default waste or allowance input, or `null` if the calculator has no waste field. |
| `assumptions` | The assumption lines the calculator itself prints. |
| `example` | `calculate()` output for those default inputs, in imperial and metric. |
| `sources` | `{ label, url }` citations already present on the definition; an empty array when the engine has none. |
| `url` | The matching construclytic.com calculator page. |
| `registry_url` | The same formula’s anchor on the public registry. |

CSV columns are `id`, `version`, `calculator_id`, `name`, `category`, `description`, `formula`, `variables`, `default_waste_percent`, `assumptions`, `sources`, `url` and `registry_url`.

## Worked examples

These three examples use the default inputs stored in this dataset and match the `example` object on each record.

### 1. Concrete slab (`CONC-SLAB-001`)

[Concrete slab calculator](https://construclytic.com/calculators/concrete/concrete-slab)

Formula: `L × W × D × quantity ÷ 27 × (1 + waste) → ready-mix order`

A 20 ft × 12 ft rectangle, 4 in thick, one slab, 8% waste:

- Net volume = 20 × 12 × (4 ÷ 12) = 80 ft³ = 80 ÷ 27 ≈ 2.96 yd³
- After waste, the engine rounds the ready-mix order up to the next 0.25 yd³, which is 3.25 yd³

The metric default is the same slab, and the order is 2.5 m³. Bag-yield lines and the other assumptions are on the record; they are planning figures, not a product label.

### 2. Steel weight (`GCC-STEEL-001`)

[Steel weight calculator](https://construclytic.com/calculators/concrete/steel-weight)

Formula: `kg/m = π × (diameter mm / 1000)² / 4 × density; kg = kg/m × bar length × count × (1 + waste/100)`

A 12 mm bar, 12 m long, 10 bars, density 7,850 kg/m³, no extra length:

- kg/m = π × (0.012)² / 4 × 7,850 = 0.8878 kg/m
- Total = 0.8878 × 12 × 10 = 106.538 kg

Diameter is always millimetres and bar length is always metres, as labelled on the calculator.

### 3. Laminate boxes (`FLOOR-LAMINATE-001`)

[Laminate flooring calculator](https://construclytic.com/calculators/flooring/laminate)

Formula: `L × W × (1 + waste) ÷ stated coverage`

An 18 ft × 14 ft room, 20 ft² printed coverage per carton, 10% waste:

- Net area = 18 × 14 = 252 ft²
- Area including waste = 252 × 1.10 = 277.2 ft²
- Cartons = ceil(277.2 ÷ 20) = 14

Coverage must be replaced with the carton label you will buy. The metric default of the same room also orders 14 cartons.

## Regenerating the export

The generator is `scripts/export-formula-dataset.ts` in [construction-intelligence-platform](https://github.com/FerozBhai01/construction-intelligence-platform). From a checkout of that repository:

```bash
node --import tsx scripts/export-formula-dataset.ts --out ../construction-calculator-formulas
```

It reads only the published, approved calculator definitions and the `calculate()` result for each calculator’s defaults. It does not invent formulas, waste factors or source citations.

## Browse the calculators

- Formula registry: https://construclytic.com/formula-registry
- How the site tests formulas: https://construclytic.com/how-we-calculate
- Every record’s `url` field points at its calculator page.
