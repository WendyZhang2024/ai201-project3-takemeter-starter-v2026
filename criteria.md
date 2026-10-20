# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1. Overall accuracy is above the majority class

**Criterion:** Overall accuracy on the held-out test split is at least 0.65, in each of the three seeds.

**Why this target:** In the 20 posts I read, 10 were `question` — about half. If that share holds in my 200, a model that always predicts `question` already scores around 0.50. 0.65 is 15 points above that, so it cannot be reached by learning the majority label alone.

---

## 2. Every label clears a floor

**Criterion:** Every label has an F1 of at least 0.50 on the test split, in all three seeds.

**Why this target:** `experience` and `resource` are about a quarter of my posts each, so roughly 50 examples each; a 15% test split leaves only 7–8 posts per label. On 7 posts every single error moves F1 by about 0.14, so 0.50 is the floor I can ask of the two small labels. Asking for the same score as `question` would be measuring the split, not the model.

---

## 3. All three labels work together

**Criterion:** Macro F1 on the test split is at least 0.60, in all three seeds.

**Why this target:** Accuracy is dominated by `question`. Macro F1 weights the three labels equally, so it drops if the model quietly ignores `experience` or `resource` while still getting `question` right. 0.60 is the point where all three labels have to be working, not just the big one.

---

## 4. Confidence separates the sure from the unsure

**Criterion:** Accuracy on the most-confident third of the test split exceeds accuracy on the least-confident third by at least 0.15, in all three seeds.

**Why this target:** `results.json` already reports both thirds. With about 30 test posts, each third holds roughly 10, so a 0.15 gap is about one and a half posts — one lucky post would not create it, and a model that is genuinely better when it is sure clears it. A smaller gap would be indistinguishable from noise.

---

## 5. My labels agree with the staff labels

**Criterion:** My agreement with the staff labels is at least 20/30 (67%).

**Why this target:** This measures me, not the model, and it is the only evidence I have that my own 200 labels mean the same thing on every post. The 30 staff posts were chosen partly because they are ambiguous, and I am applying someone else's taxonomy (`analysis` / `hot_take` / `reaction`) rather than my three labels, so I set the bar below what I would want for my own set.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
