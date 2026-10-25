# Execution validation

The packaged analysis.py ran end to end against freshly downloaded CDC files and the USDA archive. All notebook code cells were also executed sequentially in a fresh Python namespace and saved with captured tables and figures. A native Jupyter kernel could not start in the packaging environment because of socket restrictions; the notebook was validated with nbformat and the same code executed without that kernel. Colab execution itself has not been independently retested after packaging.

Reproduced: 4,489 cholesterol participants; 3,927 with two recalls; 4,591 BP participants; 4,578 BP model participants. Weighted adjusted coefficients: −1.266007 systolic and −0.799894 diastolic mmHg per 10 score points.

All exported validation checks passed. No duplicate participant IDs, nonpositive regression weights, missing model variables, or treatment-count discrepancies were found. Three chart artifacts were generated; the model-comparison figure was visually reviewed. Dependency versions and download hashes are recorded in outputs/.
