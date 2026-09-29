# Lab 4.1 in plain language: explaining phishing detectors

AI for Cybersecurity (D7084E / D7041E), Group 6: Kirill Silchenko and Stefanos Ntentopoulos.

This is the easy-to-read version of our report. It says what we built, what we found, and what the
numbers mean, without the technical shorthand. The formal version is `report/Lab4_1_Report.pdf` (and
`.docx`). Every number here comes from the notebook `lab4_1_explaining_phishing_detectors.ipynb`.

---

## 1. The question

**Is this web address a phishing site?** A phishing site pretends to be a real one (a bank, Apple,
a login page) to steal passwords. A computer model can learn to spot them, but when it raises an alarm,
the security analyst needs to know **why** before blocking the site or warning users. So this lab is
about two things: building the detector, and **explaining** its decisions.

## 2. The data

11,430 web addresses (URLs), exactly half phishing and half real. For each one, the dataset gives 87
"clues" (features), for example:

| Clue | What it means |
|---|---|
| `google_index` | 1 if Google has **not** indexed the page (common for phishing) |
| `page_rank` | How well-linked the site is, 0–10; phishing sites usually score low |
| `nb_hyperlinks` | How many links the page has; phishing pages often have very few |
| `nb_www` | How many times "www" appears in the address |
| `phish_hints` | How many phishing words ("login", "signin", "admin"…) are in the address |
| `domain_age` | How old the domain is, in days; phishing domains are often new |

7 of the 87 clues need a **live lookup** on the internet (Google, WHOIS, DNS, traffic rank). The other 80
come from the address itself or the page.

We split the URLs into three groups, always with the same random seed (42) so anyone can repeat it:
60% to **learn from**, 20% to **choose settings**, and 20% kept aside as the **final test**. Every model
is graded on the same 2,286 test URLs.

## 3. Three ways to teach a detector

| Way | In plain words |
|---|---|
| **Supervised** | The model sees every URL *with* its answer (phishing or not). The easy, expensive way: someone had to label all of them. |
| **Semi-supervised** | Only 5% of the URLs (342) have an answer. The model labels the others itself when it is very sure ("pseudo-labels") and learns from those too. |
| **Self-supervised** | First, with *no* answers at all, a small network plays a fill-in-the-blanks game: we hide 20% of each URL's clues and it must guess them back. To win, it learns how clues go together. Then a simple model uses what it learned, plus the same 342 answers. |

To know whether the extra, unlabelled URLs helped, we also trained the same models on the 342 labelled
URLs **alone**. We call those the **lower lines**: the unlabelled data only helped if a model beats them.

## 4. How good are they?

Two numbers matter most to a security team: how many phishing sites are **caught**, and how many real
sites are **wrongly flagged** (false alarms). Per 100 phishing and per 100 real URLs in the test set:

| Detector | Phishing caught (of 100) | Real sites wrongly flagged (of 100) | Overall score (macro-F1) |
|---|---|---|---|
| Random Forest, all answers | **97** | **4** | **0.965** |
| Logistic regression, all answers | 94 | 5 | 0.947 |
| Random Forest, 5% of the answers | 94 | 6 | 0.941 |
| Semi-supervised (with pseudo-labels) | 94 | 7 | 0.934 |
| Small decision tree, all answers | 91 | 6 | 0.927 |
| Self-supervised | 92 | 9 | 0.918 |
| Logistic regression, 5% of the answers | 91 | 8 | 0.912 |

**What this says:**

- The **Random Forest** (a vote of 300 decision trees) with all the answers is the best on every count.
- With only **5%** of the answers, the plain forest is still very good (94 caught, 6 false alarms).
- **The unlabelled URLs did not help.** The semi-supervised forest came out slightly *worse* than the
  plain forest on the 5%: it caught about as many phishing sites but raised more false alarms. Its
  self-made labels were almost always right (98.9% of 4,265), but the few wrong ones did more harm than
  the extra data did good. Being stricter or looser about which self-made labels to keep (we tried
  three settings) did not change this.
