# UHI Risk Assessment Toolbox

An ArcGIS Pro Python toolbox for calculating Urban Heat Island (UHI) risk based on the IPCC AR5 climate risk framework (IPCC, 2014) and the GIZ (Deutsche Gesellschaft für Internationale Zusammenarbeit GmbH) Vulnerability Sourcebook and its Risk Supplement (GIZ, 2014; GIZ and EURAC, 2017).

This tool was developed as part of the master's thesis *"Urban Heat Islands in
Olomouc: A Geospatial Identification of Thermal Patterns, Urban Structure, and
Human Perception"*, submitted to Paris-Lodron University Salzburg
and Palacký University Olomouc as part of the Erasmus Mundus Joint Master's
Degree Copernicus Master in Digital Earth.

The thesis and web mapping application applying this tool to Olomouc, Czech
Republic can be found at: **https://www.geoinformatics.upol.cz/dprace/magisterske/kiefer26/**

## What it does

The tool combines any number of pre-processed, normalized indicators into the four
risk components **Hazard**, **Exposure**, **Sensitivity** and **Adaptive
Capacity** and aggregates them into a final **Risk** score, all through a
graphical interface in ArcGIS Pro. It does not require any coding to use.

It adds six new fields to a copy of your input layer:

| Field | Description |
|---|---|
| `UHI_Hazard` | Weighted average of hazard indicators |
| `UHI_Exposure` | Weighted average of exposure indicators |
| `UHI_Sensitivity` | Weighted average of sensitivity indicators |
| `UHI_AdaptiveCapacity` | Weighted average of adaptive capacity indicators |
| `UHI_Vulnerability` | `Sensitivity × (1 − AdaptiveCapacity)` |
| `UHI_Risk` | Weighted aggregation of Hazard, Exposure and Vulnerability |

### Vulnerability formula

Vulnerability is calculated as:

```
V = S × (1 − AC)
```

where `S` is sensitivity and `AC` is adaptive capacity (adapted from Voelkel
et al., 2018). This is a deliberate departure from the additive formula
suggested in the GIZ Vulnerability Sourcebook, which was
found to overestimate vulnerability in areas with zero sensitivity but low
adaptive capacity. With the multiplicative formula, adaptive capacity can only
**reduce** existing sensitivity, it cannot generate vulnerability on its own.

### Risk formula

```
R = (H·wH + V·wV + E·wE) / (wH + wV + wE)
```

where `H`, `V`, `E` are Hazard, Vulnerability and Exposure, and `wH`, `wV`,
`wE` are their optional component weights (default: equal weighting).

Indicator-level weights work the same way within each component and are
optional, if left blank, all indicators in a component are weighted equally.
Weights don't need to sum to 1; the tool normalizes them automatically.

## Requirements

- ArcGIS Pro (developed and tested on version 3.5.4)
- Input layer: any polygon feature class (e.g. a hexagonal grid)
- All indicator fields must be **pre-processed and normalized to a 0–1 range**,
  where higher values always represent higher hazard, exposure, sensitivity,
  or adaptive capacity

## Installation

1. Download `UHI_Risk_Toolbox.atbx` from this repository.
2. In ArcGIS Pro, go to the **Catalog** pane → right-click **Toolboxes** →
   **Add Toolbox** → select the downloaded `.atbx` file.
3. The tool will appear as **UHI Risk Assessment**.

## Usage

1. Prepare your input polygon layer with normalized (0–1) indicator fields.
2. Open the tool and set the **Input layer**.
3. For each of the four components (**Hazard**, **Exposure**, **Sensitivity**,
   **Adaptive Capacity**), add the relevant indicator fields in the value
   table and optionally assign weights.
4. Optionally set component-level weights for the final Risk calculation.
5. Set an **Output layer** path.
6. Run. The output is a copy of your input layer with the six new fields
   listed above.

### Example indicator classification (as used in the Olomouc case study)

| Component | Example indicators |
|---|---|
| Hazard | Daytime/nighttime UHI intensity (land surface temperature) |
| Exposure | Urban structure/land use, daytime/nighttime population density |
| Sensitivity | Vulnerable population groups, vulnerable infrastructure, reported thermal discomfort |
| Adaptive Capacity | Healthcare access, urban green space, reported thermal comfort |

Note: Indicator classification is study-area dependent and can vary, see
the discussion in the accompanying thesis for the reasoning behind this
particular setup, especially the placement of thermal comfort/discomfort
indicators (Chapter 8.1–8.2).

## Data and reproducibility

This toolbox operates only on the indicator fields you provide, it does not
include or require any specific dataset. Some datasets used in the original
Olomouc case study (e.g. address points)
were shared under data-sharing agreements with Palacký University Olomouc and
are **not included** in this repository. The toolbox and script itself are
freely reusable and transferable to other study areas.

## Citation

If you use this tool, please cite:

> Kiefer, A. M. (2026). *Urban Heat Islands in Olomouc: A Geospatial
> Identification of Thermal Patterns, Urban Structure, and Human Perception*
> [Master's thesis]. Paris-Lodron University Salzburg & Palacký University
> Olomouc.

## References

- GIZ. (2014). *The Vulnerability Sourcebook: Concept and guidelines for
  standardised vulnerability assessments.*
- GIZ and EURAC. (2017). *Risk Supplement to the Vulnerability Sourcebook.*
- IPCC. (2014). *Climate Change 2014: Synthesis Report.* Intergovernmental
  Panel on Climate Change.
- Voelkel, J., Hellman, D., Sakuma, R., & Shandas, V. (2018). Assessing
  Vulnerability to Urban Heat: A Study of Disproportionate Heat Exposure and
  Access to Refuge by Socio-Demographic Status in Portland, Oregon.
  *International Journal of Environmental Research and Public Health, 15*(4),
  640. https://doi.org/10.3390/ijerph15040640
