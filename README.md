# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

r/piano is a public forum where players of every level ask for help, report what
they are practising, and share things they have made. This classifier sorts a post
into one of three kinds by what the post is *doing*, not by what it is about:
`question` (the author wants an answer and commits to no conclusion of their own),
`experience` (the author reports what happened to them and what they concluded),
and `resource` (the author hands the reader something usable — a file, a link, a
setting, or an explicit procedure). The distinction matters here because a post
that asks something and a post that tells you something need different replies,
and the two often look identical from the title.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `question`

**Definition:** a post whose main work is to get an answer from other people; it lays out a problem or a choice but does not conclude anything itself.

**Example 1:**
> **"ROLAND FP-30X vs Yamaha P-225 vs something else?"** — Digital Piano Question
>
> "I want to buy my first digital piano. I played an acoustic piano many years ago,
> and now I want to return to my hobby. I'll be using it in an apartment, so I need
> both a headphone output and on-board speakers. … Is there anything else in the
> $500–1000 range I should look at, or what is the best from these options?"
>
> https://www.reddit.com/r/piano/comments/1mh66fq/roland_fp30x_vs_yamaha_p225_vs_something_else/

**Example 2:**
> **"Tension in this spot in Ballade 3"** — Question/Help (Intermed./Advanced)
>
> "My upper arm seems to always tense up in this spot. Interestingly though, I
> experience almost no tension in the spot right before this, where the RH is
> jumping all over the place. What would you do to try to fix this?"
> *(the post also contains a photo of the passage)*
>
> https://www.reddit.com/r/piano/comments/1x99yrv/tension_in_this_spot_in_ballade_3/

### `experience`

**Definition:** a post whose main work is to report what happened to the author and what they concluded from it; it does not hand the reader something they can act on directly.

**Example 1:**
> **"Playing by ear is so freeing compared to learning with sheet music"**
> — Let's Discuss This
>
> "Two months ago I got a new piano, and I decided that I would try again at this
> playing by ear thing. I started with using the chords as a guide then eventually
> moving to just using my ears. … Maybe after around 1 to 2 minutes I can play the
> chorus of whatever song I like! Compared to before when I would spend at least
> 5 min looking for the sheets…"
>
> https://www.reddit.com/r/piano/comments/1wtzna5/playing_by_ear_is_so_freeing_compared_to_learning/

**Example 2:**
> **"New experience"** — Discussion
>
> "From then i had only one thing in mind: to learn at least one song from the
> start to the end. … And after a couple of month of dedication daily i actually
> just been able to play it from the start to the end … I just feel So blessed to
> accieve this little step and i wanted to share it with someone."
>
> https://www.reddit.com/r/piano/comments/13hnfo9/new_experience/

### `resource`

**Definition:** a post whose main work is to hand the reader something they can act on directly — a file, a link, a setting, a checklist, or an explicit procedure; it does not mainly narrate the author's own experience.

**Example 1:**
> **"I transposed Christian Schäfer's Sight Reading Exercises (Op. 45) into all 12 keys"**
> — Useful Resource
>
> "It always annoyed me with most sight-reading and method books that beginner
> pieces are almost exclusively in C, G, or F. … I took Christian Schäfer's Op. 45
> sight-reading exercises and re-engraved it, putting every piece through all 12
> keys. … It's completely free, no email sign-up or paywall, just direct PDF links."
>
> https://www.reddit.com/r/piano/comments/1x9q9yz/

**Example 2:**
> **"Piano repertoire spreadsheet template"** — Resource
>
> "Hello! I made a spreadsheet for my repertoire and wanted to share it with you
> all. … It includes 235 pieces aproximately within the range of Grade 3 ABRSM …
> You have a link to all the sheet music. That's it! Enjoy."
>
> *(the post ends with "I would love to know what you think about it" — a closing
> invitation, not a request for information; see the decision rule)*
>
> https://www.reddit.com/r/piano/comments/172y0dp/piano_repertoire_spreadsheet_template/

### The hardest boundary

**Which two labels:** `resource` vs `question`

**The decision rule I used every time:** If a post hands over something usable
**only as background for a question it is actually asking**, it is `question` —
the author wants an answer and the file is evidence. If the thing is the point
and the question is only a closing invitation ("let me know what you think"),
it is `resource`.



---

## The Dataset

**Where the posts came from:** r/piano, the week of 13–20 October 2026. I kept only posts with a real text body of at least 200 characters, and dropped
stickied, removed and NSFW posts. 