- The self-supervised model was only a hair better than its lower line (0.918 against 0.912), too small
  to call a real improvement.

The lab itself warned that this could happen: on this data a forest is already strong with few answers.

## 5. Explaining the detectors

### Glass boxes: models you can read directly

- The **small decision tree** is a list of yes/no questions. Its first question is *"Has Google indexed
  this page?"*, and its second is about `page_rank`. Any decision takes at most 4 questions. That makes
  sense to a security person. The price: it catches fewer phishing sites (91 of 100).
- The **logistic regression** gives each clue one weight (plus = toward phishing). It sounds readable,
  but it uses 79 of the 87 clues, so in practice it is hard to read.

### Black boxes: explained from the outside

A forest of 300 trees cannot be read, so we used two tools that explain it from the outside:

- **SHAP** splits the model's score fairly among the clues: *"not indexed by Google added +0.13,
  few links added +0.08 …"*. The pieces always add up exactly to the score. For forests it is exact, so
  running it again gives the same answer.
- **LIME** makes thousands of small changes to one URL, watches how the score moves, and fits a straight
  line through that. It gives readable rules such as *"page_rank ≤ 1 pushes toward phishing"*, plus a
  **fit score** (0 to 1) that says how well the line copies the model. Below 0.5: do not trust it much.

**What the forest looks at:** mostly `google_index`, `page_rank`, `nb_hyperlinks`, `web_traffic` and
`nb_www`. "Not indexed by Google" is its strongest single reason to say phishing. `google_index`,
`page_rank` and `nb_www` are in the top 10 of **all five** rankings we compared (tree, logistic
regression, and SHAP for the three forest/network models). The self-supervised model partly looks at other
clues from the address text, such as hyphens and URL shorteners.

## 6. The most interesting mistake

The forest was 95% sure that **`graphicsfairy.blogspot.ru`** is phishing. It is a real blog.

Why? It is not indexed by Google, has only 5 links, no "www" and no traffic rank. That is exactly the
profile of a throw-away phishing page. A small blog on a free hosting service simply looks like one.
SHAP and LIME both blame "not indexed" and "no www", but LIME's fit score for this URL is below 0.5, so
its explanation is shaky.

## 7. Testing the detector and the explanations

**Without the live lookups.** We retrained the forest without the 7 clues that need an internet lookup.
It still catches 94 of 100 phishing sites (instead of 97), with about 5 false alarms per 100 (instead of
4). So it could work as a fast first filter, for example for a brand-new site that has no history yet. But
now **every** clue it relies on is something the attacker controls (the address and the page).

**Is LIME stable?** We ran LIME five times with different random seeds on each of three URLs:

| URL | Same top-3 clues in … | LIME fit score |
|---|---|---|
| The surest phishing URL | 5 of 5 runs | about 0.63 |
| The surest real URL | only 1 of 5 (all five runs different) | about 0.42 |
| The mistake above | 3 of 5 runs | about 0.48 |

SHAP gave exactly the same answer every time. **Lesson:** LIME is only stable where it fits well. A low
fit score is a warning sign.

## 8. Turning the explanation into rules

A **Belief Rule-Based expert system (BRB)** decides with human-readable rules, like an expert would.
We took the two most important clues from SHAP (ranked on the settings group, not the test group) that have more than two values (`google_index` is only
0/1): **`page_rank`** and **`nb_hyperlinks`**. Each gets three levels, Low / Medium / High, taken from the
training data (`page_rank` 0 / 3 / 8, links 0 / 33 / 325). That gives 3 × 3 = 9 rules, for example:

- *IF page rank is Low AND links are Low THEN risk is High (0.89).*
- *IF page rank is High AND links are High THEN risk is Low (0.96).*

The beliefs come from the data: how many of the training URLs that match a rule were phishing. When
several rules fire, a method called Evidential Reasoning combines them into one phishing score.

