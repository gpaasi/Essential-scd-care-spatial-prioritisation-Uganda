# Spatial prioritisation for expansion of essential sickle cell disease services across health-facility catchments in Uganda using sickle cell-malaria co-risk and existing service deficits

DOI release License: MIT R reproducibility: renv issues pull requests

## Summary

This repository implements the analytic workflow for the paper, **“Spatial prioritisation for expansion of essential sickle cell disease services across health-facility catchments in Uganda using sickle cell-malaria co-risk and existing service deficits.”** The pipeline preprocesses higher-level public health-facility catchments, routine sickle cell disease (SCD) burden data, service-availability data, and raster-derived epidemiological and population inputs; constructs catchment-level co-risk, service-deficit, and structural-vulnerability metrics; generates overall and service-specific programme-priority rankings; quantifies coverage-to-need gaps and population-weighted inequity; and produces robustness and subregional concentration summaries.

The workflow is designed to support geographic planning for expansion of four essential SCD service domains in Uganda:

- newborn screening
- hydroxyurea
- blood transfusion support
- specialised SCD clinic care

It moves from data preparation and catchment construction to co-risk estimation, composite prioritisation, service-specific rollout analysis, coverage-to-need gap assessment, population-weighted concentration analysis, subregional decomposition, and robustness evaluation under alternative weighting scenarios.

Key outputs are cleaned catchment-level analytic datasets, co-risk surfaces and summaries, overall and service-specific priority rankings, coverage-to-need gap summaries, subregional concentration tables, robustness outputs, and manuscript-ready maps, tables, and figures.

## Important interpretation note

This repository supports a **planning-oriented spatial prioritisation framework**. The catchments used here should be interpreted as **analytic planning catchments** and **modelled service geographies**, not as direct estimates of observed patient utilisation. Likewise, the co-risk construct is a **planning proxy** for inferred epidemiological need based on the overlap of HbS-related inherited risk and malaria ecology. It is not a direct estimate of SCD prevalence, expected case counts, or SCD-specific malaria burden.

## Folder layout

```text
.
├─ data/
│  ├─ raw/                  # place source inputs here
│  ├─ interim/              # intermediate analytic files
│  ├─ processed/            # cleaned and derived outputs
│  └─ metadata/             # data dictionaries and source notes
├─ R/
│  ├─ 01_step1_clean_and_harmonise_facility_masterfile.R
│  ├─ 02_step2_link_scd_burden_and_service_inventory.R
│  ├─ 03_step3_build_analytic_planning_catchments.R
│  ├─ 04_step4_extract_hbs_pfpr_and_population_inputs.R
│  ├─ 05_step5_construct_corisk_and_service_deficit.R
│  ├─ 06_step6_structural_vulnerability_and_overall_priority.R
│  ├─ 07_step7_service_specific_rollout_priorities.R
│  ├─ 08_step8_coverage_need_gaps_and_population_weighting.R
│  ├─ 09_step9_subregional_concentration_and_typology.R
│  ├─ 10_step10_robustness_analysis.R
│  └─ 11_step11_export_tables_figures_and_maps.R
├─ figures/                 # manuscript and supplementary figures
├─ tables/                  # manuscript and supplementary tables
├─ manuscript/
│  ├─ main/                 # main manuscript files
│  └─ supplementary/        # supplementary files
├─ docs/
│  ├─ README_workflow_notes.md
│  ├─ METHODS_notes.md
│  └─ RESULTS_notes.md
├─ outputs/                 # optional exported model objects and summaries
├─ config/
│  ├─ parameters.yml
│  └─ paths.yml
├─ renv/                    # reproducible R environment, if used
├─ .gitignore
├─ CITATION.cff
├─ LICENSE
├─ README.md
└─ renv.lock
```

## Required inputs

Place under `data/raw/` unless you edit paths in the scripts.

### Essential

`catchments_traveltime_voronoi1_60min_clean4.*`  
Cleaned analytic planning catchment polygons for eligible higher-level public facilities.

`facility_masterfile.*`  
Harmonised national facility master file containing facility identifiers, names, types, and coordinates.

`scd_routine_burden_2020_2024.*`  
Routine facility-level SCD admissions and SCD deaths aggregated from DHIS2 for the 2020 to 2024 study period.

`service_inventory_scd_domains.*`  
Facility-level availability for newborn screening, hydroxyurea, blood transfusion, and specialised SCD clinic care.

`hbs_surface.*`  
Raster or extracted surface representing HbS allele frequency.

`pfpr_surface.*`  
Raster or extracted surface representing *Plasmodium falciparum* parasite rate.

`population_surface.*`  
Gridded population dataset used to estimate represented catchment populations.

`uganda_subregions.*`  
Subregional boundary layer used for aggregation and contextual mapping.

`uganda_districts.*`  
District boundary layer used for reference mapping and descriptive summaries.

### Optional

`service_specific_top50_outputs.*`  
Saved summaries of top-ranked catchments for newborn screening, hydroxyurea, transfusion, and specialised clinic rollout priority.

`robustness_overlap_outputs.*`  
Saved scenario-specific overlap summaries and robustness-class outputs.

`population_impact_outputs.*`  
Saved population-weighted concentration and population-impact summaries.

## Software

- R 4.3 or newer
- GDAL, GEOS, PROJ
- Recommended workflow management with `renv`

### Main R packages

- sf
- terra
- exactextractr
- dplyr
- tidyr
- readr
- ggplot2
- tmap
- data.table
- yaml
- stringr
- purrr

