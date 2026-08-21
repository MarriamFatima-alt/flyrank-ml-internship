# Search Ranking & Discoverability — Capstone Project

**Applied Search Intelligence: Google Search Ranking & Discoverability**
FlyRank AI ML Engineering Internship — Capstone

## What it does and who it's for

This project builds a **content-refresh prioritization pipeline** for a website's search
pages. Given a set of pages with performance signals (traffic, rankings, content age,
engagement, etc.), it produces a ranked "review first" queue that tells an SEO/content
team which pages are most worth refreshing — instead of guessing or reviewing pages in
random order.

It's built for content and SEO teams who have too many pages and too little time to
review them all manually.

## Setup — how a stranger can reproduce this

```bash
git clone https://github.com/MarriamFatima-alt/flyrank-ml-internship
cd flyrank-ml-internship
pip install -r requirements.txt
python scripts/run_all.py
```

This runs the full pipeline on the bundled anonymized sample dataset
(`data/raw/content_refresh_anonymized.csv`, ~30k pages) and writes results to `outputs/`.

Prefer no local setup? Open `notebooks/01_first_look_and_discovery.ipynb` in Google Colab
and run all cells — zero install required.

## Usage example

```bash
python scripts/01_prepare_features.py   # clean + build feature vector, define label
python scripts/02_baseline_score.py     # transparent hand-rule "fix this first" score
python scripts/03_train_model.py        # logistic regression, decision tree, random forest
python scripts/04_evaluate_and_export.py  # ranked queue + charts + Markdown report
python scripts/05_build_pdf_report.py   # shareable PDF summary
```

Output: a ranked CSV of pages to review first, a Markdown report, charts, and a PDF
summary — all in `outputs/`.

## Simple architecture sketch

```
Raw page data (CSV)
      │
      ▼
01_prepare_features.py  ──►  cleaned feature vector + label
      │
      ▼
02_baseline_score.py    ──►  hand-rule "fix this first" score  (transparent baseline)
      │
      ▼
03_train_model.py       ──►  trained model (logistic regression / decision tree /
      │                       random forest), evaluated on a client-holdout split.
      │                       Best model selected by Precision@50 → random forest.
      ▼
04_evaluate_and_export.py ──►  ranked review queue + charts + Markdown report
      │
      ▼
05_build_pdf_report.py  ──►  shareable PDF summary
```

## v2 eval results

Evaluated on the bundled anonymized sample (30,000 rows scored; 16,262 declining-label
rows, a 0.542 declining-label rate), using a **client-holdout split** to avoid leakage
across clients.

**Precision@50** — of the top 50 pages the model flags for review, how many are actually
pages that needed it:

| Model | ROC AUC | Avg precision | Precision@50 | Recall | F1 |
|---|---|---|---|---|---|
| **Random forest (selected)** | 0.750 | 0.618 | **0.740** | 0.744 | 0.640 |
| Decision tree | 0.742 | 0.575 | 0.580 | 0.716 | 0.634 |
| Logistic regression | 0.700 | 0.522 | 0.400 | 0.567 | 0.566 |
| Hand-written baseline rule | 0.627 | 0.468 | 0.240 | – | – |

The best model (random forest, selected by Precision@50) roughly **triples** the
hand-written baseline's precision at prioritizing which pages to review first —
0.240 → 0.740. The exported final queue flags 3,602 pages as high-confidence review
candidates, out of 30,000 pages scored.

Results are **observed / measured on the bundled anonymized sample** — this is a
decision-support signal for a content team, not a prediction of Google's ranking
algorithm.

## Limitations

- Trained and evaluated only on the bundled **anonymized sample** (~30k pages) — not the
  full ~79M-row warehouse, so results may shift on the full dataset or a different site's
  data.
- The model ranks *review priority*, not guaranteed ranking outcomes — a page flagged
  "review first" still needs human judgment on what to actually change.
- [Add one more real limitation you noticed while running it — e.g. a feature that had a
  lot of missing values, or a case where the model and baseline disagreed and you're not
  sure which was right.]

## Built with AI — transparency note

This pipeline is built on FlyRank's internship starter template. I used AI assistance
(Claude) to help draft this README and understand the pipeline structure; the actual
run, evaluation numbers, and outputs came from executing the unmodified starter scripts
on the provided sample dataset, as instructed in the assignment brief.

## Demo

See the 3–5 minute demo video linked in the assignment submission thread — a live run of
`python scripts/run_all.py` with narration explaining each step and one limitation
explained on camera.

**Build-in-public post:** [View on LinkedIn →](https://www.linkedin.com/posts/marriam-fatima-47687b409_aiml-machinelearning-flyrankinternship-share-7496473737987325952-2cIb/)

---
*Built as part of the FlyRank AI ML Engineering Internship — Applied Search Intelligence track.*
