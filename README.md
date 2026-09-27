# Lab 4.1: Explaining Phishing Detectors

AI for Cybersecurity (D7084E / D7041E), Group 6: Kirill Silchenko (kirsil-5@student.ltu.se) and
Stefanos Ntentopoulos (stente-5@student.ltu.se).

We train phishing-URL detectors in three ways (supervised, semi-supervised with 5% labels,
self-supervised with 5% labels), explain them with glass boxes, SHAP and LIME, test the detector without
its external lookups and the stability of LIME, and turn the two most important features into a
Belief Rule-Based expert system (BRB).

- **Notebook to hand in:** `lab4_1_explaining_phishing_detectors.ipynb`. It runs from top to bottom
  and holds lab steps A0–D5 in order, plus five optional extra checks at the end.
- **Report:** `report/Lab4_1_Report.pdf` and `report/Lab4_1_Report.docx` (Word, with our title page),
  both built from `report/Lab4_1_Report.md`.

## Dataset

Web Page Phishing Detection, version 3 (Hannousse & Yahiouche, 2021), Mendeley Data,
https://doi.org/10.17632/c2gw7fy2j4.3, licence CC BY 4.0. The file is committed as
`data/dataset_phishing.csv` (renamed from `dataset_B_05_2020.csv`, content unchanged): 11,430 URLs, half
phishing, 87 features and the label `status`. `data/README.md` has the SHA-256 hash and the download
command.

## Setup

```bash
uv venv --python 3.13 .venv
source .venv/bin/activate
uv pip install -r requirements.txt
```

Python 3.13 (3.11–3.13 should work). Pinned versions: scikit-learn 1.6.1, shap 0.52.0, lime 0.2.0.1,
numpy 2.5.3, pandas 3.0.6, scipy 1.18.1, matplotlib 3.11.2, plus Jupyter, nbformat and nbconvert.
fpdf2 and markdown are only needed to build the report PDF.

**Google Colab:** upload `lab4_1_explaining_phishing_detectors.ipynb` and `dataset_phishing.csv` (with
the folder icon) and run all cells. The first cell installs shap and lime if they are missing, and the
notebook uses `dataset_phishing.csv` next to it when `data/` does not exist.

## How to run

| What | Command | Time |
|---|---|---|
| Run the hand-in notebook, top to bottom | `jupyter nbconvert --to notebook --execute --inplace lab4_1_explaining_phishing_detectors.ipynb` (or *Run All* in Jupyter) | about 10 min |
| Rebuild the hand-in notebook from the part notebooks, and run it | `python tools/assemble.py --strict` | about 10 min |
| Build the report PDF from the latest results | `python report/build_report.py` | a few seconds |
| Build the Word version of the report | `python report/build_docx.py` | a few seconds |

Run the commands from the repository root. The self-supervised SHAP explanation is the slowest part (a
permutation explainer, about 5 minutes).

## How the code is organised

We worked in separate part notebooks, so that two people never edit the same file. Each cell starts
with the lab step it belongs to (`# STEP A4`). Cells that only stand in for another notebook's step while
developing are marked `# STANDIN`. `tools/assemble.py` collects the `STEP` cells from all part
notebooks, puts them in lab order, drops the stand-ins, and writes and runs the hand-in notebook.

| Path | Contents |
|---|---|
| `parts/00_setup.ipynb` | A0–A3: install check, data loading, 60/20/20 stratified split, `report()` |
| `parts/10_supervised_forest.ipynb` | A4–A6 (tree, logistic regression, forest), B1–B2, B5–B6 (glass boxes, SHAP, LIME), C1–C2 (ablation, LIME stability), extra LIME-stability check on all three URLs |
| `parts/20_label_scarce.ipynb` | A7–A10 (5% labels, pseudo-labelling, self-supervised, the 7-model table), B3–B4 (SHAP for these models, top-10 comparison), extra CUTOFF comparison |
| `parts/30_brb.ipynb` | D1–D5 (BRB), extra checks: a hand-picked feature pair, the feature choice on validation data, and the ER code against our Lab 3 implementation |
| `tools/assemble.py` | Builds and runs the hand-in notebook (`--no-execute`: build only; `--strict`: fail if a lab step is missing) |
| `results/tables/`, `results/figures/` | Every table and figure the notebook writes; the file name starts with the lab step |
| `report/` | Report source (`Lab4_1_Report.md`), PDF and Word versions, their build scripts, the title-page template, bundled fonts, code images for the appendix |
| `data/` | The dataset and its source, licence and hash |

## Headline results (test set, 2,286 URLs)

| Model | Macro-F1 | Recall | FAR |
|---|---|---|---|
| Random Forest, all labels | 0.965 | 0.968 | 0.038 |
| Random Forest, 5% labels | 0.941 | 0.942 | 0.061 |
| Semi-supervised (pseudo-labels) | 0.934 | 0.940 | 0.071 |
| Self-supervised + logistic regression, 5% labels | 0.918 | 0.922 | 0.087 |
| BRB on `page_rank` + `nb_hyperlinks` | 0.802 | 0.773 | 0.169 |

The unlabelled data did not help on this dataset. The full table is in
`results/tables/A10_test_scores.csv`; the discussion is in the report.

## Reproducibility

`random_state=42` everywhere. The SHAP permutation explainer for the self-supervised model gets
`seed=42`; without it, its ranking changes between runs. LIME uses fixed seeds (42, and 0–4 for the
stability test). Running the notebook twice gives identical result tables. The small MLP in step A9 can
differ in the last decimals between machines.

## Credits

Example code from the lab instructions; scikit-learn, SHAP, LIME; the analytical ER formula in the last
extra check is copied from our Lab 3 BRB code (`src/brbes.py` in that repository). Fonts in
`report/fonts/`: Liberation Sans (SIL OFL 1.1) and DejaVu Sans Mono (Bitstream Vera licence). An AI
assistant (Claude, by Anthropic) was used to plan the work, write the code and draft the report.
