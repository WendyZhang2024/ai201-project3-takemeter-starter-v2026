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
| 5. Agreement with staff labels | ≥ 20/30 | — | — | — | MET (28/30) |

Criterion 5 is a single measurement over all 30 staff posts, not one per seed,
so it carries no seed columns: 28/30 = 93%, from `agreement.py`.

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

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Overall accuracy | ≥ 0.65 | **MISSED** | 0.639 in all three seeds. The target needs 24 of 36; the model gets 23, which is exactly the number of `question` posts in the split. |
| 2 | Every label F1 | ≥ 0.50 | **MISSED** | `experience` and `resource` score 0.000 in every seed. Only `question` (0.780) clears the floor. |
| 3 | Macro F1 | ≥ 0.60 | **MISSED** | 0.260 in all three seeds, less than half the target. |
| 4 | Confidence gap | ≥ 0.15 | **MET** | 0.417 / 0.333 / 0.750, all above 0.15 — but see the note on this one below. |
| 5 | Agreement with staff | ≥ 20/30 | **MET** | 28/30 = 93%, from `agreement.py`. |

**Diagnoses**

**1. Overall accuracy — the model never answers anything but the majority
class.** The confusion matrix settles it: the `experience` and `resource`
columns are entirely zero, so every non-`question` post was answered
`question`. 0.639 is not a measurement of learning, it is the majority-class
share — 23 of 36 — and 23 is exactly how many `question` posts each stratified
split contains. The three seeds agree to three decimals for the same reason: the
class counts are fixed by stratification and the prediction is constant, so
there is nothing left to vary. The target needs 24 correct; the model reaches
23 by never trying.

Why this much data cannot reach it: 168 training rows, 3 epochs, a 65/25/11
class balance. The gradient that would teach `experience` versus `question` is
swamped by the gradient that teaches `question` every time.

**2. Every label F1 — two causes that compound.** First, too few examples:
`resource` has 26 rows in total and 4 in each test split, where one post moves
F1 by about 0.14; `experience` has 59 rows and 9 in test. Second, a boundary I
applied inconsistently, which is where the agreement report earns its place. I
labelled 2 of 30 posts by feel rather than by rule — `sp_005` downgraded despite
two load-bearing numbers, `sp_020` upgraded with no fact at all — and both went
against the definitions I was supposed to be applying. At that rate, roughly 16
of my own 240 rows carry a different reading of my own rule. The small labels
are exactly where those boundaries are hardest, so a labelling problem lands
hardest on the labels with the least data to absorb it. This is the one place
where the evidence says *my labels*, not just *my model*.

**3. Macro F1 — arithmetic, not a separate failure.** Macro F1 is the mean of
the three per-label scores. With `question` at 0.780 and the other two at 0.000,
the ceiling is 0.260. Fixing criterion 2 fixes this one; there is nothing
independent left to diagnose.

**A note on criterion 4, which passed for the wrong reason.** 0.417 / 0.333 /
0.750 clear 0.15 in every seed, so the verdict is MET. But the gap is not
calibration: mean confidence when the model is right is 0.699, and when it is
wrong 0.678 — it is no more sure when it is correct. Since every prediction is
`question`, the confidence thirds differ only in how many `question` posts each
happens to contain. The criterion passed; the thing it was written to detect — a
model that knows when it is unsure — is still absent.

**No criterion was revised.** All five could be measured with the numbers I
wrote in unit 5, so all five keep those numbers. A revision is only warranted
when a criterion cannot be measured, and none of mine had that problem: the
three misses are misses, not broken criteria.


## Agreement Report

**Agreement rate:** 28 / 30 = 93%

I labelled all 30 under the staff taxonomy before I opened
`data/staff_labels.csv`, and committed `my_staff_labels.csv` first (`84321ac`),
so the two sets are independent.

**Disagreements**

**1. `sp_005` — staff said `analysis`, I said `reaction`**

> *The post:* "Carbon plates aren't the whole story. My times dropped 2% in the
> shoes and another 2% the following year in the same pair, when the only thing
> that changed was going from 35 to 50 miles a week."

> *Staff said / I said:* `analysis` / `reaction`

> *My call, and why:* Staff is right, and rule 3 is the reason: immediacy does
> not outrank evidence. The post makes a general claim — carbon plates are not
> the whole story — and backs it with two measurements plus a controlled change
> in weekly mileage. The numbers carry the argument, which is exactly rule 1's
> test. I labelled it `reaction` because it is written in the first person, but
> first person appears nowhere in the definitions as a downgrade. What makes
> this a consistency problem rather than a one-off is that `sp_002`, `sp_007`,
> `sp_008` and `sp_010` have the same shape — *I measured X and got Y* — and I
> labelled all four `analysis`, which staff also agreed with.

> *Which pile:* **(a)** — the rule covered it; I applied it loosely and
> inconsistently.

