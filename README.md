# 🩺 DiaLLM-BD — Diabetes Risk Prediction & Healthcare Assistant

### An Explainable AI-Powered Healthcare System for Type-2 Diabetes Risk Prediction and Patient Education in Bangladesh

**DiaLLM-BD** is an AI-powered healthcare prototype designed to support Type-2 diabetes risk prediction and patient education in Bangladesh. It integrates Machine Learning (ML), Explainable Artificial Intelligence (XAI), SHAP-based interpretability, and an interactive LLM-inspired educational chatbot into a Streamlit application.

Using the DiaHealth dataset, the system provides transparent prediction insights, evidence-grounded diabetes information, and access to nearby healthcare facilities. Its primary goal is to support diabetes awareness and informed decision-making through accessible AI-driven healthcare assistance.

---

## ✨ Key Features

- 🧠 **Diabetes Risk Prediction:** Predicts Type-2 diabetes risk using trained machine learning models.
- 📊 **Model Comparison:** Evaluates multiple ML algorithms and generates performance comparison results.
- 🔍 **Explainable AI (XAI):** Uses SHAP to explain individual predictions through feature contributions and waterfall visualizations.
- 📄 **Report Generation:** Exports patient-level prediction explanations in PDF, DOCX, PNG, and JPEG formats.
- 💬 **Healthcare Chatbot:** Provides diabetes-related educational information grounded in WHO, IDF, and ADA sources.
- 🤖 **Local LLM Integration:** Supports Ollama for locally generated chatbot responses without requiring a paid API key.
- 🏥 **Nearby Doctors & Hospitals:** Discovers nearby healthcare facilities using OpenStreetMap.
- 🇧🇩 **Bangladesh-Focused Approach:** Uses the DiaHealth dataset for diabetes risk prediction and evaluation.

---

## 🔧 Major Improvements in This Prototype

This version introduces several important corrections and enhancements to the original prototype.

### 1. Target Leakage Correction

A target-column detection issue in `train_model.py` was identified and corrected.

Previously, the system automatically selected `family_diabetes` (family history of diabetes) instead of the actual outcome column, `diabetic`. This allowed the actual diabetes label to remain among the input features, resulting in misleading validation accuracy of approximately 99%.

**Implemented correction:**
- Explicitly configured `TARGET_COLUMN_OVERRIDE = "diabetic"`.
- Corrected the target selection in the training pipeline.
- Removed the target leakage issue.

> ⚠️ **Important:** Existing `models/*.pkl` and `outputs/*.csv` files are outdated. Retraining is required to generate updated models and evaluation results.

### 2. Recall-Focused XGBoost Deployment

The deployment model is now fixed to XGBoost using a recall-first screening strategy.

Model deployment is independent of whichever algorithm achieves the highest validation F1-score or accuracy. This reflects the research motivation that low recall for the diabetic class can be problematic in screening applications.

The complete model comparison table is still generated for research and evaluation.

### 3. Patient-Level SHAP Explanations

The Prediction Explanation tab now provides:

- Individual feature contributions.
- Factors that increase or decrease predicted risk.
- SHAP waterfall visualizations.
- Exportable patient-level prediction reports.

### 4. Retrieval-Grounded Healthcare Chatbot

The chatbot retrieves relevant diabetes information from a knowledge base containing sourced WHO, IDF, and ADA factsheets.

With Ollama installed, it can generate responses using a locally running LLM and retrieved contextual information.

The `remote_llm_generate` integration point in `rag_chatbot.py` also provides an extension point for future Gemini or OpenAI integration.

### 5. Nearby Doctors & Hospitals

A healthcare discovery feature provides OpenStreetMap-based lookup of nearby healthcare facilities.

Users can search through:
- Manually entered locations.
- Optional browser geolocation using an additional package.

---

## 🖥️ System Requirements

- Windows 10 or Windows 11
- Python 3.10 or Python 3.11
- Visual Studio Code (recommended)
- Internet connection for relevant retrieval and healthcare facility lookup features

---

## 🚀 Installation & Setup

### Step 1: Install Python

Download Python 3.10 or 3.11:

https://www.python.org/downloads/

During installation, enable **Add Python to PATH**.

### Step 2: Clone the Repository

```bash
git clone https://github.com/Arnob-HRes/DiaLLM-BD-App-Diabetes-Assistant.git
```

Navigate to the project directory:

```bash
cd DiaLLM-BD-App-Diabetes-Assistant
```

### Step 3: Open in Visual Studio Code

