# Changelog

History of the changes made in this project, oldest first. Each entry says what changed, why,
and when. New entries are appended at the bottom.

## 2026-09-23 23:42 EEST: Task list for Lab 4.1

**What**
- `TASKLIST.md`: created. Task overview without owners at the top (T1–T7, lab steps A1–D5, extras
  X1–X5), division of work between Kirill (supervised forest, Part C) and Stefanos (label-scarce
  models, comparisons, BRB), sync points, suggested schedule, starting point, kickoff agreements
  (data roles, part notebooks with step markers and stand-in cells, repository layout, shared
  variable names), detailed tasks, "Check yourself" owners, grading checklist, teacher questions.

**Why**
Requested to plan Lab 4.1 step by step for two people working as independently as possible. The
overview went at the top of `TASKLIST.md` instead of a separate `TASKS.md`, so the two lists cannot
drift apart. The part-notebook + assembly-script method lets both work in separate files and still
hand in the single top-to-bottom notebook the lab requires.

**Note**
Time is the commit time of `9bbcbea`; the file was written earlier that evening (exact time not
recorded).

## 2026-09-26 10:34 EEST: Ignore the PreviousLabs folder

**What**
- `.gitignore`: created with `PreviousLabs/`.

**Why**
`PreviousLabs/` holds local reference copies of earlier labs that should not go into this
repository. Checked with `git check-ignore`. Time is the commit time of `23ceb27`.

## 2026-09-26 10:43 EEST: Task T2, environment, dataset and folder layout

**What**
- `data/dataset_phishing.csv`: downloaded from Mendeley Data (original name `dataset_B_05_2020.csv`,
  dataset c2gw7fy2j4 version 3) and renamed to the name the lab uses.
- `data/README.md`: created. Citation (Hannousse & Yahiouche, 2021), CC BY 4.0 licence, source, the
  rename, SHA-256 and the command to download it again.
- `requirements.txt`: created. Pinned to the versions in our Lab 3 environment: scikit-learn 1.6.1,
  shap 0.52.0, lime 0.2.0.1, numpy 2.5.3, pandas 3.0.6, scipy 1.18.1, matplotlib 3.11.2, joblib,
  threadpoolctl, jupyter, ipykernel, nbformat, nbconvert.
- `.gitignore`: added `.venv/`, `__pycache__/`, `.ipynb_checkpoints/`, and a note that the dataset is
  committed on purpose.
- `parts/`, `tools/`, `results/figures/`, `results/tables/`, `report/`: created, each with an empty
  `.gitkeep` so that git keeps the folder.
- `.venv/`: created locally with `uv` from `requirements.txt` (ignored by git).
- `TASKLIST.md`: ticked the finished T2 items and updated the dataset row in section 1.

**Why**
Task T2 (Stefanos): both machines must run the same code with the same library versions. The dataset
is committed because it is small (3.7 MB) and CC BY 4.0 allows redistribution with credit. The Lab 3
versions were reused because they are known to work together.

**Verified**
SHA-256 of the CSV matches the hash Mendeley publishes. 11,430 rows × 89 columns, 5,715 per class, no
missing values, all 7 external features present. In the fresh `.venv`, the lab's split gives
6,858 / 2,286 / 2,286; a small forest, SHAP (shape `(n, 87, 2)`) and LIME all run.

**Next**
Commit on `stente-5`, merge to `main` so that Kirill can start T3. The T2 box in the overview stays
open until then.

## 2026-09-26 10:58 EEST: Global changelog skill (outside this repository)

**What**
- `~/.claude/skills/changelog/SKILL.md`: created. Instructions to append an entry (what, why, real
  date and time) to `CHANGELOG.md` in the project root after every change, creating the file if
  needed, oldest first, never rewriting old entries.

**Why**
Requested: a global skill that keeps the history of every project and conversation. Named `changelog`
because skill names must be lowercase; it maintains `CHANGELOG.md`.

## 2026-09-26 11:00 EEST: Changelog rule in the global instructions (outside this repository)

**What**
- `~/.claude/CLAUDE.md`: added the section "Changelog: always use the changelog skill".

**Why**
A skill is only used when its description matches the task. A rule in the global instructions is
loaded in every session, so the changelog is written after every change, not only sometimes.

