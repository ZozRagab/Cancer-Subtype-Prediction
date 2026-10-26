# Breast Cancer Subtype Prediction with Explainable AI

This repository contains the project specification, risk plan, literature review, project schedule, and the initial GSE96058 data inspection and exploratory data analysis (EDA) notebook for a breast cancer molecular subtype prediction project.

## Project scope

The planned system will predict the five PAM50 subtype labels represented in the GSE96058 breast cancer cohort: Luminal A, Luminal B, HER2-enriched, Basal-like, and Normal-like. The intended project is a research-use software prototype for compatible processed gene-expression profiles. Planned later work includes leakage-aware model development and evaluation, SHAP explanations, and a Streamlit interface.

This is a work in progress. The current notebook prepares and explores data; it does **not** train or evaluate a predictive model. There are no model results in this repository. This project is not a clinical diagnostic tool and does not establish clinical validity or performance on other cohorts.

## Repository contents

```text
.
├── .gitignore
├── README.md
├── docs/
│   ├── Breast_Cancer_XAI_Project_Specification_and_Risk_Plan.pdf
│   ├── Defense_I_Literature_Review.pdf
│   └── Defense_I_Project_Plan_Gantt.xlsx
└── notebooks/
    └── GSE96058_Dataset_Inspection_Defense_I_Submission_Final.ipynb
```

The project plan records planned dates and task statuses; those entries are schedule information, not evidence that planned modeling or application work has been completed. Refer to the workbook for the current task-level plan.

## Dataset and preparation

The notebook downloads the official GSE96058 supplementary expression matrix and GEO SOFT sample metadata from [NCBI GEO GSE96058](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE96058). It matches expression sample identifiers to GEO sample titles, reads the PAM50 subtype characteristics, and excludes 136 technical replicate columns whose identifiers end in `repl`. The notebook records 3,273 primary samples and 30,865 gene-expression features.

The source expression values are already processed and reported by the notebook as `log2(FPKM + 0.1)`. The notebook retains that scale without applying a second log transform. It creates a sample-by-gene table with PAM50 labels and exports cleaned expression and label CSV files under `/content/data/processed/` when run in its intended Google Colab environment. These generated data files are not included in this repository.

The notebook reports five-class label counts of Luminal A 1,657; Luminal B 729; Basal-like 339; HER2-enriched 327; and Normal-like 221. It also records 49/50 exact PAM50 gene-symbol matches, and 50/50 after resolving the historical `ORC6L` symbol to `ORC6`. These are notebook data-inspection outputs, not classifier metrics.

## Notebook use

Open `notebooks/GSE96058_Dataset_Inspection_Defense_I_Submission_Final.ipynb` in Google Colab and run its cells from top to bottom. The notebook downloads the GEO files if they are not already present in the Colab session and writes generated CSV outputs to `/content/data/processed/`.

To run it locally, use Jupyter with Python 3, install the dependencies below, and adjust the notebook's `/content` paths to a writable local data directory. The raw GEO files and generated CSVs can be large; keep them outside Git and do not commit them.

### Dependencies

The notebook uses:

- Python 3
- Jupyter Notebook or Google Colab
- `pandas`
- `numpy`
- `matplotlib`

For a local environment, install the Python packages with:

```bash
python -m pip install pandas numpy matplotlib
```

## Current status

- Project specification and risk plan: prepared.
- Literature review: prepared for Defense I.
- GSE96058 data inspection and EDA notebook: provided with saved inspection outputs; independent read-back verification of its generated CSV exports remains outstanding according to the notebook checklist.
- Dataset label and replicate audit: marked in progress / verify in the project plan.
- Expression-scale, identifier-alias, and EDA verification: tracked in the project plan.
- Predictive models, held-out evaluation, SHAP analysis, and application: planned future work; no trained-model results are supplied here.

## Scientific limitations

- The target labels are GEO-provided PAM50 annotations. Predicting them would measure agreement with those reference labels, not prove diagnostic superiority or clinical utility.
- GSE96058 is the selected development cohort. Results from this cohort alone would not establish generalization to other datasets, laboratories, or clinical settings.
- The five PAM50 classes are imbalanced. Normal-like requires careful interpretation and review; it should not be treated as an independent clinical control group based on this notebook.
- Expression preprocessing and identifier matching affect the features available for modeling. The expression representation, sample alignment, replicate handling, and gene-symbol alias resolution require validation before model development.
- The notebook's EDA is descriptive. It does not perform feature selection, train/test splitting, model fitting, evaluation, causal inference, or clinical validation.
- Future explanations such as SHAP would describe model behavior and associations. They would not establish biological causation.

## Data and contribution policy

Do not commit raw or processed expression matrices, generated CSV exports, model files, credentials, or other private data. The `.gitignore` covers common data, model, cache, secret, and temporary files. Keep only project documentation, notebooks, and source code needed to reproduce the work.
