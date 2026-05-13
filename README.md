# first-visit-ed-risk-prediction

This project predicts high-frequency Emergency Department (ED) utilization among depression patients using a hybrid Decision Tree → Random Forest pipeline enriched with Social Determinants of Health (SDOH) features. Developed as part of the 2026 ASA DataFest at UCLA, the project received the Don Ylvisaker Best Insight Winner Award among 78 participating teams and over 370 student participants.

http://datafest.stat.ucla.edu/asa-datafesttm-at-ucla-2/2026-datafest-results/

## Overview

The primary goal was to design a first-visit prediction tool capable of identifying patients at risk of repeated ED utilization at the moment of intake, before longitudinal medical history becomes available.

The final model achieved a calibrated AUC of 0.740 and identified over 52% of high-risk patients while flagging only the top 20% of intake cases.

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

## Pipeline

The pipeline begins with Decision Tree segmentation using intake-time demographic features, where each terminal node’s observed ED utilization rate is transformed into a continuous `dt_risk_score` feature and passed into the Random Forest model. This creates an interpretable bridge between patient segmentation and predictive modeling.

Eleven SDOH variables across five canonical domains were incorporated, and the final model applies Platt-calibrated probabilities with stratified 5-fold cross-validation in a leakage-free first-visit prediction framework.

## Tech Stack

| Category            | Tools / Methods                                        |
| ------------------- | ------------------------------------------------------ |
| Programming         | Python, Jupyter Notebook                               |
| ML Models           | Decision Tree, Random Forest, Gradient Boosting        |
| Visualization       | matplotlib, seaborn                                    |

## Collaboration

Developed collaboratively with Mehek Bajaj, Madeleine Curran, Nina Huang, and Anika Lala.

I primarily led:

* Decision Tree segmentation
* SDOH feature engineering
* Random Forest training
* Probability calibration
* First-visit prediction framework design

## Presentation

<p align="center">
  <img width="1188" height="866" alt="Image" src="https://github.com/user-attachments/assets/5f55f09e-3d69-4324-be87-a2e3e095c739" />
  <img width="1188" height="866" alt="Image" src="https://github.com/user-attachments/assets/adf74a78-1dd6-4d8a-89bc-a93737d540bc" />
  <img width="1188" height="866" alt="Image" src="https://github.com/user-attachments/assets/a6105090-49b8-45a0-8f30-80543fa15d1b" />
</p>
