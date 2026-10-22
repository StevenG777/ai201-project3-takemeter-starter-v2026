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

## 1.

**Overall accuracy on the held-out test split is at least 0.50 on each of the three seeds.** (Accuracy = correct predictions ÷ all test posts.)

**Why this target:**
I picked 0.5 because 5 wrong out of 10 is the most I'd accept for a feed filter. That is 2× chance for 4 labels (0.25). I didn't go higher because my hardest boundary, `analysis` vs `hot_take`, needed a written decision rule, so I don't expect near-perfect accuracy, and the test set is only about 30 posts.

---

## 2.

**On the held-out test split, on each of the three seeds, recall is greater than 0.50 for `other` and `reaction`, and at least 0.40 for `analysis` and `hot_take`.** (Recall of a label = correct predictions of that label ÷ test posts whose true label is that label.)

**Why this target:**
For `other` and `reaction` I want recall above 0.50, meaning the model finds more than half of that label's posts, which is better than a coin flip per label. A fraction works whatever the test count per label turns out to be. I set the floor lower, at 0.40, for `analysis` and `hot_take` because that is the boundary I needed a written decision rule for, so I expect the model to confuse those two more. 0.25 is chance for 4 labels, so 0.40 still shows it learned something. Each label has few test posts, so one post can move its recall a lot on a single seed.

---

## 3.

**In `labels.csv`, no label makes up more than 49% of the rows, and the smallest label makes up at least 17% of the rows.**

**Why this target:**
The assignment already caps any label at 70%, but at that cap the other three labels would share 30%, about 10% each, which is only about 3 posts per label in a test set of about 30. That is too few for the recall targets in criterion 2 to mean anything, because 1 right out of 3 could easily be luck. I want at least about 5 test posts for the smallest label, which is 5 ÷ 30, or about 17% of the file. With 5 posts, 0.40 recall is 2 right and more than 0.50 recall is 3 right. 49% is the largest ceiling that still leaves room for every other label to reach 17% (49 + 3 × 17 = 100). Fewer than 5 is too coarse: with 3 test posts one post is a third of a label's recall, so a single post could flip pass to fail. Even at 5 test posts, 2 right out of 5 could still be luck, but it is the least I'd accept for this assignment.

---

## 4.

**On the 30 staff posts, labelled under the staff's written definitions, my labels match the staff labels on at least 21 of 30 posts (70%).**

**Why this target:**
Deciding what a comment's main claim is, and then whether its facts actually carry that claim, depends on how closely I read the English, so two careful readers can differ on the same post. The staff taxonomy says staff chose several of these posts because they are genuinely ambiguous, and that a rate in the 70s is normal and expected. I picked 70% to align with that stated staff standard. It is also well above the 25% I would get by chance with 4 labels. Each post is about 3 points, so 21 of 30 is the lowest count that reaches 70%.

---

## 5.

**On the held-out test split, on each of the three seeds, the fine-tuned model's macro F1 is at least 15 percentage points higher than the baseline's macro F1, with both scored on the same held-out posts.** (Macro F1 = the average of the four per-label F1 scores, so each label counts equally. "15 percentage points" means 0.15, e.g. 0.50 to 0.65.)

**Why this target:**
I want to successfully fine-tune this model, and for me success means the fine-tuned model beats the zero-shot baseline on the same metric by a clear threshold. If it only matches the baseline, the training added nothing. I chose macro F1 so a label with few posts counts as much as a big one. I picked 15 points because the test set is small, about 30 posts, so a few posts can move a score by a few points, and I want a gap that is bigger than that noise. This is the criterion I would be most disappointed to miss.

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
