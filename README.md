# Galaxy photometric redshifts with SDSS DR17

Author: Dr. Suraj Poudel

An educational astronomy and machine-learning project predicting spectroscopic redshift from five-band photometry. The executed tutorial explains the Python, SQL, model evaluation, and astrophysical interpretation step by step.

## Results

The final random forest achieved **test MAE 0.015886**, a 66.2% reduction relative to a median baseline and a 31.3% reduction relative to linear regression.

| Model | MAE | RMSE | Normalized bias | NMAD | Outlier fraction |
|---|---:|---:|---:|---:|---:|
| Median baseline | 0.046965 | 0.059198 | -0.004974 | 0.054866 | 0.000 |
| Linear regression | 0.023117 | 0.032923 | 0.000819 | 0.023877 | 0.003 |
| Random forest | 0.015886 | 0.021930 | 0.000391 | 0.015489 | 0.000 |

These are measured held-out test results, transcribed from the supplied executed notebook; included CSV summaries are rounded to the displayed precision.

## Data and experiment

- 5,000 SDSS DR17 Legacy galaxies; all passed the declared broad cleaning rules.
- Spectroscopic selection: galaxy class, reliable redshift flag, clean photometry, and 0.01 < z < 0.30. See the notebook for the exact SQL.
- The deterministic CHECKSUM ordering selects a pilot subset; it is not a statistically representative random survey sample.
- Inputs: dereddened r magnitude and u−g, g−r, r−i, i−z colors. Target: spectroscopic redshift.
- Random partitions: 3,000 training, 1,000 validation, 1,000 reserved test; seed 42.
- Baseline and standardized linear regression compared with random forests.
- Leaf sizes 1, 5, 10, 20 compared on validation. Five-fold cross-validation inside training compared shortlisted sizes 1 and 5.
- Selected forest: 300 trees, leaf size 1, all features considered at each split. All final models refitted on 4,000 development galaxies before test evaluation.
- Permutation importance evaluated on validation; shuffling g−r caused the largest MAE increase.

## Run in Google Colab

1. Upload `notebooks/galaxy_photoz_complete_tutorial.ipynb` to Colab.
2. Run cells sequentially. Package installation and an SDSS download require internet access. Runtime and service behavior can vary.
3. To reproduce the exact sample, first restore your saved runtime archive into `/content/galaxy-photoz-ml`; otherwise the notebook queries SDSS again. A new download is not guaranteed to reproduce historical metrics exactly.
4. The last export cell is new and has not been executed in this edited copy. Run it to download the complete runtime archive, then download the executed notebook separately.

The package includes the executed tutorial, documentation, dependency snapshot, and rounded summary tables. **The raw/clean catalog, partition CSVs, individual test predictions, and fitted joblib model are not embedded in this package.** They were saved in the user's Colab runtime; retrieve them using the final export cell while the session is available. Displayed notebook tables are not the complete catalog.

## Metrics and limitations

Define dz = z_pred − z_spec and delta = dz / (1 + z_spec). MAE and RMSE use dz. Bias is mean(delta), NMAD is 1.4826 × median(abs(delta − median(delta))), and the outlier fraction uses abs(delta) > 0.15.

The model overpredicts low redshifts and underpredicts high redshifts on average. Test bias is +0.015276 below z = 0.05 and -0.017549 at z = 0.25–0.30. The latter bin contains only 30 galaxies. Small global bias hides cancellation, and the broad outlier threshold can hide important low-redshift errors.

Broad magnitude/error bounds retain unusual and uncertain photometry. The model produces point estimates, not calibrated individual uncertainties. Random holdout performance does not establish transfer to another survey, spatial region, fainter population, or redshift range. Importance measures model reliance rather than physical causation. No claim of a new discovery or survey-wide photo-z catalog is made.

## Files

- `notebooks/galaxy_photoz_complete_tutorial.ipynb`: explanations, original executed code, and outputs; one new unexecuted export cell.
- `results/test_metrics.csv`: rounded final test comparison.
- `results/test_metrics_by_redshift.csv`: rounded final forest diagnostics.
- `requirements.txt`: package versions recorded by the uploaded notebook.
- `PROJECT_CHECKLIST.md`: remaining steps for preserving artifacts and publishing.

## Data attribution and method documentation

Data source: [SDSS DR17](https://www.sdss4.org/dr17/). Follow [SDSS acknowledgments](https://www.sdss4.org/collaboration/acknowledgements/) and applicable data-use instructions when distributing data. The notebook links release documentation, selection definitions, and method documentation.

Software: NumPy, pandas, matplotlib, Astropy, Astroquery, and scikit-learn. See [random forests](https://scikit-learn.org/stable/modules/ensemble.html#random-forests), [cross-validation](https://scikit-learn.org/stable/modules/cross_validation.html), and [permutation importance](https://scikit-learn.org/stable/modules/permutation_importance.html).

## Suggested résumé wording

Built a reproducible SDSS galaxy photometric-redshift regression project in Python, comparing baseline, linear, and random-forest models; achieved held-out test MAE of 0.0159 (66% below a median baseline) and analyzed feature importance and redshift-dependent bias.
