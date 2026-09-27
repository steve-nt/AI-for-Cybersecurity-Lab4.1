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

## 2026-09-26 23:20 EEST: Part D, the BRB (lab steps D1–D5), and the full hand-in notebook

**What**
- `parts/30_brb.ipynb`: created and stored with its outputs. Title, `%run 00_setup.ipynb` and a stand-in
  for A6/B2/B5/B6 (`rf`, `shap_top`, `i_wrong`, LIME import) marked `STANDIN`, then:
  - D1: top two non-0/1 SHAP features, with a check that each gets three different referential values:
    `page_rank` and `nb_hyperlinks` (`google_index` skipped as 0/1).
  - D2: the lab's BRB engine (input transformation, activation weights, Evidential Reasoning).
  - D3: referential values from the training data (page_rank 0 / 3 / 8, nb_hyperlinks 0 / 33 / 325), the 9
    data-driven rules saved to `results/tables/D3_rules.csv`, a check for rules with support below 20
    (none), and engine sanity checks (level value → full belief, halfway → 0.5/0.5, weights sum to 1,
    beliefs + Unknown = 1).
  - D4: `BRBDetector`, depth-2 tree and forest on the same two features, scores saved to
    `results/tables/D4_brb_scores.csv`, the depth-2 tree printed as rules.
  - D5: rule trace for the forest's mistake (with each firing rule's beliefs), LIME on the High-risk
    output, SHAP KernelExplainer waterfall saved to `results/figures/D5_shap_brb_waterfall_high.png`
    (`silent=True`), and the expert hand-edit of R4 run on a copy and restored afterwards.
  - "What we see" cells for D1, D3, D4 and D5.
- `lab4_1_explaining_phishing_detectors.ipynb`: rebuilt with `tools/assemble.py --strict` and executed:
  all 24 lab steps A0–D5, 67 cells, no stand-in cells, no errors, about 7 minutes.
- `results/figures/B2_shap_forest_beeswarm.png`: regenerated by the rebuild (SHAP's beeswarm shuffles
  its drawing order, so the image bytes change on every run; the values do not).
- `TASKLIST.md`: ticked D1–D5 (the optional X5 cross-check is not done).

**Why**
Part D turns the explanation into readable rules and checks whether LIME and SHAP agree with them. The
full rebuild proves the single hand-in notebook runs from top to bottom, as the lab requires.

**Verified**
Test set (macro-F1 / recall / FAR): BRB 0.802 / 0.773 / 0.169; depth-2 tree 0.803 / 0.948 / 0.334; forest on
the two features 0.847 / 0.842 / 0.148; forest on all 87 0.965 / 0.968 / 0.038. The BRB is fooled by the
same URL as the forest (phishing score 0.735, Unknown 0.315). SHAP says the few links push High risk up
most; LIME (fit 0.74) ranks page_rank slightly first. The hand-edit of R4 raises recall to 0.818 and FAR
to 0.183. Every results table rewritten by the full rebuild is identical to the part-notebook runs.

## 2026-09-27 03:15 EEST: Optional extras X1–X5

**What**
- `parts/20_label_scarce.ipynb`, X1: pseudo-labelling repeated with CUTOFF 0.85 / 0.90 / 0.95, compared on
  validation with the 5% lower line (assert: 0.90 reproduces step A8); saved to
  `results/tables/X1_cutoff_sweep.csv`.
- `parts/10_supervised_forest.ipynb`, X2: LIME with seeds 0–4 on all three B5 URLs, plus SHAP 5 times on
  each; saved to `results/tables/X2_lime_stability_three_urls.csv`.
- `parts/30_brb.ipynb`:
  - X3: a second BRB on the hand-picked pair `domain_age` + `length_url` (the lab's example `nb_www` has
    only two distinct referential values), built and scored like D3/D4 inside a helper that restores the
    step-D rules; saved to `results/tables/X3_brb_pairs.csv` and `X3_rules_domain_age_length_url.csv`.
  - X4: SHAP ranking of the forest on 500 validation URLs against the test ranking used in D1; saved to
    `results/tables/X4_shap_ranking_test_vs_validation.csv`.
  - X5: analytical ER copied from our Lab 3 `src/brbes.py` (`er_aggregate`, credited in the cell, because
    `PreviousLabs/` is not in the repository) and compared with the lab's recursive ER on every test URL;
    saved to `results/tables/X5_er_crosscheck.csv`.
  - D5 answer corrected: the large Unknown comes from the rule weights being split over four partly-active
    rules, not from the rules disagreeing (see X5).
- "What we see" cells for X1–X5.
- `lab4_1_explaining_phishing_detectors.ipynb`: rebuilt with `--strict` and executed (82 cells, no errors).
- `results/figures/B2_shap_forest_beeswarm.png`: regenerated (drawing order only).
- `TASKLIST.md`: ticked X1–X5 and the optional items in C2 and D2; X3's description now names the pair used.

