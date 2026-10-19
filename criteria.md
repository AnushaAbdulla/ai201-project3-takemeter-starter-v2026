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

## 1. Overall accuracy is at least 0.75 on the test split, on all three seeds.

**Why this target:**
I expected about 60% of my posts to be about `the_business`, but after labelling 220 posts the split came out the other way: 61% `the_music`, 39% `the_business`. So a model that always guesses music already scores about 0.61. The test split is about 33 posts, so 0.75 means getting roughly 4 to 5 more posts right than that lazy guess.

---

## 2. Macro F1 is at least 0.70 on the test split, on all three seeds.

**Why this target:**
Accuracy alone can hide a model that guesses `the_music` because it's the bigger label. Macro F1 weighs both labels equally, so it only passes if the model is actually telling music posts apart from business posts. I set it a little below accuracy because the smaller label will drag it down.

---

## 3. `the_business` has an F1 of at least 0.65 on the test split, on all three seeds.

**Why this target:**
`the_business` is my smaller label, 85 of 220 posts (39%), so only about 13 of them land in a 33-post test split. One wrong prediction moves its F1 by several points between seeds, so the target is looser than for the overall numbers. It's still above what guessing would get.

---

## 4. `the_music` has an F1 of at least 0.80 on the test split, on all three seeds.

**Why this target:**
`the_music` is my bigger label, 135 of 220 posts (61%) and about 20 in the test split, so it has the most training examples and the steadiest score. It should be the easiest thing the model does. If it can't reach 0.80 on music, the problem is probably my labels and not the amount of data, because this label has no shortage of examples.

---

## 5. The model's most-confident third of test posts is at least 0.15 more accurate than its least-confident third, on all three seeds.

**Why this target:**
Many r/LetsTalkMusic posts mention both an album and the industry, so I expect the model to be unsure on exactly those mixed posts and sure on the clear ones. Each third is only about 10 posts, so a gap of 0.10 would be a single post and could happen by chance. At 0.15, the model's confidence has to actually mean something.

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