**How I labelled them:** All 240 by hand, using the three definitions above and the
decision rule. The 20 `cold` rows were labelled first, with no tools, no AI and no
second opinion; they are marked `cold` in the `note` column. The remaining 220 are
marked `manual`. I did not use AI to pre-label any post.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `question` | 155 | 64.6% |
| `experience` | 59 | 24.6% |
| `resource` | 26 | 10.8% |
| **Total** | 240 | 100% |

`question` is under the 70% cap but is still the majority class: a model that
always predicts it scores 64.6% accuracy without learning anything.

**Three hard cases**

**1.**
> *The post:* "Blind Listening Exercise #2" — seven recordings of the Goldberg
> Variation 3 with timestamps, ending with "I want to hear your thoughts,
> critiques, and pianist guesses in the comments".
>
> *Could have been:* `resource` or `question`.
>
> *I chose, because:* the exercise is the thing being handed over — the
> recordings and the timestamps are usable without the author. "I want to hear
> your thoughts" is a closing invitation, not a request for information the
> author lacks, so the decision rule keeps it as `resource`.

**2.**
> *The post:* "How do you recover from a bad performance/exam?" — a long report
> of an exam (73/100, a disappointed jury, "problematic passages") that ends
> with "I don't know how I can put even more energy into this".
>
> *Could have been:* `question` or `experience`.
>
> *I chose, because:* the exam story exists to support the question in the
> title; the author reaches no conclusion of their own and is asking how to
> recover. The main work is the question, so it is `question`.

**3.**
> *The post:* "Listing old (bad) rep in college app?" — ten years of playing
> history, a change of teacher, and a closing request for what conservatory
> applicants would do about listing old repertoire.
>
> *Could have been:* `experience` or `question`.
>
> *I chose, because:* the body is almost entirely the author's history and what
> changed in the past year, and the questions are about the author's own
> situation at the end. The main work is the report, so it is `experience`, even
> though the title is a question.

---

## The Training Run


**Base model:** `distilbert-base-uncased` (Hugging Face), fine-tuned for 3-way
sequence classification.

**Settings:** 3 epochs, learning rate 2e-5, batch size 16, max length 128 tokens,
seed 42. Trained on `cpu` (torch 2.14.1+cpu).

**Anything I changed from the defaults, and why:** Nothing. This is the notebook's
default configuration, unchanged.

**Split sizes:** 168 train / 36 validation / 36 test (70/15/15, stratified). Per
label in the test split: `question` 23, `experience` 9, `resource` 4. `resource`
is below the ~8 the brief warns about, so on four test posts a single error moves
its F1 by about 0.25.

The run completed and wrote `results.json` and `test_split.csv`: accuracy 0.639,
macro F1 0.260. The model predicted `question` for all 36 test posts.

---

## How I Used AI

**Moment 1**

- *What I asked for:* "Here are my three label definitions for r/piano posts.
  Write eight posts that sit exactly on the boundaries between them. Don't label them."
- *What came back:* one post handed over a spreadsheet and ended with "I would love
  to know what you think about it". Under my original rule — handed over something
  + asked a question → `question` — that post was a `question`, which felt wrong.
- *What I changed:* rewrote the decision rule. The artifact has to be *background
  for a real question* to count as `question`; if the artifact is the point and the
  question is a closing invitation, it stays `resource`.

**Moment 2**

- *What I asked for:* "Here are my five acceptance criteria. For each one, say
  whether a reader who never spoke to me could take the measurement and check the
  outcome."
- *What came back:* the targets were not checkable as written. They did not say
  which split the number is measured on, or that it has to hold in every seed, so
  "accuracy at least 0.65" could be read as the validation split, the test split,
  or an average of the two.
- *What I changed:* every criterion now names the held-out test split and "in each
  of the three seeds", and the accuracy target states the 0.50 majority-class
  baseline it is measured against.

**Pre-labelling disclosure:** None. I did not use AI to pre-label any post. All 240
rows were labelled by hand; the 20 rows I labelled first, with no tools, are marked
`cold` in `labels.csv`, and the remaining 220 are marked `manual`.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.194 | 0.639 | +0.445 |
| Macro F1 | 0.149 | 0.260 | +0.111 |
| F1 — `question` | 0.267 | 0.780 | +0.513 |
| F1 — `experience` | 0.000 | 0.000 | +0.000 |
| F1 — `resource` | 0.182 | 0.000 | −0.182 |

