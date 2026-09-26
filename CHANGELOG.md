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
