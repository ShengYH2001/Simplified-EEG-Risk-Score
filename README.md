# Simplified EEG Risk Score (SERS) Prediction Calculator

![Platform](https://img.shields.io/badge/platform-Windows-blue)
![Status](https://img.shields.io/badge/status-research%20use-orange)
![License](https://img.shields.io/badge/license-MIT-green)

A bedside decision-support tool for **prognostic assessment of comatose patients after cardiac arrest (CA)**, built on the **Simplified EEG Risk Score (SERS)** — a multi-feature, time-window-specific EEG scoring system derived from a retrospective cohort of 251 post-CA patients (344 EEG recordings) at the First Affiliated Hospital of Chongqing Medical University.

The calculator integrates five key EEG features with routine clinical indicators (initial rhythm, pupillary reflex, APACHE II score, NSE) to estimate the risk of poor neurological outcome at **three post-CA time windows: Day 1, Day 2–5, and > Day 5**.

---

## Why SERS?

- **~40% of comatose post-CA patients fall into a "prognostic grey zone"** — single EEG features (e.g., background continuity or reactivity alone) show false-positive rates as high as 37.6–58.3%.
- EEG pathology evolves over time: the predictive value of individual features shifts across the post-arrest course, so a single-time-point assessment is unreliable.
- **SERS integrates background amplitude, dominant frequency, continuity, reactivity, and sleep waveforms**, with scoring weights derived from time-window-specific logistic regression coefficients — combining the biological interpretability of EEG with the stability of a multivariable model.

## Features

- **Time-window-specific scoring** — separate, validated SERS weights for Day 1, Day 2–5, and > Day 5 after cardiac arrest
- **Two-step workflow** — compute the SERS from EEG features, then combine it with clinical variables for the final risk estimate
- **Risk stratification** — outputs a risk category (low / medium / high) together with an estimated probability of poor outcome
- **Standalone Windows executable** — no Python installation or programming required; runs entirely offline, no patient data leaves your machine
- **Open for external validation** — we invite researchers and clinicians to test the SERS on independent datasets

## The SERS Scoring System

Each EEG feature is scored 0 (favorable) or a time-window-specific weight (unfavorable). The SERS is the sum of the five item scores. EEG features are defined according to the **ACNS Standardized Critical Care EEG Terminology (2021)**.

### Day 1 (total range 0–16.5)

| EEG feature | 0 points | Points if abnormal |
|---|---|---|
| Background amplitude | Normal | **4** (Abnormal) |
| Background dominant frequency | Fast band (α/β) | **2** (Slow band θ/δ or no discernible activity) |
| Background continuity | Continuous | **4** (Discontinuous) |
| Reactivity | Present | **4** (Absent) |
| Sleep waveforms | Present | **2.5** (Absent) |

### Day 2–5 (total range 0–14)

| EEG feature | 0 points | Points if abnormal |
|---|---|---|
| Background amplitude | Normal | **4** (Abnormal) |
| Background dominant frequency | Fast band (α/β) | **2.5** (Slow band θ/δ or no discernible activity) |
| Background continuity | Continuous | **3** (Discontinuous) |
| Reactivity | Present | **3** (Absent) |
| Sleep waveforms | Present | **1.5** (Absent) |

### > Day 5 (total range 0–17)

| EEG feature | 0 points | Points if abnormal |
|---|---|---|
| Background amplitude | Normal | **4** (Abnormal) |
| Background dominant frequency | Fast band (α/β) | **2.5** (Slow band θ/δ or no discernible activity) |
| Background continuity | Continuous | **4** (Discontinuous) |
| Reactivity | Present | **4** (Absent) |
| Sleep waveforms | Present | **2.5** (Absent) |

### Risk stratification by SERS

| Time window | Low risk | Medium risk | High risk |
|---|---|---|---|
| Day 1 | 0 (poor outcome 33.3%) | 2–8.5 (59.5%) | 10–16.5 (100%) |
| Day 2–5 | 0 (20.0%) | 1.5–7 (67.4%) | 7.5–14 (97.3%) |
| > Day 5 | 0 (11.1%) | 2.5–6.5 (66.0%) | 8–17 (100%) |

## Installation

1. Download [`main.exe`](main.exe) from this repository (click the file → **Download**), or clone the repo:
   ```bash
   git clone https://github.com/ShengYH2001/Simplified-EEG-Risk-Score.git
   ```
2. Double-click `main.exe` — no installation, no dependencies, no internet connection required.

> If Windows SmartScreen prompts a warning for the unsigned executable, click **More info → Run anyway**.

## Usage

### Step 1 — Select the post-CA time window

Open the calculator and choose the time window of the EEG recording (**Day 1**, **Day 2–5**, or **> Day 5**).

<img width="372" height="262" alt="step1_select_time_window" src="https://github.com/user-attachments/assets/15dd0fef-1103-4609-a3c1-111a57aeab85" />

### Step 2 — Calculate the SERS

Click **Calculate** to open the EEG scoring panel. For each of the five EEG features, select the patient's finding from the drop-down menu, then click **Calculate EEG score**. The SERS is filled in automatically.

<img width="367" height="262" alt="step2_calculate_eeg_score" src="https://github.com/user-attachments/assets/f2ec0fbc-5582-4e39-a293-8049b4cf721a" />

### Step 3 — Enter clinical indicators and obtain the risk estimate

Complete the remaining fields:

| Field | Input |
|---|---|
| Ventricular fibrillation | Whether the initial rhythm was ventricular fibrillation (shockable rhythm) — Yes / No |
| Pupillary reflex | Whether the pupillary light reflex was absent — Yes / No |
| APACHE II score | Acute Physiology and Chronic Health Evaluation II score (within 72 h after CA) |
| Simplified EEG risk score | Auto-filled from Step 2 (editable) |
| NSE | Neuron-specific enolase, ng/mL (within 72 h after CA) |

Click **Calculate risk**. The tool outputs the **risk category** (e.g., *High risk of mortality*) and the **estimated probability** of poor outcome.

<img width="665" height="663" alt="step3_view_result" src="https://github.com/user-attachments/assets/01f8fc48-c247-4867-8e22-bc294fdb76e8" />

## Model Performance

Internal validation with leave-one-out cross-validation (combined model: SERS + initial rhythm + pupillary reflex + APACHE II + NSE):

| Time window | AUC (95% CI) | Accuracy | Sensitivity | Specificity |
|---|---|---|---|---|
| Day 1 (n = 119) | **0.965** (0.934–0.995) | 89.08% | 88.00% | 94.74% |
| Day 2–5 (n = 127) | **0.909** (0.855–0.963) | 80.32% | 78.10% | 90.91% |
| > Day 5 (n = 98) | **0.903** (0.842–0.963) | 81.63% | 78.67% | 91.30% |

The SERS alone achieved AUCs of **0.912 / 0.893 / 0.881** at Day 1 / Day 2–5 / > Day 5, significantly outperforming individual EEG features, Synek grading, and Young grading within each time window (all *P* < 0.05, with significant IDI and NRI).

## External Validation

This repository also serves as an **open validation platform** for the SERS. If you use the calculator on your own cohort, we warmly invite you to share your validation results via [Issues](../../issues) or by contacting the corresponding author — multi-center external validation is essential for establishing the generalizability of the score.

## Citation

If you use this tool or the SERS in your research, please cite:

> Wang Y, Sheng Y, Li F. *Prognostic Assessment of Comatose Patients After Cardiac Arrest: Construction of a Simplified Monitoring EEG Risk Score Across Different Time Windows.* (Manuscript; preprint available). Department of Neurology & Department of Critical Care Medicine, The First Affiliated Hospital of Chongqing Medical University, Chongqing, China.

## Authors and Contact

- **Yan Wang, MD** — Department of Neurology & Nursing Department
- **Yuanhui Sheng, MD** — Department of Critical Care Medicine & Department of Emergency
- **Feng Li, MD** (corresponding author) — 📧 204267@hospital.cqmu.edu.cn

The First Affiliated Hospital of Chongqing Medical University, Chongqing 400016, China

## Funding

- Chongqing Science and Health Joint Medical Research Project (Grant No. 2026MSXM082)
- 2024 Nursing Research and Innovation Project of the First Affiliated Hospital of Chongqing Medical University (Grant No. HLYB2024-03)

## Disclaimer

This software is intended for **research purposes only**. It does not replace clinical judgment and must not be used as the sole basis for treatment decisions or decisions to withdraw life-sustaining therapy. Prognostication after cardiac arrest should always follow current international guidelines and integrate multimodal assessment by qualified clinicians.

## License

This project is released under the [MIT License](LICENSE).
