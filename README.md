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

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->

TakeMeter sorts posts from r/LetsTalkMusic, a public subreddit for long-form discussion of music across all genres and eras. Each post gets one of two labels based on what it asks the reader to think about: `the_music` (the sound, songs, albums, or an artist's work) or `the_business` (the industry, streaming platforms, labels, careers, or drama). The split matters because a big share of the community's discussion is about everything around the music rather than the music itself, and many posts mention both. The classifier is fine-tuned on about 200 hand-labelled posts.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `the_music`

**Definition:**
The post is about the music itself: the sound, songs, albums, or an artist's work.

**Example 1:**
>Title: "Where have all the epic melodies gone?"
- discusses sound, and how it changed over time

**Example 2:**
> Title: "Critically acclaimed albums you don't like by artists you love"
- about specific albums and how they sound to the listener

### `the_business`

**Definition:**
The post is about what surrounds the music: the industry, streaming platforms, labels, careers, or drama.

**Example 1:**
>Title: "It's actually pretty shocking how much the major music industry downsized in 20 years."
- industry, pitfalls, lost artists

**Example 2:**
>Title: "If you worked in specific music divisions and A&R for major labels, what happened to them?"
- businesses and what happened to them


### The hardest boundary

**Which two labels:** `the_music` and `the_business`

**The decision rule I used every time:**
Label by what the post asks the reader to think about. If it mentions an album or artist but the question is about money, fame, platforms, or drama, it's `the_business`. If it mentions the industry but the point is how the music sounds, it's `the_music`.



---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:**
I scraped the posts from Arctic Shift which is a reddit post archive.

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Cold first, Pre-labelled with AI and corrected.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `the_music` | 135 | 61% |
| `the_business` | 85 | 39% |
| **Total** | 220 | 100% |

This is not how I originally had through the distribution would be, and therefore I have adjsuted my criteria.

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* "Has streaming made listeners less emotionally attached to individual albums?" The writer grew up listening to albums completely and asks whether streaming has changed that connection for everyone.
>
> *Could have been:* `the_music` or `the_business`
>
> *I chose, because:* `the_business`. It's about albums and listening, but the question it asks is what a platform has done to listeners in general. Its subject is streaming's effect, not the music itself.

**2.**
> *The post:* "Hooked on an Album." It opens with "Streaming services have robbed us of the joy of falling in love with an album," then spends the rest of the post on how the writer comes to love a particular album.
>
> *Could have been:* `the_business` or `the_music`
>
> *I chose, because:* `the_music`. It reads almost like case 1, but after the opening line, streaming drops out and the post is about the writer's own experience of an album. These two posts are where my decision rule matters most: the platform's effect on everyone is business, and one person's listening is music.

**3.**
> *The post:* "Do you think iconic songwriters ever secretly use uncredited songwriters to improve their work?" It uses Alex Turner and *AM* as the example.
>
> *Could have been:* `the_music` or `the_business`
>
> *I chose, because:* `the_business`. It talks about songwriting, but the question is whether the industry hides who really writes the songs. A similar post asking whether artists *lose something artistically* when they use co-writers went to `the_music`, because that one is about how the songs come out.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Trained on:** Apple Silicon GPU (MPS), torch 2.14.1, recorded in `results.json`.

**Settings:** <!-- epochs, learning rate, batch size, seed -->
3 epochs, learning rate 2e-5, batch size 16, seed 42, max length 128 tokens.

**Anything I changed from the defaults, and why:**
Nothing. This is the default run.

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->

| | Train | Val | Test |
|---|---|---|---|
| `the_music` | 95 | 20 | 20 |
| `the_business` | 59 | 13 | 13 |
| **Total** | 154 | 33 | 33 |

Both labels have more than 8 posts in the test split.



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:*
Asked AI to look at my 20 cold labels and write a label for the rest of my examples.
- *What came back:*
Good, but not perfect labels.
- *What I changed:*
Out of 200 I changed about 15 labels.

**Moment 2**

- *What I asked for:*
Asked AI to count the number of labels for me
- *What came back:*
It did so successfully and gave me the number of each label
- *What I changed:*
My criterion to match, since my assumptions was the business label would be higher.

**Pre-labelling disclosure:**

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained



<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.697 | 0.606 | -0.091 |
| Macro F1 | 0.659 | 0.377 | -0.282 |
| F1 — `the_business` | 0.545 | 0.000 | -0.545 |
| F1 — `the_music` | 0.773 | 0.755 | -0.018 |

**What I predicted before I looked:**
I predict the trained model will not do better than the baseline. Not sure why, but I have a feeling I broke something. 

**What the gap actually means:**
The baseline beat my trained model on every measure, so my fine-tuning added nothing. It made things worse. The trained model never once predicted the_business: all 13 business posts were called music, and its 0.606 accuracy is exactly what always guessing music would score.



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.75 | 0.606 | 0.606 | 0.606 | MISSED |
| 2. Macro F1 | ≥ 0.70 | 0.377 | 0.377 | 0.377 | MISSED |
| 3. F1 — the_business | ≥ 0.65 | 0.000 | 0.000 | 0.000 | MISSED |
| 4. F1 — the_music | ≥ 0.80 | 0.755 | 0.755 | 0.755 | MISSED |
| 5. Confidence gap | ≥ 0.15 | 0.545 (0.909 − 0.364) | 0.364 (0.727 − 0.364) | 0.545 (0.818 − 0.273) | MET |


### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted | the_music | the_business |
|---|---|---|
| **the_music** | 20 | 0 |
| **the_business** | 13 | 0 |

**My biggest off-diagonal number, and what it means:**
13: all 13 real the_business posts in the test split were called the_music, and 0 went the other way. The model never learned the boundary from the business side. It treated every post as music, so its 20 "correct" music predictions don't show it recognized music either. 


---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Overall accuracy | ≥ 0.75 | MISSED | 0.606 on all three seedsalways guessing `the_music` scores (20 of 33). |
| 2 | Macro F1 | ≥ 0.70 | MISSED | 0.377 on all three seeds |
| 3 | F1 — `the_business` | ≥ 0.65 | MISSED | 0.000 on all three seeds. The model never predicted `the_business` once. |
| 4 | F1 — `the_music` | ≥ 0.80 | MISSED | 0.755 on all three seeds. It caught every music post only because it called everything music. |
| 5 | Confidence gap | ≥ 0.15 | MET | Gaps of 0.545, 0.364 and 0.545, all above 0.15. Met on paper, but every prediction was `the_music`. |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->

Criteria 1-4 all failed because the model just always guesses the_music. All 13 business posts in the test split were called `the_music` and none went the other way. The four misses aren't four separate problems.

It is a model problem because, I matched staff on 30 of 30 posts under a taxonomy I didn't write, including the posts staff picked because they're ambiguous. When I'm given a written rule, I apply it consistently. That makes "a boundary I applied inconsistently" an unlikely main cause.

The zero-shot model, given only my two definitions, reached 0.545 F1 on `the_business`. My trained model got 0.000. If my business labels were incoherent, a model reading the definition cold shouldn't be able to find them either. So the label is learnable, and my trained model didn't learn it.

`MAX_LENGTH` is 128 tokens, but my median post is about 970 characters, so the model sees only the opening of most posts. Many of my business posts only turn to the industry or streaming after an opening about an album. On top of that,`the_business` has 59 training posts against 95 for music. With so little training, always guessing music is the safest strategy, and that's what the model found.

Criterion 5 was met, but only because the model's confidence was higher on music posts. With no business predictions, it doesn't show useful confidence yet.



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** 30 / 30 = 100%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**
This sections is N/A because I scored correctly.
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