**Why**
The extras test the robustness of the main results: whether the cutoff changes the A10 verdict, whether
LIME's stability holds beyond the easiest URL, whether intuition beats SHAP for choosing BRB features,
whether choosing features on test rows leaked, and whether the ER code is correct.

**Verified**
X1: no cutoff beats the forest on 5% labels (validation macro-F1 0.935 / 0.931 / 0.931 against 0.938).
X2: LIME same top 3 in 5, 1 and 3 of 5 runs (phishing / legitimate / wrong URL); SHAP 5 of 5 on all three.
X3: hand-picked BRB macro-F1 0.717 against 0.802, but phishing score 0.366 on the forest's mistake.
X4: identical top-10 ranking on validation and test. X5: normalised beliefs and phishing scores differ by
less than 1e-15; mean Unknown 0.234 (recursive) against 0.000 (analytical). Before adding X5, a scratch
check confirmed that the notebook's copy equals the real Lab 3 module exactly. Part notebooks take 4.6,
4.8 and 3.1 minutes; the hand-in notebook about 10 minutes.

## 2026-09-27 12:45 EEST: Report, README and final check

**What**
- `report/Lab4_1_Report.md`: the report source, with the lab's five headings, 6 tables, a two-panel
  Figure 1 (SHAP beeswarm, LIME for the forest's mistake) and a code appendix. Every result number is a
  `{{file:row:column}}` token or a `[[table ...]]` directive filled from `results/tables/*.csv` at build
  time. The few numbers typed by hand (hand-edit scores, LIME fit 0.74, Unknown 0.315, 79 of 87 weights,
  referential values, top-10 overlaps) were checked against the hand-in notebook's output. The
  "who did what" section is left as a clearly marked placeholder for the group; the AI-use statement says
  what the assistant was used for.
- `report/build_report.py`: builds `report/Lab4_1_Report.pdf` (fpdf2 + markdown): fills the tokens,
  renders tables with fitted column widths, places figures side by side, renders the code of steps A8
  and D3 from the hand-in notebook as appendix images (`report/figures/code_A8.png`, `code_D3.png`), and
  prints the page count.
- `report/fonts/`: Liberation Sans and DejaVu Sans Mono with their licences, so that the PDF builds the
  same on every machine.
- `report/Lab4_1_Report.pdf`: 5 pages, 3 of main text and 2 of appendix.
- `README.md`: created (dataset, setup, Colab, how to run, code organisation, headline results,
  reproducibility, credits).
- `requirements.txt`: added `fpdf2==2.8.8` and `markdown==3.11` for the report build.
- `parts/10_supervised_forest.ipynb` (extra LIME-stability check): `max(sets, key=sets.count)` instead
  of `max(set(sets), ...)`. With all 5 LIME runs different, the "most common" set was picked by set
  iteration order, which depends on Python's string-hash randomisation, so it changed between processes.
- `lab4_1_explaining_phishing_detectors.ipynb`, `results/`: re-run and rebuilt.
- `TASKLIST.md`: ticked the finished report, README and final-check items. Still open: the who-did-what
  line and the upload to Canvas.

**Why**
The report, README and final check are what is handed in. Filling numbers from the result files keeps
the report consistent with the notebook. The report does not mention the task list or internal task
IDs (report-no-tasklist skill; checked with grep on the source and on the PDF text).

**Verified**
Final check on a fresh copy of every file that would be committed (no `.venv`, no `PreviousLabs`), with a
new environment from `requirements.txt`: the hand-in notebook ran top to bottom in 9.5 minutes, the report
built, and the report text was identical to the original. All result tables were byte-identical except the
LIME-stability table's "most common" column, which led to the tie-break fix above. After the fix, a part
run and the hand-in run (separate processes) give identical tables. No absolute paths in the files, no
`STANDIN` cell in the hand-in notebook, 0 errors.

## 2026-09-27 13:13 EEST: Word version of the report

**What**
- `report/build_docx.py`: adapted from our Lab 3 script. It now reads `report/Lab4_1_Report.md`, fills
  the numbers and tables with the same code as the PDF build (`build_report.expand`), skips the Markdown
  title and author line, handles `[[figures]]` (images side by side), `[[code]]` (the notebook's code
  rendered as images) and `[[pagebreak]]`, uses 10 pt body text, and sets the title page's assignment
  title to "Lab 4.1: Explaining Phishing Detectors".
- `report/title_page_template.docx`: copied from Lab 3 (title page with course, group, authors and the
  university logo), because `PreviousLabs/` is not in the repository.
- `report/Lab4_1_Report.docx`: built.
- `README.md`: mentions the Word version and how to build it.

**Why**
Requested: the report as a Word document, built by our existing script, so that it says the same as
the PDF and can be edited in Word.

**Verified**
All 12 XML parts parse; python-docx opens the file (6 tables, 5 images including the logo, headings 1–5
and the appendix); every number in the PDF also appears in the Word file; no task-list references. The
page count in Word was not checked (no Word or LibreOffice here); with US Letter, the template's margins
and 10 pt text it may differ from the PDF's 3 + 2 pages.