**2. `sp_020` — staff said `hot_take`, I said `analysis`**

> *The post:* "Every injury thread on this forum is the same injury and the
> same three replies."

> *Staff said / I said:* `hot_take` / `analysis`

> *My call, and why:* Staff is right. `hot_take` is defined as a confident
> claim with no support offered, and this post offers no number, no example and
> no quote — "the same three replies" reads like a count, but none of the three
> is ever named. I treated "I have read a lot of threads and noticed a pattern"
> as if the pattern were evidence, but the definitions ask what support is in
> the post, not what the author might have seen. That is me reading generously,
> not the rule being unclear.

> *Which pile:* **(a)** — the rule covered it; I read it too generously.

**What the pattern in my disagreements tells me:**

Both errors come from the same shortcut: I classified by what the post *felt*
like instead of running rule 1's test. `sp_005` felt personal, so I downgraded
it even though two load-bearing numbers were sitting in it; `sp_020` felt like
an observation, so I upgraded it even though there was no fact at all. One
habit, pointing in two directions.

That matters for the diagnosis below. It is a labelling problem, not a model
problem — and at 2 in 30 (6.7%), the same rate applied to my own 240 rows would
mean roughly 16 of them were labelled by feel rather than by my own written
rule. A model cannot learn a boundary that its training data applies two ways.



---

## The Improvement

**What I changed:** one thing — the loss function. `WeightedTrainer` in notebook
section 4 uses a cross-entropy weighted by inverse class frequency:

    weight[c] = len(train) / (3 * count[c])   →   ≈ 0.52 / 1.36 / 3.08

for `question` / `experience` / `resource`. Nothing else moved: same 240 rows,
same three seeds, same 3 epochs, same learning rate, same tokenizer, same
`max_length`.

**Which diagnosis pointed at it:** criterion 2's. The confusion matrix had
all-zero columns for `experience` and `resource`, and the training set is
155/59/26 — a majority class worth 65% of the data. If the model is never
penalised more for missing a 26-row class than for missing a 155-row one, the
cheapest way to minimise loss is to predict the big class every time, which is
exactly what it did. I could not use the instruction's first option (add 30
examples to `resource`) because this repo has no pool of unlabelled posts, so
re-weighting the existing rows was the closest available version of that fix.

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.65 | 0.667 | 0.639 | 0.611 | MISSED |
| 2. Every label F1 (lowest label) | ≥ 0.50 | 0.000 | 0.000 | 0.000 | MISSED |
| 3. Macro F1 | ≥ 0.60 | 0.331 | 0.446 | 0.361 | MISSED |
| 4. Confidence gap (most-sure third − least-sure third) | ≥ 0.15 | 0.333 | 0.250 | 0.500 | MET |
| 5. Agreement with staff labels | ≥ 20/30 | — | — | — | MET (28/30) |

Before and after, side by side:

| Measure | Before (42 / 7 / 2024) | After (42 / 7 / 2024) |
|---|---|---|
| Overall accuracy | 0.639 / 0.639 / 0.639 | 0.667 / 0.639 / 0.611 |
| Macro F1 | 0.260 / 0.260 / 0.260 | 0.331 / 0.446 / 0.361 |
| F1 — `experience` | 0.000 / 0.000 / 0.000 | 0.200 / 0.600 / 0.000 |
| F1 — `resource` | 0.000 / 0.000 / 0.000 | 0.000 / 0.000 / 0.333 |
| Spread in accuracy | 0.000 | 0.056 |
| Spread in macro F1 | 0.000 | 0.115 |

**Did it help, and how do I know:**

Partly — and the part it did not help is the more interesting half.

It helped where the diagnosis pointed. Macro F1 rose from 0.260 in every seed to
0.331 / 0.446 / 0.361, a mean of 0.379, which is +0.119. The all-zero columns
are gone: seed 42 now gets 1 of its 9 `experience` posts right, and seed 2024
gets 1 of its 4 `resource` posts. The collapse to the majority class is broken.
The model is willing to predict a small label now, which is exactly what the
weighting was for.

It did not move a single criterion to MET. Accuracy lands at 0.611–0.667, so
criterion 1 still fails two of the three seeds; `experience` and `resource` are
still under 0.50 in at least two seeds, so criterion 2 fails; macro F1 tops out
at 0.446 against a 0.60 target.

And it cost stability. Before, the three seeds agreed to three decimals; now
macro F1 spans 0.115 across seeds, past the notebook's own threshold for "too
unstable to call". That is not noise the weighting added — it is noise the
weighting exposed. With 26 `resource` rows, four of them in a test split, how
much a 3× class weight helps depends on which four posts the seed drew. The
before numbers looked stable only because a constant prediction cannot vary.

So the diagnosis had the right mechanism, the change attacked the right thing,
and the ceiling was never the loss function: it is 26 labelled examples. The
next real move is more `resource` data, not another training setting.

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
