# Dietary Patterns and Cardiovascular Health
**Denise Young · Python portfolio project · CDC NHANES / USDA FPED 2017–2018**

A reproducible public-health analytics workflow that links dietary recalls, laboratory measurements, blood-pressure examinations, demographics, and medication questionnaires. It demonstrates Python data integration and validation while reporting both near-null findings and model-dependent associations.

## What this project demonstrates
- Ingesting SAS XPT and SAS7BDAT data with pandas; safely reading an embedded 7-Zip archive without executing it.
- Joining seven CDC files and one USDA file using participant IDs and one-to-one cardinality checks.
- Aggregating one-to-many prescription records without duplicating participants.
- Handling missingness, eligibility criteria, and questionnaire skip patterns explicitly.
- Engineering an energy-adjusted, rank-based dietary feature.
- Comparing unweighted correlations and weighted regression coefficients.
- Exporting reproducible charts, aggregate CSVs, validation checks, source hashes, and software versions.

## Findings
The original notebook reported the following; see the executed packaged notebook and `outputs/` for reproduced results.

| Analysis | Adults | Result |
|---|---:|---|
| Day 1 fiber and total cholesterol | 4,489 | Pearson r ≈ −0.004 |
| Two-day average fiber and total cholesterol | 3,927 | Pearson r ≈ −0.020 |
| Custom score and systolic BP, unweighted | 4,591 | Pearson r ≈ −0.006 |
| Custom score and diastolic BP, unweighted | 4,591 | Pearson r ≈ −0.050 |
| Weighted model adjusting for age and reported treatment | 4,578 | Approximately −1.266 systolic and −0.800 diastolic mmHg per 10 score points |

Raw associations were small. Adjusted weighted coefficients were negative, but this does not demonstrate a causal benefit or statistical significance. The model-comparison export separates weighting from adjustment on a common sample.

![Raw score and blood-pressure relationships](outputs/plant_score_blood_pressure.png)

![Coefficients under four model specifications](outputs/model_comparison.png)

## Run it
### Google Colab
1. Upload `Public_Health_Analysis.ipynb` at https://colab.research.google.com/.
2. Select **Runtime → Run all**. The first cell installs missing packages.
3. Internet access is required for first-run public data downloads.
4. View charts in the notebook; download `outputs/` files from Colab's Files panel if needed.

### Local Python
Use Python 3.10 or later in a virtual environment:

```bash
python -m pip install -r requirements.txt
python analysis.py
```

Run from the repository root. The first run downloads public data to `data/raw/`; subsequent runs reuse it. Delete a cached file to download a fresh copy. `outputs/source_manifest.json` records source URLs and hashes. `outputs/software_versions.json` records versions used in the delivered run. Dependencies in requirements.txt allow compatible version ranges rather than an exact environment lock.

## Files
| File | Purpose |
|---|---|
| `Public_Health_Analysis.ipynb` | Annotated notebook with executed results |
| `analysis.py` | Same analysis as a runnable Python script |
| `requirements.txt` | Dependency installation |
| `outputs/` | Charts, aggregate results, provenance, and validation records |
| `METHODS.md` | Cohort rules, variables, score definition, and limitations |
| `PORTFOLIO_NOTES.md` | Resume bullet and interview talking points |
| `GITHUB_UPLOAD.md` | Instructions for publishing the package |

## Interpretation limits
This is a cross-sectional exploratory portfolio project, not a clinical study. The custom score is not a validated plant-based diet index and cannot identify habitual vegan diets. It omits refined grains, added sugars, oils, and solid fats. Medication classification does not capture every BP-affecting or cholesterol-lowering drug. One or two recall days do not establish usual intake. Age and treatment adjustment leave substantial potential confounding.

Dietary weights are used in WLS, but NHANES strata, PSUs, and domain variance estimation are not implemented. No confidence intervals or p-values are reported, and the project does not claim nationally representative causal effects. See METHODS.md.

## Sources
- [CDC NHANES 2017–2018 datasets and documentation](https://wwwn.cdc.gov/nchs/nhanes/search/DataPage.aspx?Cycle=2017-2018)
- [USDA FPED datasets](https://www.ars.usda.gov/northeast-area/beltsville-md-bhnrc/beltsville-human-nutrition-research-center/food-surveys-research-group/docs/fped-databases/)
- [USDA FPED 2017–2018 methodology](https://www.ars.usda.gov/ARSUserFiles/80400530/pdf/fped/FPED_1718.pdf)

Prepared with AI assistance. All observations come from publicly released government survey data. No private cruise data are included. The analyst should understand the workflow and limitations before claiming independent proficiency.