**What I predicted before I looked:**

I expect the baseline to beat my trained model on `experience` and `resource`,
and to lose on overall accuracy. My unit 5 run predicted `question` for all 36
test posts and scored F1 0.00 on the other two labels, so *any* baseline that
ever answers `experience` or `resource` correctly wins those two columns
outright. The interesting number is accuracy: mine was 0.639, so if the
baseline lands near that too, my fine-tuning learned nothing beyond the
majority class.

*Written before running `python baseline.py`, so it is a prediction rather than
a summary.*

**What the gap actually means:**

Half my prediction held. The baseline did win `resource` (0.182 against 0.000)
and did lose overall accuracy, exactly as I wrote. What I got wrong is
`experience`: I expected the baseline to win that column too, and both models
scored 0.000 — neither ever predicts it.

But the shape of the gap is not "trained model good, baseline bad". Over the
same 36 posts:

| Strategy | Accuracy |
|---|---|
| Always `question` (the majority class) | 0.639 |
| Always `experience` | 0.250 |
| Always `resource` | 0.111 |
| **The baseline** | **0.194** |

The baseline labelled 29 of 36 posts `resource` — the smallest class, 4 posts —
and never once said `experience`. So it collapsed, but collapsed toward the
*rarest* label, which is why it scores below every lazy strategy except
always-`resource` itself. The trained model also collapsed, but toward the
*majority* label, which is the cheaper and rational way to fail.

That reframes what fine-tuning bought. It bought 0.445 of accuracy and 0.513 of
`question` F1, and essentially nothing on the two labels that carry the
taxonomy's actual distinctions: `experience` is 0.000 for both models, and the
only reason `resource` looks better for the baseline is that 4 posts make F1
move in jumps of 0.14. The honest reading is that 200 examples did not teach
the smallest labels, and that my baseline is a weak opponent because its prompt
pointed it at the wrong class.

I think the prompt is why. `resource` has the longest and most concrete
definition — "a file, a link, a setting, a checklist, or an explicit
procedure" — and a general model given three definitions of unequal
concreteness will drift toward the one that looks like a checklist item. It
found artefacts in plain questions. A prompt that describes one label more
tangibly than the others is not neutral, and that is the first thing I would
measure next: the same baseline with bare label names and no definitions at
all, to see whether my definitions helped or hurt.




---

## Run Log — Before

Notebook section 6, run once across three seeds (`42`, `7`, `2024`) on CPU.
Each column is one seed; the target comes from `criteria.md` and has to hold in
all three.

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.65 | 0.639 | 0.639 | 0.639 | MISSED |
| 2. Every label F1 (lowest label) | ≥ 0.50 | 0.000 | 0.000 | 0.000 | MISSED |
| 3. Macro F1 | ≥ 0.60 | 0.260 | 0.260 | 0.260 | MISSED |
| 4. Confidence gap (most-sure third − least-sure third) | ≥ 0.15 | 0.417 | 0.333 | 0.750 | MET |
| 5. Agreement with staff labels | ≥ 20/30 | — | — | — | pending (milestone 3) |

Criterion 2 is reported as that seed's **lowest** per-label F1, because the
criterion says *every* label: `question` alone reaches 0.780, `experience` and
`resource` are both 0.000, so the seed scores 0.000.

The three seeds produced identical numbers (spread 0.000), and that is not
stability. `split_dataset()` stratifies, so all three test splits hold exactly
23 `question` / 9 `experience` / 4 `resource` posts even though the posts
themselves differ — seed 42 and seed 7 share only 5 of their 36 posts. A model
that answers `question` every time therefore scores 23/36 in every seed. The
zero spread measures the stratification, not the model.

### Confusion matrix

Seed 42 — seeds 7 and 2024 produce the identical matrix, for the reason above.

| true \ predicted | question | experience | resource |
|---|---|---|---|
| **question** | 23 | 0 | 0 |
| **experience** | 9 | 0 | 0 |
| **resource** | 4 | 0 | 0 |

Device: `cpu` (CPU only), torch `2.14.1+cpu`, `max_length` 128.

**My biggest off-diagonal number, and what it means:**

9 real `experience` posts were labelled `question`, and 0 went the other way.
The whole `experience` and `resource` columns are zero, so every non-`question`
post was answered `question`. That is not a boundary the model learned badly —
it is a boundary the model never predicted at all, and it only ever errs in one
direction, toward the majority label.



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
