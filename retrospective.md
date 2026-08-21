# Retrospective

**Written for the person I was in Week 1**

When I started this track, the thing I set out to prove — the claim behind my
portfolio at marriamfatima.vercel.app — was simple: that I could actually *ship*
working AI/ML systems, not just complete tutorials. I had a handful of small
projects already (a plagiarism checker, an NLP classifier, a PDF chatbot), but I
wanted a portfolio and a body of work that read as "this person builds things
that run," not "this person watched some courses." That was the bar I set for
myself in Week 1.

## What changed

The biggest shift wasn't a specific skill — it was how I debug. Early on, when
something broke, my instinct was to re-run it and hope, or search for the exact
error message and copy a fix. By the end of this track, my instinct changed to:
read the error carefully, isolate exactly which step failed, and understand *why*
before touching anything.

The clearest example of this was my NLP Text Classification project. I had three
models — SVM, Naive Bayes, and Logistic Regression — all stuck at a suspiciously
uniform ~63% accuracy. That uniformity was the clue: if three different algorithms
converge on the exact same number, the problem probably isn't the models, it's
upstream of them. I traced it back to a preprocessing bug, fixed it, and accuracy
jumped to 74–79% across all three. That taught me to treat a "boring" identical
result as a red flag, not a coincidence.

I hit a smaller version of the same lesson while building the demo video for this
capstone. My pipeline commands kept failing with confusing errors — `git clone`
throwing "too many arguments," then `pip install` failing right after. It looked
like three unrelated bugs. It was actually one bug: a Colab magic command (`%cd`)
had gotten combined onto the same line as `git clone`, so the clone itself never
ran cleanly, and everything downstream failed for the same root cause. Same
lesson as the NLP bug, different context: when multiple things fail together,
look for the one shared cause before debugging each symptom separately.

## The capstone itself

For the search-ranking capstone, I ran the full pipeline — feature prep, a
transparent hand-written baseline, three trained models evaluated on a
client-holdout split, and a final ranked review queue. The hand-written baseline
scored 0.240 on Precision@50. The trained random forest model scored 0.740 —
roughly three times better. Seeing that gap made something click that no
tutorial had: a simple, honest baseline isn't just a formality, it's what turns
"the model works" into "the model actually helps," with a real number attached.

## What I'd build next

I'd want to validate this pipeline against FlyRank's full-scale dataset instead
of just the anonymized sample — the honest limitation I flagged in my demo is
that 30,000 rows and 79 million rows might behave differently. I'd also want to
turn the plagiarism checker and the PDF chatbot from standalone projects into one
connected tool, since they solve adjacent problems (checking originality, then
answering questions about a document) and currently live in separate repos with
no shared interface.

## Three most transferable things I learned

1. **A uniform, "too clean" result is a signal, not a success** — whether it's
   three models converging on the same accuracy or three commands failing the
   same way, sameness usually points to one shared root cause upstream.
2. **A baseline isn't optional** — without the hand-written rule scoring 0.240,
   the random forest's 0.740 would just be a number with nothing to compare it
   to. Every model I build from now on gets a baseline first.
3. **Documentation is part of the build, not an afterthought** — writing the
   README forced me to explain my own architecture back to myself, and I caught
   gaps in my own understanding of the pipeline while writing it, before anyone
   else ever saw it.
