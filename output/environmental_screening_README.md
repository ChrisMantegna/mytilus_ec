# Environmental source-screening workflow

## Files

- `environmental_source_screening.qmd` — executable Quarto EDA and screening template, piloted on the 2019–2025 Ecology marine-water abiotic and nutrient tables.

## Start here

1. Place the QMD in the **root of the RStudio project** (the folder that contains `data/`).
2. Set the YAML `params.abiotic_file` and `params.nutrient_file` paths to the two combined CSVs. In this pilot they default to `data/analysis/abiotic_all.csv` and `data/analysis/nutrients_all.csv`.
3. Install prerequisites in R if needed: `install.packages(c("tidyverse", "lubridate", "knitr", "quarto"))`. Install the Quarto CLI separately if RStudio doesn't already have it.
4. Render in RStudio or use `quarto render environmental_source_screening.qmd` from the project root.
5. Review the generated HTML and `outputs/environmental_screening/` CSV summaries, figures, metadata/dictionary, suitability template and task log.

## Critical interpretation notes

- `apply_qc` is **false by default**. All descriptive and seasonal outputs are preliminary until variable-specific, documented QC flags are attached and reviewed.
- The data dictionaries and suitability decisions are **templates**; they do not automatically become verified facts or classifications.
- The source's nitrate/nitrite/etc. units must be verified from the original NetCDF metadata before comparing agencies.
- Profiles are distinct from depth-bin records. Nutrient samples are sparse discrete measurements at selected depths; do not treat them as continuously profiled.
- Some 2019–2025 tables may have changing schemas. Fix mismatches explicitly in a preprocessing step rather than silently omitting variables.
- The `0–13 m` rope zone and four depth bins are exploratory, not assumed to represent uniform actual mussel occupation.
- Station crosswalks must be validated, not automatically matched on approximate coordinate proximity alone.

## Scaling up

Maintain three linked artifacts for **each environmental source**: (1) source/version registry, (2) variable/method/QC dictionary, and (3) source-by-use inclusion decision. Preserve the separate regional, station-level, and mussel-linked evaluation tracks. Archive original data and perform any QC exclusions in new derivative tables, not in the raw source.

Recommended project layout:

```
mytilus_ec/
  data/
    raw/             # immutable agency downloads; may be excluded from ordinary Git
    interim/         # parsed sources, joined identifiers, QC annotations
    analysis/        # review-approved analytic tables
    metadata/        # crosswalks, source registry, units/method dictionary
  scripts/           # extraction and harmonization scripts
  reports/           # source-specific QMD screening reports
  outputs/           # exported figures, QC tables, review templates
```

Quarto renders produce source-specific outputs under `outputs/environmental_screening/`. Before running a copied report for another source, give it a new `source_id` **and** `output_dir` to avoid overwriting previous files.
