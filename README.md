# BOLD Mortality Prediction Model

Predicting whether an ICU patient died during their hospital admission, using only labs
and vitals available at a single point in time, and explaining which biomarkers drive
that prediction.

Early pipeline development for the COMFORT project, built on the BOLD ICU dataset.

## Data

The intended source is real BOLD, a credentialed PhysioNet dataset:
https://physionet.org/content/blood-gas-oximetry/1.0/

Certification is currently in progress. Until then the notebook runs on
`synthetic_bold_dataset.csv`, fabricated data using real BOLD column names. It exists
only to keep the code runnable, and the notebook prints a warning banner while it's in
use. Switching to real data is one line:

```python
DATA_PATH = "bold_dataset.csv"
```

## Done so far

**Step 1: Load and inspect.** Reports shape, column types and missing data per column.
2,000 patients, 71 columns.

**Step 2: Define the label.** The target is `in_hospital_mortality`. Checks it's clean,
then reports the class balance: 15.2% died, roughly 5.6 survivors per death. Because
deaths are rare, scoring uses AUROC and AUPRC rather than accuracy.

**Step 3: Pick the predictors.** 57 columns kept, covering labs, vitals, blood gas, SOFA
severity scores and demographics. 14 excluded to avoid leakage: future SOFA scores,
length of stay, IDs, and the outcome itself. These are only known at or after the
outcome, so using them would let the model read the answer backwards.

## Still to do

Steps 4 to 9 (cleaning, train/test split, logistic regression, Random Forest with SHAP,
confounder check, subgroup check) are waiting on real BOLD access. They produce actual
results, which aren't worth generating on fabricated data.

## Running it

```bash
python3 -m venv .venv
.venv/bin/pip install -r requirements.txt
.venv/bin/jupyter lab bold_mortality_pipeline.ipynb
```

Run top to bottom.