## 2026-09-26 11:01 EEST: Backfilled this changelog

**What**
- `CHANGELOG.md`: filled the empty file with the header and the entries above.

**Why**
The changelog skill was created after the earlier changes were made, so they were recorded afterwards.
Their times come from the git commit times (`9bbcbea`, `23ceb27`) and the files' modification times,
not from memory.

## 2026-09-26 11:05 EEST: Task T3, setup notebook (lab steps A0–A3)

**What**
- `parts/00_setup.ipynb`: created and stored with its outputs. One Markdown and one code cell per step,
  each starting with its step marker (`<!-- STEP A0 -->` / `# STEP A0` … `A3`):
  - A0: installs shap and lime only when they are missing (Colab); locally they come from
    `requirements.txt`, because the uv environment has no pip.
  - A1: moves to the repository root when run from `parts/`, loads `data/dataset_phishing.csv` (or
    `dataset_phishing.csv` in Colab), creates `results/figures/` and `results/tables/`, and asserts shape
    (11430, 89) and 5,715 URLs per class.
  - A2: the lab's label, feature matrix and 60/20/20 stratified split (`random_state=42`), with asserts
    for 87 features, 6,858 / 2,286 / 2,286 rows and a 50% phishing share in each part.
  - A3: the lab's `report()` helper, unchanged.
- `TASKLIST.md`: ticked the finished T3 items (the merge to `main` is still open).

**Why**
Task T3: every part notebook starts with `%run 00_setup.ipynb`, so the data, split and `report()` must
be identical for both of us. The asserts stop a notebook early if the data or split ever differ from
the numbers the lab gives. The code follows the lab's example so that the assembled notebook matches
the instructions.

**Verified**
Executed with nbconvert: all asserts pass. A throwaway notebook in `parts/` ran `%run 00_setup.ipynb`,
ended up in the repository root, and scored a 300-tree forest with `report()` on validation (macro-F1
0.961); the throwaway notebook was deleted afterwards.

## 2026-09-26 11:44 EEST: Global report-no-tasklist skill (outside this repository)

**What**
- `~/.claude/skills/report-no-tasklist/SKILL.md`: created. For any file whose name contains "report"
  or "deliverable" (any capitalisation or extension), and for scripts or drafts that write into one: never
  mention a task list (task-list file names, the words "task list" or "tasklist", internal task IDs such as
  T1–T7 or X1–X5, checkboxes, plan schedules). The assignment's own step names (e.g. "Step A4") are
  still allowed. Includes a grep check for leftovers.

**Why**
Requested: reports and deliverables are handed in and must not refer to our internal planning file.

## 2026-09-26 11:46 EEST: Make the report rule consistent and dependable

**What**
- `TASKLIST.md` (T5): the AI-use line now says "at least for planning the work" instead of "at least for
  this task plan", and a new checklist item says that the report never mentions the task list (the
  lab's own step names A1–D5 are fine).
- `~/.claude/CLAUDE.md` (outside this repository): added the section "Reports and deliverables: never
  mention a task list", which points to the `report-no-tasklist` skill.

**Why**
The old T5 wording would have put a mention of the plan into the report, against the new
`report-no-tasklist` skill. The rule in the global instructions is loaded in every session, so the skill
is applied reliably and not only when its description happens to match.

## 2026-09-26 12:06 EEST: Task T4, assembly script

**What**
- `tools/assemble.py`: created. Reads `parts/*.ipynb` in name order, takes the step marker from each
  cell's first line (`# STEP A4` / `<!-- STEP A4 -->`), drops `STANDIN` cells and empty cells, sorts
  by lab step (A0–A10, B1–B6, C1–C2, D1–D5, then extras X1–X5) while keeping notebook order inside a
  step, adds a title cell, gives every cell a fresh id and no old outputs, validates, writes
  `lab4_1_explaining_phishing_detectors.ipynb` and executes it from the repository root.
  - Stops with a list of file + cell number for cells without a marker or with an unknown step.
  - Prints which part notebook each step came from, warns when a step comes from more than one
    notebook, and lists missing lab steps.
  - Options: `--no-execute` (build only), `--strict` (fail if any lab step A0–D5 is missing, for the
    hand-in), `--parts` and `--output` (used for testing).
  - On an execution error it keeps the outputs up to the failing cell and exits with code 1.
