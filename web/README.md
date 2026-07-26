# Complicated Appendicitis Web Research Prototype

This directory contains the Streamlit interface and locked model artifact for a
retrospectively developed seven-feature AdaBoost model for preoperative risk
stratification of complicated appendicitis in children.

> This application is a research prototype, not a clinically validated medical
> device. The displayed model output is a relative risk score rather than a
> calibrated absolute probability. It must not be used for stand-alone
> diagnosis, treatment selection, or other clinical decisions.

## Online Prototype

https://comappy-model.streamlit.app/

## Model Inputs

The final model uses:

1. Preoperative C-reactive protein
2. Monocyte-to-lymphocyte ratio
3. Neutrophil-to-lymphocyte ratio
4. Appendiceal diameter
5. Body weight
6. Preoperative platelet count
7. Neutrophil-monocyte-to-lymphocyte ratio

The interface requests the basic blood-cell counts needed to calculate MLR,
NLR, and NMLR automatically.

## Model Context

- Internal model-selection hold-out AUC: 0.831 (95% CI, 0.759-0.899)
- Development-stage threshold: 0.4963
- External validation AUC: 0.828 (95% CI, 0.741-0.905)
- Temporal validation AUC: 0.849 (95% CI, 0.766-0.914)

The internal hold-out set informed algorithm, feature-set, and threshold
selection. Its performance is therefore a development-stage estimate. The
external and temporal cohorts were not used for these decisions.

## Run Locally

```bash
cd web
python -m pip install -r requirements.txt
streamlit run app.py
```

The following files must remain together:

```text
app.py
final_model.pkl
final_features.pkl
requirements.txt
.streamlit/config.toml
```

## Output Interpretation

The interface reports:

- a model-derived relative risk score;
- an above-threshold or below-threshold classification; and
- the fixed development-stage threshold.

It intentionally does not provide treatment recommendations. Prospective
validation, local recalibration, and clinical-impact assessment are required
before clinical use.
