# first-visit-ed-risk-prediction

## Overview

This project predicts high-frequency Emergency Department (ED) utilization among depression patients using a hybrid Decision Tree → Random Forest pipeline enriched with Social Determinants of Health (SDOH) features. The primary goal was to design a first-visit prediction tool capable of identifying patients at risk of repeated ED utilization at the moment of intake, before longitudinal medical history becomes available.

The final model achieved a calibrated AUC of 0.740 and identified over 52% of high-risk patients while flagging only the top 20% of intake cases.

Developed as part of the 2026 ASA DataFest at UCLA, the project received the Don Ylvisaker Best Insight Award among 78 participating teams and over 370 student participants.

http://datafest.stat.ucla.edu/asa-datafesttm-at-ucla-2/2026-datafest-results/

## Data Availability

The original dataset used for this project was provided through the ASA DataFest competition and is proprietary.  
In accordance with DataFest participation policies, this repository contains only:
- brief project descriptions
- the final products of the competition

## Motivation

Depression patients frequently rely on the Emergency Department as a substitute for longitudinal care. Many enter a repeated cycle of:

1. Crisis visit
2. Stabilization and discharge
3. Lack of outpatient follow-up
4. Return ED visit

This project aims to interrupt that cycle early by enabling proactive intervention during initial registration.

### Key Constraints

* SDOH screening coverage available for only ~25% of patients
* Severe class imbalance
* Strict first-visit-only feature restriction to prevent leakage

---

## Pipeline Architecture

### Step 1 — Decision Tree Segmentation

A shallow Decision Tree (`max_depth=4`) segments patients using intake-time demographics:

* age
* sex
* race
* marital status
* county

Each terminal node's observed ED utilization rate becomes a continuous feature:

```python
dt_risk_score
```

This creates an interpretable bridge between segmentation and downstream modeling.

### Step 2 — SDOH Feature Integration

Eleven SDOH variables across five canonical domains were evaluated:

| Domain           | Example Features              |
| ---------------- | ----------------------------- |
| Economic         | Financial hardship            |
| Food             | Food insecurity               |
| Transportation   | Transportation barriers       |
| Social           | Social isolation              |
| Health Behaviors | Exercise, alcohol use, stress |

Transportation barriers emerged as the strongest modifiable predictor.

Adding SDOH features improved:

```txt
AUC: 0.695 → 0.740
```

### Step 3 — Random Forest Prediction

Final model:

* Random Forest (`n_estimators=300`, `max_depth=8`)
* Platt probability calibration
* Stratified 5-fold cross-validation

---

## Model Performance

| Model                               | Features               | AUC               |
| ----------------------------------- | ---------------------- | ----------------- |
| Random Forest + Calibration (Final) | Baseline + DT + SDOH   | **0.740 ± 0.015** |
| Gradient Boosting                   | Baseline + DT + SDOH   | 0.733 ± 0.010     |
| Random Forest                       | Baseline + DT only     | 0.703 ± 0.016     |
| Demographics Only                   | Registration data only | 0.695 ± 0.019     |

### Risk Stratification

| Risk Tier     | Actual ED Rate |
| ------------- | -------------- |
| Low Risk      | 2.8%           |
| Moderate Risk | 10.3%          |
| High Risk     | 22.7%          |

This produced an approximately **8× separation** between low- and high-risk groups.

Operating at the top 20% intervention threshold:

* **Precision:** 20.6%
* **Recall:** 52.3%

---

## Key Contributions

* Hybrid **Decision Tree → Random Forest** pipeline
* Leakage-free first-visit prediction framework
* Structured SDOH integration
* Probability-calibrated risk scoring
* Operationally actionable intervention targeting

---

## Proposed Intervention Strategy

```txt
Flag top 20% of intake patients
        ↓
Assign care navigator during visit
        ↓
Schedule outpatient follow-up within 7 days
```

Expanding SDOH screening coverage from 25% → 100% would substantially improve deployment reach.

---

## Tech Stack

| Category            | Tools / Methods                                        |
| ------------------- | ------------------------------------------------------ |
| Programming         | Python, Jupyter Notebook                               |
| ML Models           | Decision Tree, Random Forest, Gradient Boosting        |
| Visualization       | matplotlib, seaborn                                    |

---

## Collaboration

Developed collaboratively with Mehek Bajaj, Madeleine Curran, Nina Huang, and Anika Lala.

I primarily led:

* Decision Tree segmentation
* SDOH feature engineering
* Random Forest training
* Probability calibration
* First-visit prediction framework design

---

## Presentation

<p align="center">
  <img width="1188" height="866" alt="Image" src="https://github.com/user-attachments/assets/5f55f09e-3d69-4324-be87-a2e3e095c739" />
  <img width="1188" height="866" alt="Image" src="https://github.com/user-attachments/assets/adf74a78-1dd6-4d8a-89bc-a93737d540bc" />
  <img width="1188" height="866" alt="Image" src="https://github.com/user-attachments/assets/a6105090-49b8-45a0-8f30-80543fa15d1b" />
</p>

---

## Limitations

* SDOH coverage currently limited to ~25% of patients
* Risk tier thresholds are distribution-based, not clinically validated
* Tier validation performed on training distribution
* External validation remains future work

---

## Conclusion

This project demonstrates that meaningful ED risk prediction is possible using only information available at first intake.

The results further suggest that structured SDOH screening — particularly transportation access — may play a major role in preventing repeated emergency care utilization among depression patients.

---