- `lab4_1_explaining_phishing_detectors.ipynb`: first generated version (title + steps A0–A3, executed).
- `TASKLIST.md`: ticked T4 (execution is built into the script, so the separate nbconvert command is
  no longer needed).

**Why**
Task T4: we work in separate part notebooks to avoid merge conflicts, but the lab requires one notebook
that runs from top to bottom. Built early so that problems with markers show up long before the final
assembly.

**Verified**
Real build from `parts/00_setup.ipynb` executes without errors, also when started from another folder.
Throwaway tests in `/tmp` (deleted afterwards): cells from two part notebooks interleave in lab order;
the stand-in cell never runs; the duplicate-step warning appears; unmarked cells and an unknown step
stop the build with exit code 1; `--strict` exits 1 when steps are missing; a failing cell keeps the
earlier outputs and exits 1.

## 2026-09-26 12:18 EEST: Point from Part A to the setup steps

**What**
- `TASKLIST.md`: added a first row "A1–A3" to the Part A table of the task overview, pointing to T3.

**Why**
The Part A table started at A4, which looked like a numbering gap. The lab's steps A1–A3 are done in T3
(setup notebook), and the new row says so.

## 2026-09-26 12:33 EEST: Lab steps A7–A9, label-scarce models

**What**
- `parts/20_label_scarce.ipynb`: created and stored with its outputs. A title cell and
  `%run 00_setup.ipynb`, both marked `STANDIN`, then one Markdown and one code cell per step:
  - A7: 5% labelled / 95% unlabelled split of the training set (`stratify`, `random_state=42`), with
    asserts that it gives 342 / 6,516 rows, covers exactly the training rows and has no validation or
    test URL; lower lines `rf_few` and `logreg_few` on the 5% only.
  - A8: pseudo-labelling as in the lab (3 rounds, `CUTOFF = 0.9`, final `rf_semi`), plus a per-round check
    of how many guesses were right against `y_hidden` (check only, never used to train), saved to
    `results/tables/A8_pseudo_label_rounds.csv`.
  - A9: fill-in-the-blanks network (20% masking, `MLPRegressor(32)`), `encode()` and the `self_sup`
    Pipeline as in the lab, printed next to `logreg_few`, the fair comparison.
- `results/tables/A8_pseudo_label_rounds.csv`: created by A8.
- `lab4_1_explaining_phishing_detectors.ipynb`: rebuilt and executed with `tools/assemble.py` (now A0–A3
  and A7–A9).
- `TASKLIST.md`: ticked A7, A8 and A9.

**Why**
Lab steps A7–A9: the lower lines are needed to judge whether the unlabelled 95% helped, and the lab
keeps `y_hidden` to check the pseudo-labels, which its example code does not do.

**Verified**
Validation scores (macro-F1 / recall / FAR): forest 5% 0.938 / 0.946 / 0.069; logreg 5%
0.912 / 0.915 / 0.092; semi-supervised 0.931 / 0.946 / 0.083; self-supervised 0.914 / 0.918 / 0.090.
Pseudo-labels: 2,778 + 1,122 + 365 added, accuracy 99.4% → 98.5% → 95.6% per round (98.9% overall).
The assembled notebook runs top to bottom and gives the same numbers.

## 2026-09-26 21:30 EEST: Warning filter and lab step A10

**What**
- `parts/00_setup.ipynb` (A0): added `warnings.filterwarnings("ignore", message="Unknown solver options")`,
  with a comment; re-executed.
- `parts/20_label_scarce.ipynb`: re-executed without the warning, and added:
  - a `STANDIN A4-A6` cell with the lab's code for `tree`, `logreg` and `rf` (dropped by the assembler);
  - A10: the 7 detectors on the test set, saved to `results/tables/A10_test_scores.csv`, with the two
    lower-line comparisons and the distance to the upper line printed;
  - an A10 Markdown cell "What we see" that answers the two "Check yourself" questions with the numbers.