1. Install Visual Studio Code: https://code.visualstudio.com/
2. Install the Python extension.
3. Open the project folder using `File > Open Folder`.
4. Open the integrated terminal using `Ctrl + ``.

### Step 4: Create a Virtual Environment

```bash
python -m venv venv
```

Activate the virtual environment:

```bash
venv\Scripts\activate
```

### Step 5: Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 6: Retrain the Machine Learning Models

**Retraining is required for this corrected prototype.**

```bash
python train_model.py
```

The training script generates:

- Model validation comparison results.
- Final test evaluation metrics.
- Trained model files under `models/`.
- Evaluation outputs under `outputs/`.

Save the generated results and update the corresponding experimental results in the research paper.

### Step 7: Run the Application

```bash
streamlit run app.py
```

The application will open in your default browser.

**Local URL:** http://localhost:8501

---

## 🤖 Optional: Local LLM Chatbot with Ollama

DiaLLM-BD supports locally running LLMs through Ollama.

### Install Ollama

Download the Windows installer:

https://ollama.com/download

### Download the Model

Open a terminal and execute:

```bash
ollama pull llama3.1
```

This downloads the model (approximately 4.7 GB).

Keep Ollama running in the background. The chatbot automatically detects the local LLM and uses retrieved knowledge to generate responses.

If Ollama is unavailable, the chatbot falls back to its retrieval-template response mechanism.

**Benefits:**
- No paid API key required.
- Local LLM inference.
- Retrieval-grounded responses.
- Reduced dependence on external LLM services.

---

## 📍 Optional: Browser Geolocation

To enable one-click browser geolocation, install:

```bash
pip install streamlit-js-eval
```

Without this package, users can still search for nearby healthcare facilities using manually entered locations.

---

## 📂 Project Structure

```text
DiaLLM-BD-App-Diabetes-Assistant/
│
├── app.py
├── train_model.py
├── rag_chatbot.py
├── nearby_care.py
├── export_report.py
├── knowledge_base.json
├── requirements.txt
│
├── data/
│   └── diabetes_bangladesh.csv
│
├── models/
│   └── Generated model files
│
├── outputs/
│   └── Generated evaluation results
│
└── README.md
```

### Core Components

| File | Description |
|---|---|
| `app.py` | Main Streamlit application and interactive interface. |
| `train_model.py` | Machine learning training, evaluation, and model generation. |
| `rag_chatbot.py` | Retrieval-grounded chatbot and optional Ollama integration. |
| `nearby_care.py` | OpenStreetMap geocoding and healthcare facility search. |
| `export_report.py` | PDF, DOCX, PNG, and JPEG report generation. |
| `knowledge_base.json` | Sourced diabetes-related knowledge used by the chatbot. |
| `data/diabetes_bangladesh.csv` | Dataset used for model training and evaluation. |
| `models/` | Stores generated machine learning models. |
| `outputs/` | Stores generated evaluation results. |

---

## 🧪 Research & Technologies

The project integrates several technologies across artificial intelligence, healthcare, and data science.

| Technology | Application |
|---|---|
| Machine Learning | Diabetes risk prediction and model evaluation. |
| Explainable AI (XAI) | Understanding model predictions. |
| SHAP | Patient-level feature contribution analysis. |
| Retrieval-Augmented Generation (RAG) | Grounded healthcare information retrieval. |
| Large Language Models (LLMs) | Locally generated chatbot responses. |
| Streamlit | Interactive web application. |
| Ollama | Local LLM execution. |
| OpenStreetMap | Healthcare facility discovery. |
| Python | Core development and implementation. |

---

## ⚠️ Disclaimer

DiaLLM-BD is an academic and research-oriented prototype developed for diabetes risk screening research and patient education.

- Predictions should not be interpreted as confirmed medical diagnoses.
- The chatbot provides educational information, not professional medical advice.
- Model performance must be independently validated before any real-world clinical application.
- Healthcare facility information may be incomplete or outdated.
- Users should consult qualified healthcare professionals for medical decisions.

---

## 👨‍💻 Project Information

**Project Name:** DiaLLM-BD  
**Repository:** [DiaLLM-BD-App-Diabetes-Assistant](https://github.com/Arnob-HRes/DiaLLM-BD-App-Diabetes-Assistant)  
**GitHub Username:** Arnob-HRes  
**Domain:** Artificial Intelligence | Machine Learning | Healthcare | Explainable AI  
**Framework:** Streamlit  
**Primary Application:** Type-2 Diabetes Risk Prediction and Patient Education in Bangladesh

---

⭐ If you find this project interesting, consider giving the repository a star!
