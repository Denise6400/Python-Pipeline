# Resume and interview notes

## Suggested resume bullet
Built a reproducible Python workflow integrating eight CDC NHANES and USDA data files; validated participant-level joins, engineered dietary features, and exported visualizations and exploratory weighted regression results for a 4,591-adult blood-pressure sample.

## Skills supported by this project
Python, pandas, NumPy, Matplotlib, statsmodels, Jupyter/Google Colab, data cleaning, data integration, feature engineering, descriptive analysis, exploratory regression, reproducible reporting.

## Interview explanation
“I linked public dietary, laboratory, examination, demographic, and medication datasets. I checked join cardinality, handled multiple prescriptions per participant, and preserved questionnaire skip patterns. I explored fiber and cholesterol, then constructed a transparent dietary score for blood-pressure analysis. Raw associations were weak; weighted adjusted coefficients differed. I documented those differences and the limitations instead of claiming causation.”

## Be ready to explain
- What SEQN identifies and why one-to-one merge validation matters.
- Why medication rows must be aggregated before joining to people.
- Missing values versus zero values; skipped questions versus explicit no responses.
- Why energy adjustment and within-component ranking avoid directly summing different units.
- Why the custom score is not a validated diet index.
- Why survey weights, confounder adjustment, and complex-survey variance are different tasks.
- Why weak or null associations are still reportable findings.

This project was developed with AI assistance. Review and run the notebook yourself; do not claim advanced survey-statistics expertise or independent authorship of methods you cannot explain.
