# Methods and data dictionary

## Input files
All datasets refer to the 2017–2018 cycle. CDC base URL: https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2017/DataFiles/ . Append the file identifier and `.htm` for the codebook or `.xpt` for the data.

| File | Purpose | Key variables |
|---|---|---|
| DR1TOT_J | First dietary recall | SEQN, DR1DRSTZ, WTDRD1, WTDR2D, DR1TFIBE, DR1TKCAL |
| DR2TOT_J | Second dietary recall | SEQN, DR2DRSTZ, DR2TFIBE |
| DEMO_J | Demographics and survey design | SEQN, RIDAGEYR, SDMVSTRA, SDMVPSU |
| TCHOL_J | Total serum cholesterol | SEQN, LBXTC (mg/dL) |
| RXQ_RX_J | Prescription reports | SEQN, RXDUSE, RXDDRUG |
| BPX_J | Blood pressure | SEQN, BPXSY1–3, BPXDI1–3 (mmHg) |
| BPQ_J | Reported hypertension treatment | SEQN, BPQ020, BPQ040A, BPQ050A |
| FPED_DR1TOT_1718 | USDA food-group equivalents | SEQN and food groups below |

## Cohorts
- Cholesterol sample: age ≥20, reliable first recall (DR1DRSTZ=1), positive WTDRD1, observed fiber and total cholesterol.
- Two-recall cholesterol sample: additionally DR2DRSTZ=1, observed Day 2 fiber, and positive WTDR2D. Calculate arithmetic average fiber. Both comparison correlations use the same subset and are unweighted.
- BP sample: age ≥20, reliable first recall, positive WTDRD1, and observed mean systolic and diastolic readings. This is constructed independently of cholesterol availability.
- BP score sample: matched food groups, complete scoring components, positive reported energy.
- BP regression: exclude unknown treatment status; hold this sample fixed across model specifications.

## Blood-pressure outcome rule
Arithmetic mean of available readings 1–3, separately for systolic and diastolic. Missing readings are skipped; all missing yields missing. Fourth attempts are not used. Zero diastolic values are retained as recorded. This exploratory rule is explicit and reproducible; alternatives should be assessed before epidemiological inference. No hypertension diagnosis is inferred from these means.

## Statin feature
Case-insensitive ingredient substring matching for atorvastatin, rosuvastatin, simvastatin, pravastatin, lovastatin, fluvastatin, pitavastatin. Matching finds combination products. Collapse records using any statin match. Unknown medication records use RXDUSE not in {1,2}, or RXDUSE=1 with missing/blank drug or codes 55555/77777/99999. A known statin match takes priority over other unknown drugs. "No statin reported" is not equivalent to no lipid-lowering medication. No automatic exclusion based on high fiber values is applied.

## Custom dietary score
| Component | Source | Direction | Original unit |
|---|---|---|---|
| Whole fruit | F_TOTAL minus F_JUICE | Higher raises score | cup equivalents |
| Vegetables excluding legumes | V_TOTAL | Higher raises score | cup equivalents |
| Whole grains | G_WHOLE | Higher raises score | ounce equivalents |
| Legumes | PF_LEGUMES | Higher raises score | ounce equivalents |
| Soy | PF_SOY | Higher raises score | ounce equivalents |
| Nuts/seeds | PF_NUTSDS | Higher raises score | ounce equivalents |
| Meat/poultry/seafood | PF_MPS_TOTAL | Higher lowers score | ounce equivalents |
| Eggs | PF_EGGS | Higher lowers score | ounce equivalents |
| Dairy | D_TOTAL | Higher lowers score | cup equivalents |

Columns have the prefix `DR1T_`. Divide each component by DR1TKCAL and multiply by 1,000. Within the eligible BP sample, calculate unweighted percentile ranks with average ties; negate animal-food densities before ranking. Average all nine component ranks and multiply by 100. Do not sum raw cup and ounce equivalents. Legumes enter only once. A score point is relative to this sample and not a percentage of plant energy. Ranks are fixed before treatment-status exclusions.

This is a custom score, not PDI, hPDI, HEI, a clinical threshold, or evidence of habitual vegan eating. Separate soy and legumes components give these categories explicit equal component weights. Many zero intakes create ties and limited resolution. Refined grains, added sugar, solid fats, and oils are not scored. No score was optimized against BP outcomes, but the research direction and score were developed exploratorily after viewing earlier fiber/cholesterol results; this is not a preregistered confirmatory study.

## Questionnaire treatment categories
- BPQ050A=1: reported current BP medicine.
- BPQ050A=2: explicitly reported no current BP medicine.
- Missing BPQ050A with BPQ020=2 or BPQ040A=2: no BP treatment reported, question skipped.
- Otherwise unknown.

Skips are not silently treated as explicit "no" answers. These categories capture reported treatment for hypertension, not every drug capable of lowering BP.

## Models
For each BP outcome, fit unweighted OLS and dietary-weighted WLS, unadjusted and adjusted, on the identical complete-case sample. Exposure = score/10. Adjustment = age centered at 50, its square, and categorical treatment status. Weight = WTDRD1. Examine coefficients only. Survey strata/PSUs are retained but not used for variance estimation. Default model confidence intervals are not reported.

Weighting is not confounding adjustment. Neither removes unmeasured confounding. Treatment could be related to prior BP and diet changes, complicating causal interpretation. No claim of significance, national effect, or causation is supported. Future work: validated diet indices, repeat food-group recalls, complex-survey methods, sex and lifestyle covariates, alternate BP averaging, and sensitivity analyses.