Install packages as needed, or restore the project environment with:

```r
renv::restore()
```

## Run the pipeline

### Step 1. Clean and harmonise facility master data
```r
source("R/01_step1_clean_and_harmonise_facility_masterfile.R")
```
Prepares the harmonised facility reference file and verifies core identifiers and coordinates.

### Step 2. Link SCD burden and service inventory data
```r
source("R/02_step2_link_scd_burden_and_service_inventory.R")
```
Links routine SCD admissions and deaths together with service-availability indicators to the facility frame.

### Step 3. Build analytic planning catchments
```r
source("R/03_step3_build_analytic_planning_catchments.R")
```
Constructs the final analytic planning catchments using the predefined catchment framework.

### Step 4. Extract HbS, PfPR, and population inputs
```r
source("R/04_step4_extract_hbs_pfpr_and_population_inputs.R")
```
Extracts raster-derived HbS, PfPR, and population values to catchment level.

### Step 5. Construct co-risk and service deficit
```r
source("R/05_step5_construct_corisk_and_service_deficit.R")
```
Constructs standardised co-risk, service-access, and service-deficit metrics.

### Step 6. Structural vulnerability and overall programme priority
```r
source("R/06_step6_structural_vulnerability_and_overall_priority.R")
```
Generates structural-vulnerability measures and the overall programme-priority score.

### Step 7. Service-specific rollout priorities
```r
source("R/07_step7_service_specific_rollout_priorities.R")
```
Produces separate priority rankings for newborn screening, hydroxyurea, transfusion, and specialised clinic care.

### Step 8. Coverage-to-need gaps and population weighting
```r
source("R/08_step8_coverage_need_gaps_and_population_weighting.R")
```
Calculates overall and service-specific coverage-to-need gaps, population-weighted means, and population-impact summaries.

### Step 9. Subregional concentration and programme typology
```r
source("R/09_step9_subregional_concentration_and_typology.R")
```
Aggregates catchment-level outputs to subregional level and summarises concentration of priority and rollout needs.

### Step 10. Robustness analysis
```r
source("R/10_step10_robustness_analysis.R")
```
Tests stability of the overall ranking under alternative weighting scenarios.

### Step 11. Export tables, figures, and maps
```r
source("R/11_step11_export_tables_figures_and_maps.R")
```
Produces manuscript-ready tables, figures, and supplementary outputs.

## Key outputs

Typical outputs include:

- cleaned catchment-level analytic datasets
- co-risk and service-deficit summaries
- structural-vulnerability distributions
- overall programme-priority rankings
- service-specific rollout-priority rankings
- overall and service-specific coverage-to-need gap summaries
- population-weighted concentration and impact outputs
- subregional priority and typology summaries
- robustness-class and overlap outputs
- manuscript-ready figures, maps, and tables

### Example output locations

- `data/processed/`
- `figures/`
- `tables/`
- `outputs/`

## Manuscript linkage

This repository supports the paper:

**Spatial prioritisation for expansion of essential sickle cell disease services across health-facility catchments in Uganda using sickle cell-malaria co-risk and existing service deficits**

Main manuscript files are in `manuscript/main/` and supplementary files are in `manuscript/supplementary/`.

## Reproducibility

The analysis is organised as an ordered stepwise workflow. Scripts are intended to be run sequentially from Step 1 through Step 11. Intermediate objects saved by earlier steps are used by later steps for prioritisation, gap analysis, subregional interpretation, and robustness assessment.

For reproducibility:

- keep all raw inputs in `data/raw/`
- write derived files to `data/interim/` or `data/processed/`
- avoid editing outputs manually
- use `renv` to lock the R package environment
- record all path changes if adapting the workflow to a new machine
- document any restricted or non-shareable source files

## Troubleshooting

### File or path errors
Check that all required source files are present in `data/raw/` and that local paths in the scripts match your machine.

### CRS mismatch
Confirm that all spatial layers use a consistent coordinate reference system before extraction, overlay, or mapping.

### Catchment linkage problems
Check facility identifiers, coordinate validity, and polygon linkage rules if facility records fail to match the analytic planning catchments.

### Missing raster values
Inspect raster extents, masks, and alignment if HbS, PfPR, or population extraction yields unexpected missing values.

### Weighting or ranking inconsistencies
Check that prioritisation weights in `config/parameters.yml` match the manuscript specification and that scenario labels are consistent across robustness outputs.

### Sparse routine burden
Catchment-level SCD deaths and some service-specific outputs may be sparse. Interpret visible burden outputs cautiously and in relation to diagnosis, access, and referral context.

## Citing

Please cite this repository using `CITATION.cff` and cite the associated manuscript and substantive data sources used in the analysis, including:

- Uganda SCD epidemiology sources
- HbS and PfPR source surfaces
- service-availability data sources
- population raster sources
- spatial accessibility and catchment-construction methods used in the study

## License

See `LICENSE`.

Recommended approach:

- code under the MIT License
- manuscript text, tables, and figures under a Creative Commons license if desired

## Contact

**George Paasi**  
Makerere University College of Health Sciences, School of Medicine, Kampala, Uganda  
Mbale Clinical Research Institute, Mbale, Uganda  
Email: georgepaasi8@gmail.com  
ORCID: 0000-0001-6360-0589  
Postal address: P.O. Box 1966, Mbale, Uganda

Co-authors: Grace Ndeezi, Ruth Namazzi, Ezekiel Mupere, Ian Guyton Munabi, Sarah Kiguli, Richard Idro, and Peter Olupot-Olupot.
