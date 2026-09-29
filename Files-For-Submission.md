# Files for submission

The lab asks for two uploads on Canvas: **the report as a PDF** and **the code** (a zip or a repository
link). The code must be one notebook that runs from top to bottom, plus a short README (libraries,
dataset, how to run), with `random_state=42`.

The whole project is about 11 MB (69 files). The `.venv` (825 MB) and `PreviousLabs/` (1.7 GB) folders
must stay out.

## 1. The report (upload as PDF)

| File | What to do |
|---|---|
| `report/Lab4_1_Report.pdf` | **Upload this.** 4 pages: 3 pages of report + a 1-page code appendix, built from the Markdown source |
| `report/Lab4_1_Report.docx` | The same report in Word, with our title page. Upload it only if a Word file is wanted; it also ships in the code zip |

If you edit the text, change `report/Lab4_1_Report.md` and rebuild both:
`.venv/bin/python report/build_report.py` and `.venv/bin/python report/build_docx.py`.

## 2. The code (zip these, or submit the repository link)

| Path | Why it is needed |
|---|---|
| `lab4_1_explaining_phishing_detectors.ipynb` | **The notebook the lab asks for**: steps A0–D5 in order plus the extra checks, runs top to bottom (about 10 minutes), stored with its outputs |
| `README.md` | Libraries and versions, dataset, how to run (locally and in Colab), seed, headline results |
| `requirements.txt` | Pinned versions (scikit-learn 1.6.1, shap 0.52.0, lime 0.2.0.1, …) |
| `data/dataset_phishing.csv`, `data/README.md` | The dataset (CC BY 4.0, may be shared with credit), its source, licence and hash |
| `results/tables/` (20 files), `results/figures/` (11 files) | Every table and figure the notebook writes and the report cites |
| `parts/` (4 notebooks), `tools/assemble.py` | How the hand-in notebook is built: the part notebooks and the script that joins them in lab order |
| `report/Lab4_1_Report.md`, `report/build_report.py`, `report/build_docx.py`, `report/title_page_template.docx`, `report/fonts/`, `report/figures/` | The report source and how the PDF and Word files are generated from it |
| `.gitignore` | Keeps the environment and previous labs out of the repository |

Optional, not required by the lab but useful to a reader:

| Path | What it is |
|---|---|
| `Report-Plain-Lang.md` | The report in plain language |
| `CHANGELOG.md` | The history of every change, with dates and reasons |
| `TASKLIST.md` | The plan we worked to |

## 3. Do not submit

| Path | Reason |
|---|---|
| `.venv/` | Local Python environment (825 MB); `requirements.txt` replaces it |
| `PreviousLabs/` | Copies of Labs 1–3 (1.7 GB), already submitted with those labs; git-ignored |
| `.git/` | Only needed if you submit the repository link instead of a zip |
| `4.1 Lab_ Explaining_Phishing_Detectors.pdf`, `.txt` | The assignment instructions, not our work |
| `Files-For-Submission.md` | This checklist |
| `__pycache__/`, `.ipynb_checkpoints/` | Caches; git-ignored |

## 4. Before zipping

- [ ] Read the report PDF once more, especially section 5 (who did what) and the AI-use statement.
- [ ] If the Markdown changed, rebuild the PDF and the Word file (section 1).
- [ ] Commit and push everything, so that the zip and the repository link match.
- [ ] Optional: merge `stente-5` into `main`, so that the repository link shows the final version.

The final check has been done: on a fresh copy with a new environment from `requirements.txt`, the
notebook ran top to bottom without errors and reproduced every table; the report numbers match.

## 5. Making the zip

From the repository root, after committing, this packs exactly the committed files. The environment,
previous labs, caches and `.git` are left out automatically:

```bash
git archive --format=zip --prefix=AI-for-Cybersecurity-Lab4.1/ -o ../Lab4_1_Group6_code.zip HEAD
```

To leave out the files listed in section 3 as "do not submit" (the instructions and this checklist), and
optionally the planning files:

```bash
zip -d ../Lab4_1_Group6_code.zip \
  "AI-for-Cybersecurity-Lab4.1/4.1 Lab_ Explaining_Phishing_Detectors.pdf" \
  "AI-for-Cybersecurity-Lab4.1/4.1 Lab_ Explaining_Phishing_Detectors.txt" \
  "AI-for-Cybersecurity-Lab4.1/Files-For-Submission.md"
```

The zip is about 6 MB (tested on 2026-09-27). Then upload `../Lab4_1_Group6_code.zip` and
`report/Lab4_1_Report.pdf` to Canvas.