- `results/tables/A10_test_scores.csv`: created by A10.
- `TASKLIST.md`: ticked A10.
- `lab4_1_explaining_phishing_detectors.ipynb`: overwritten by a build-only run
  (`tools/assemble.py --no-execute`), so it now has no outputs and cannot run past A10 until the real
  A4–A6 cells exist. The committed version (executed, A0–A9) can be restored from git.

**Why**
scipy 1.18 with scikit-learn 1.6.1 printed a harmless `OptimizeWarning: Unknown solver options: iprint`
for every logistic regression. A10 is the main results table and answers whether the unlabelled data
helped.

**Verified**
Test set (macro-F1 / recall / FAR): tree 0.927 / 0.912 / 0.057; logreg 0.947 / 0.943 / 0.050; forest (100%)
0.965 / 0.968 / 0.038; forest (5%) 0.941 / 0.942 / 0.061; logreg (5%) 0.912 / 0.908 / 0.084; semi-supervised
0.934 / 0.940 / 0.071; self-supervised 0.918 / 0.922 / 0.087. In a throwaway assembly in `/tmp` with
A4–A6 present as real steps, the notebook ran top to bottom and A10 gave the same table.

## 2026-09-26 22:06 EEST: Lab steps A4–A6, B3 and B4

**What**
- `parts/10_supervised_forest.ipynb`: created and stored with its outputs. Title and `%run 00_setup.ipynb`
  marked `STANDIN`, then:
  - A4: decision tree, depth 3 and 4 compared on validation; depth 4 kept. Both depths saved to
    `results/tables/A4_tree_depth.csv`.
  - A5: logistic regression in a scaler Pipeline. A6: Random Forest, 300 trees, all labels.
- `parts/20_label_scarce.ipynb`: added and executed:
  - B3: SHAP for the semi-supervised forest (TreeExplainer, 500 test URLs) and the self-supervised model
    (permutation explainer, 100 training URLs as background, 200 test URLs, `silent=True` to keep a
    200-line progress bar out of the notebook). Beeswarms saved to
    `results/figures/B3_shap_self_beeswarm.png` and `B3_shap_semi_beeswarm.png`.
  - A `STANDIN B1-B2` cell with the lab's code for `w` and `sv1`.
  - B4: the 5-column top-10 table saved to `results/tables/B4_top10_lists.csv`, counts per feature, the
    overlap between the lists, and a "What we see" Markdown cell with the answers.
- `TASKLIST.md`: ticked A4, A5, A6, B3 and B4.

**Why**
A4–A6 are the supervised models that A10 and B1/B2/B5 need. With them in place, the assembled notebook
no longer depends on the A4–A6 stand-in. B3/B4 answer whether the models look at the same clues.

**Verified**
Validation: tree depth 3 macro-F1 0.913, depth 4 0.925 (kept); logreg 0.938; forest 0.961. No
ConvergenceWarning. SHAP shapes (500, 87) and (200, 87). `google_index`, `page_rank` and `nb_www` are in
all 5 top-10 lists; the two forests share 8 of 10 features, the self-supervised model shares 5 with the
supervised forest and 3 with the semi-supervised one. A throwaway assembly in `/tmp` (real parts plus a
temporary B2 made from the stand-in) ran top to bottom; A10 gave the same table as before. Running
`parts/20_label_scarce.ipynb` takes about 9.5 minutes, 5.7 of them in the permutation explainer.
`lab4_1_explaining_phishing_detectors.ipynb` was not rebuilt: it would stop at B4 until B1 and B2 exist.

## 2026-09-26 22:40 EEST: Lab steps B1 and B2, seeded SHAP for the self-supervised model, full rebuild

**What**
- `parts/10_supervised_forest.ipynb`: added and executed:
  - B1: tree rules (`export_text`, 16 leaves) saved to `results/tables/B1_tree_rules.txt`; the 10
    strongest logistic-regression weights saved to `results/tables/B1_logreg_top10_weights.csv`;
    "What we see" cell (first question = "Has Google indexed this page?"; 79 of 87 weights > 0.01).
  - B2: SHAP for the supervised forest on 500 test URLs, shape (500, 87, 2) checked; bar and beeswarm
    plots saved to `results/figures/B2_shap_forest_bar.png` and `B2_shap_forest_beeswarm.png`;
    `shap_top` saved to `results/tables/B2_shap_top.csv`; "What we see" cell (red dots of
    `google_index` on the right = not indexed pushes toward phishing).
