# 🩺 MedAssist AI — Healthcare Triage & Information Assistant

> AI-powered symptom analysis system that predicts possible medical conditions using a trained machine learning model and generates professional health assessment reports.

[![Python](https://img.shields.io/badge/Python-3.11+-blue.svg)](https://python.org)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-orange.svg)](https://scikit-learn.org)
[![Gradio](https://img.shields.io/badge/Gradio-UI-teal.svg)](https://gradio.app)
[![Gemini](https://img.shields.io/badge/Gemini_API-NLP-green.svg)](https://ai.google.dev)

---

## Overview

MedAssist AI is a healthcare support tool that helps users understand their symptoms before deciding whether to seek medical attention. It combines a trained Random Forest classifier with Google Gemini API to deliver structured, context-aware health information.

The system considers multiple patient factors simultaneously: reported symptoms, age, gender, symptom duration, and existing medical conditions — to provide more relevant and personalized output than a generic symptom checker.

This is an **information assistant**, not a diagnostic tool. Every output includes a clear medical disclaimer.

---

## Features

- **41-disease prediction** using Random Forest classifier trained on 132 symptom features (97.6% test accuracy)
- **Context-aware explanations** powered by Gemini API, factoring in age, gender, duration, and comorbidities
- **Emergency detection** with immediate red-alert warnings for high-risk symptoms (chest pain, breathlessness, unconsciousness)
- **Severity classification** — Low Risk / Moderate Risk / High Risk badges on every assessment
- **Confidence scoring** displayed as a visual bar with percentage
- **Symptom search** — filter 132 symptoms in real time without scrolling
- **Gender-aware form** — Pregnancy field appears only for female patients
- **Professional PDF report** — clinical-format document with patient details, assessment summary, and recommendations

---

## Tech Stack

| Layer | Technology |
|---|---|
| ML Model | Random Forest Classifier (scikit-learn) |
| NLP / Explanation | Google Gemini API |
| UI Framework | Gradio (custom CSS) |
| PDF Generation | ReportLab |
| Language | Python 3.11+ |
| Environment | Google Colab |

---

## How It Works

```
User Input
(Name, Age, Gender, Duration, Existing Conditions, Symptoms)
        ↓
Symptom vector (132 binary features)
        ↓
Random Forest Classifier
        ↓
Predicted Disease + Confidence Score + Severity Badge
        ↓
Gemini API → Context-aware explanation
        ↓
Gradio UI → Result card with emergency detection
        ↓
PDF Report → Professional clinical document (downloadable)
```

---

## Model Details

| Property | Value |
|---|---|
| Algorithm | Random Forest Classifier |
| Training samples | 4,920 (120 per class, perfectly balanced) |
| Disease classes | 41 |
| Symptom features | 132 binary indicators |
| Cross-validation accuracy | 100% (5-fold CV) |
| Test set accuracy | 97.6% (42 held-out samples) |
| Label encoding | scikit-learn LabelEncoder |

High CV accuracy is expected given the structured, deterministic nature of the symptom-disease dataset (each disease has a fixed symptom pattern with no noise). Real-world performance would vary with noisy, incomplete inputs.

---

## Dataset

**Disease Prediction Using Machine Learning** (Kaggle — kaushil268)

- 41 diseases, 132 symptom features
- Training set: 4,920 rows
- Test set: 42 rows
- All features binary (0 = symptom absent, 1 = symptom present)
- Target column: `prognosis` (disease name as string)

---

## Development Process

### Step 1 — Data Loading and Exploration
Loaded `Training.csv` and `Testing.csv`. Identified and removed an artifact column (`Unnamed: 133`) introduced by CSV formatting. Verified class balance (exactly 120 samples per disease class).

### Step 2 — Preprocessing
- Dropped artifact column
- Separated features (132 symptom columns) from target (`prognosis`)
- Applied `LabelEncoder` to convert string disease names to integer class labels

### Step 3 — Model Training
- Trained `RandomForestClassifier` with `n_estimators=200`, `random_state=42`
- Ran 5-fold cross-validation to confirm generalization
- Fit final model on full training set
- Evaluated on held-out test set: **97.6% accuracy**

### Step 4 — Gemini API Integration
- Integrated Google Gemini API for natural language explanation generation
- Prompt engineered to produce context-aware, cautious, under-100-word responses
- Patient context (age, gender, duration, comorbidities) injected into each prompt

### Step 5 — Emergency Detection Logic
- Defined a list of red-flag symptoms (chest pain, breathlessness, coma, bleeding, unconsciousness)
- Rule-based check runs before displaying any result
- Emergency symptoms trigger an immediate high-visibility alert regardless of model confidence

### Step 6 — Severity Classification
- Confidence ≥ 75% and no emergency symptoms → 🟢 Low Risk
- Confidence ≥ 50% → 🟡 Moderate Risk
- Confidence < 50% → 🟡 Uncertain — Monitor
- Any emergency symptom present → 🔴 High Risk

### Step 7 — UI Design (Gradio + Custom CSS)
- Built with `gr.Blocks` for full layout control
- Custom medical teal palette (`#0F766E`) with glassmorphism-style result card
- Google Fonts (Lexend + Source Sans 3) loaded via CSS import
- Symptom search box filters 132 checkboxes in real time
- Gender-aware conditions field (Pregnancy option shown only for Female)
- Age slider, symptom duration dropdown, existing conditions checklist

### Step 8 — PDF Report Generation
- Built using ReportLab `SimpleDocTemplate`
- Clinical layout: header, patient details table, symptoms section, assessment table, explanation, disclaimer footer
- Filename auto-generated with patient name and timestamp
- Color scheme matches app (teal accents, clean white background)

---

## Project Structure

```
medassist-ai-triage/
│
├── medassist_chatbot.py       # Complete source code (all cells combined)
├── README.md                  # This file
```

---

## Safety & Disclaimers

MedAssist AI is an **information tool only**. It does not constitute medical advice, replace clinical diagnosis, or recommend specific treatments. All outputs include a mandatory disclaimer. Emergency symptom detection logic is rule-based and independent of model confidence to ensure critical warnings are never suppressed by an uncertain prediction.

---

## Author

**Aila Nasir** — AI/ML Engineer & Data Scientist

**GitHub** — [github.com/ailanasirai](https://github.com/ailanasirai)
**LinkedIn** — [linkedin.com/in/aila-nasir](https://www.linkedin.com/in/aila-nasir/)
**Kaggle** — [kaggle.com/ailanasirai](https://www.kaggle.com/ailanasirai)

---

*Actively maintained. Features and documentation updated as development progresses.*
