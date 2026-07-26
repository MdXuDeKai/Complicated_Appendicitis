# Interpretable Machine-Learning Research Prototype for Pediatric Complicated Appendicitis

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-research%20prototype-FF4B4B.svg)](https://comappy-model.streamlit.app/)

This repository provides a locked AdaBoost model and an interactive Streamlit
research prototype for preoperative risk stratification of complicated
appendicitis in children.

The model was developed for children already undergoing surgical evaluation and
appendectomy for clinically suspected acute appendicitis. It is not intended to
diagnose appendicitis in unselected children presenting with abdominal pain.

> **Research-use notice:** This is a retrospective prediction-model research
> prototype, not a clinically validated medical device. Its output is a
> model-derived relative risk score rather than a calibrated absolute
> probability. It must not be used as a stand-alone basis for diagnosis,
> treatment selection, or other clinical decisions. Prospective validation,
> local recalibration, and clinical-impact assessment are required before
> clinical use.

## Study Design

The study used three retrospectively assembled cohorts from two Chinese tertiary
children's hospitals:

| Cohort | Institution and period | Sample size | Role |
| --- | --- | ---: | --- |
| Derivation | Shanxi Children's Hospital, Jan-Dec 2024 | 539 | Split into training and model-selection hold-out sets |
| Training | Derivation-cohort subset | 377 | Model fitting, stratified five-fold cross-validation, hyperparameter tuning, and SHAP feature ranking |
| Internal model-selection hold-out | Derivation-cohort subset | 162 | Algorithm comparison, nested feature-set evaluation, decision-threshold selection, and development-stage performance reporting |
| External validation | Tianjin Children's Hospital, Jan-Mar 2025 | 114 | Independent retrospective validation |
| Temporal validation | Shanxi Children's Hospital, Jan-Mar 2025 | 120 | Independent retrospective validation |

The external and temporal cohorts were withheld from all development-stage
decisions.

The primary outcome was determined from postoperative pathology reports.
Complicated appendicitis was defined by documented gangrenous or perforated
appendicitis, transmural inflammation with periappendiceal extension, or
periappendiceal abscess.

## Model Development

- Twenty-four preoperative variables were initially screened.
- Procalcitonin was excluded before modeling because 82.2% of values were
  missing; the remaining 23 candidate predictors entered the modeling
  pipeline.
- Continuous-variable imputation was fitted on the training set and applied
  without refitting to the hold-out and validation cohorts.
- Eleven candidate algorithms were tuned using stratified five-fold
  cross-validation within the training set.
- AdaBoost had the highest internal hold-out AUC point estimate and was carried
  forward.
- SHAP-based feature ranking was performed in the training set; nested
  feature-set comparison was performed in the internal model-selection
  hold-out set.
- The decision threshold (0.4963) was determined in the hold-out set and fixed
  before application to the external and temporal cohorts.

Because the internal hold-out set informed algorithm, feature-set, and threshold
selection, its estimates are development-stage results and may be optimistic.

## Final Seven Predictors

1. Preoperative C-reactive protein (CRP)
2. Monocyte-to-lymphocyte ratio (MLR)
3. Neutrophil-to-lymphocyte ratio (NLR)
4. Appendiceal diameter
5. Body weight
6. Preoperative platelet count
7. Neutrophil-monocyte-to-lymphocyte ratio (NMLR)

MLR, NLR, and NMLR are calculated from the corresponding preoperative blood-cell
counts entered in the prototype.

## Reported Performance

The following values correspond to the final seven-feature AdaBoost model at
the fixed development-stage threshold:

| Cohort | AUC (95% CI) | Sensitivity | Specificity | Brier score |
| --- | ---: | ---: | ---: | ---: |
| Internal model-selection hold-out | 0.831 (0.759-0.899) | 0.938 | 0.667 | 0.180 |
| External validation | 0.828 (0.741-0.905) | 0.938 | 0.500 | 0.189 |
| Temporal validation | 0.849 (0.766-0.914) | 0.928 | 0.510 | 0.181 |

Discrimination did not show substantial apparent degradation in the available
external and temporal retrospective cohorts. These results do not establish
prospective generalizability or clinical impact. Calibration was compressed in
the validation cohorts, so raw model outputs should not be interpreted as
calibrated absolute probabilities.

## Web Research Prototype

The interactive prototype is available at:

**https://comappy-model.streamlit.app/**

The interface:

- accepts the required preoperative inputs;
- automatically calculates MLR, NLR, and NMLR;
- displays a model-derived relative risk score;
- reports whether the score is above or below the fixed development-stage
  threshold; and
- displays a research-use disclaimer.

It does not provide treatment recommendations.

## Run Locally

```bash
python -m pip install -r web/requirements.txt
streamlit run web/app.py
```

The deployed application expects `final_model.pkl` to be available in the
`web/` directory.

## Repository Files

```text
web/
├── app.py                 # Streamlit research-prototype interface
├── final_model.pkl        # Locked seven-feature AdaBoost model
├── final_features.pkl     # Stored feature list
├── requirements.txt       # Runtime dependencies
└── .streamlit/config.toml # Streamlit configuration
```

No patient-level clinical data are included in this public repository.

## Limitations

- Retrospective study design.
- The internal hold-out set was reused for multiple development decisions.
- External and temporal validation cohorts were modest in size.
- All cohorts were drawn from Chinese tertiary pediatric centers.
- The histopathological endpoint may vary across pathologists and institutions.
- Prospective, international, and clinical-impact evaluations remain necessary.

## Contact

For research questions, please contact:

**Xu DeKai**  
Email: xdk1207@sina.com

---

Last updated: July 2026