- `parts/20_label_scarce.ipynb` (B3): `shap.Explainer(self_prob, background, seed=42)`, re-executed.
- `results/tables/B4_top10_lists.csv`, `results/figures/B3_*`: regenerated with the seeded explainer.
- `lab4_1_explaining_phishing_detectors.ipynb`: rebuilt and executed; it now holds A0–A10 and B1–B4, all
  real steps, and runs top to bottom without errors.
- `TASKLIST.md`: ticked B1 and B2.

**Why**
B1/B2 are the glass-box and SHAP explanations of the supervised models, and `shap_top` is needed for the
BRB (D1). The seed: the permutation explainer draws random feature orders, so without a seed the
self-supervised SHAP ranking changed between runs (ranks 4 and 5 swapped in `B4_top10_lists.csv`), which
breaks "the notebook reproduces your numbers". A quick test confirmed: no seed → different values,
`seed=42` → identical values.

**Verified**
Supervised-forest SHAP top 5: `google_index`, `page_rank`, `nb_hyperlinks`, `web_traffic`, `nb_www`. The
part notebook and the hand-in notebook now produce a byte-identical `B4_top10_lists.csv`. With the seed,
the self-supervised top 10 contains the same features as before in a slightly different order, so the
B4 answers still hold. The hand-in notebook takes about 9.5 minutes.

## 2026-09-26 22:50 EEST: Lab steps B5 and B6, three URLs explained locally

**What**
- `parts/10_supervised_forest.ipynb`: added and executed:
  - B5: the forest's surest phishing URL (test row 34), surest legitimate URL (row 10) and most
    confident mistake (row 1257), as in the lab, with the raw URL text; saved to
    `results/tables/B5_three_urls.csv`.
  - B6: SHAP waterfalls (`results/figures/B6_shap_waterfall_<name>.png`) and LIME charts with the lab's
    settings (`results/figures/B6_lime_<name>.png`), fit scores below 0.5 flagged, and the SHAP-vs-LIME
    top-3 comparison saved to `results/tables/B6_shap_vs_lime_top3.csv`.
  - "What we see" cells for B5 and B6.
- `TASKLIST.md`: ticked B5 and B6.

**Why**
Local explanations answer the analyst's "why this URL?". The mistake (`i_wrong`) is explained again by
the BRB in D5.

**Verified**
80 test mistakes of 2,286. The mistake is `http://graphicsfairy.blogspot.ru/` (legitimate, P = 0.950):
not indexed, 5 links, no "www", no traffic rank. SHAP and LIME share 2, 1 and 2 of their top 3 features;
LIME fit scores are 0.63, 0.42 and 0.48. The hand-in notebook was not rebuilt in this step (it will be
rebuilt after Part D).

## 2026-09-26 23:00 EEST: Lab steps C1 and C2, ablation and LIME stability

**What**
- `parts/10_supervised_forest.ipynb`: added and executed:
  - C1: forest retrained without the 7 external features (names checked against the columns first),
    both forests scored on the test set and saved with the change to `results/tables/C1_ablation.csv`;
    SHAP top 10 of the new forest saved to `results/tables/C1_shap_top_without_external.csv`.
  - C2: LIME 5 times with seeds 0–4 on the surest phishing URL, as in the lab, saved to
    `results/tables/C2_lime_stability.csv`; plus SHAP run 5 times on the same URL, so that "SHAP 5 of 5"
    is measured, not assumed.
  - "What we see" cells for C1 and C2.
- `TASKLIST.md`: ticked C1 and C2 (the "write both numbers in the report" item stays open for the
  report).

**Why**
C1 shows how much the detector depends on live lookups; C2 tests whether the LIME explanation can be
trusted from run to run.

**Verified**
Without external features: macro-F1 0.965 → 0.944, recall 0.968 → 0.940, FAR 0.038 → 0.052; new top 5
`nb_hyperlinks`, `nb_www`, `phish_hints`, `safe_anchor`, `ratio_extHyperlinks`. LIME 5 of 5 same top 3,
SHAP 5 of 5 identical values. The part notebook now takes about 8.5 minutes.