**How good is it?** With only 2 clues, it is much weaker than the full forest:

| Model (2 clues only, except the last) | Phishing caught (of 100) | Real sites wrongly flagged (of 100) |
|---|---|---|
| BRB | 77 | 17 |
| Tiny decision tree (2 questions) | 95 | 33 |
| Forest on the same 2 clues | 84 | 15 |
| Full forest, all 87 clues | 97 | 4 |

The BRB and the tiny tree have almost the same overall score, but the tree flags **one in three** real
sites. A security team would prefer the BRB: alarms that are wrong a third of the time get ignored.

**The mistake again.** The BRB is fooled by the same blog (phishing score 0.735), because of "few links".
When we act as the expert and change one rule by hand, it catches more phishing (82 of 100) but raises more
false alarms (18 of 100). That is the strength of a BRB: its knowledge is in rules a person can read and
change.

**Do SHAP and LIME agree with the rules?** SHAP does: it says the few links pushed the risk up most, and
those are exactly the rules that fire. LIME ranks `page_rank` slightly first, because it measures
something different (how fast the risk changes, not what pushed it).

## 9. Extra checks we did

- **Did the test URLs influence the choice of the two clues?** No. We rank the clues on the settings
  group (validation), so the test group is only used to grade the BRB. As a check, ranking on the test
  group gives the same top 10 clues in the same order.
- **Is the rule-combining code right?** We compared it with a second, independent implementation (from our
  Lab 3). The phishing scores are identical. One nuance: the lab's version reports an "Unknown" part
  (0.315 for the blog), the other reports none. In this lab, "Unknown" means "several rules fire partly",
  not "the rules disagree".
- **Would a human pick better clues?** We built a second BRB on clues a security analyst might choose:
  domain age and URL length. It is clearly worse overall (0.717 against 0.802), but it is **not** fooled
  by the blog, which is an old domain with a short address. Different clues make different mistakes.

## 10. Which clues can an attacker fake?

- **Easy to fake:** anything in the address or the page. Add "www" to the name, avoid words like "login",
  or put hundreds of hidden links on the page. The last one would move a phishing page into the BRB's
  "many links = low risk" rules.
- **Hard to fake quickly:** the live lookups. Getting indexed by Google, earning a page rank, real
  traffic, an old domain.

## 11. What we would do in practice

Use the **Random Forest with all answers**, and show the analyst its **SHAP** explanation for each alarm
(exact and stable). Run it without the live lookups as a fast first filter, and let the full model decide
when the lookups arrive. Do not show LIME explanations with a fit score below 0.5. If labelling is
expensive, labelling 5% and training a plain forest gets close; the unlabelled data added nothing here.

---

## Words used

| Word | Meaning |
|---|---|
| Feature / clue | One number describing a URL, e.g. its length or whether Google indexed it |
| Label / answer | Whether a URL is really phishing or not |
| Recall | Share of phishing sites the detector catches |
| FAR (false-alarm rate) | Share of real sites the detector wrongly flags |
| Macro-F1 | One overall score that balances both classes; 1.0 is perfect |
| Lower line | The same model trained on the few labels alone; the bar the unlabelled data must beat |
| Pseudo-label | A label the model gives itself when it is very sure |
| Glass box / black box | A model you can read directly / one you can only explain from the outside |
| SHAP | Splits a prediction fairly among the clues; exact for forests |
| LIME | Fits a straight line around one prediction; readable but random |
| BRB | A rule-based expert system with beliefs (Low / Medium / High risk) and an Unknown part |

## Where things are

| What | File |
|---|---|
| The notebook we hand in (runs top to bottom) | `lab4_1_explaining_phishing_detectors.ipynb` |
| The formal report | `report/Lab4_1_Report.pdf` and `.docx` |
| Every table and figure | `results/tables/` and `results/figures/` (names start with the lab step) |
| How to install and run | `README.md` |
