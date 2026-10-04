# SignBridge

**ASL-to-Foreign-Language Translation for Deaf and Mute Travelers**

SignBridge is a pipeline that converts American Sign Language (ASL) video
into spoken or written output in a language other than English — for
example, ASL in, spoken Mandarin out — so that a Deaf or mute ASL-only
signer can communicate directly with someone they share no common language
with, such as while traveling abroad.

Existing sign language tools stop at English (e.g. Google's SL2T on
Pixel 11). SignBridge extends translation to additional spoken languages,
initially **Mandarin and Hindi**, and adds a reply path so the signer can
also understand a spoken response, returned to them as English text.

This project is, and will remain, **free, open-source, and non-commercial**
— no paid tier, no monetization, no investment path. This isn't just a
mission choice: it's a condition of the dataset licenses below.

## Status

Early development / project scoping stage. See `/docs` for the full
project scoping document and architecture notes.

## How it works
ASL video → Pose extraction → Sign recognition → Gloss → English
→ [signer confirms?] → English → target language → spoken output
Spoken reply → Speech-to-text → Translation → English text (back to signer)


1. **MediaPipe Holistic** extracts hand, body, and face keypoints from
   raw video — no pixel data passed downstream.
2. **Sign2Gloss** — a pose-based model converts the keypoint sequence into
   a gloss sequence.
3. **Gloss2Text** — a fine-tuned seq2seq model turns gloss into fluent
   English.
4. **Translation + speech output** — an open-source, self-hostable
   translation model (NLLB-200 / Argos Translate) translates the English
   into the target language, followed by on-device text-to-speech.
5. A **signer confirmation step** and confidence-based text fallback
   reduce the risk of a confidently wrong translation in a travel
   situation where errors matter.

## Datasets

| Dataset | Role | License |
|---|---|---|
| [How2Sign](https://how2sign.github.io/) | Primary training corpus — continuous ASL video with English transcripts and gloss | CC BY-NC 4.0 |
| [ASL-LEX](http://asl-lex.org/) | Supplementary lexicon (~2,700 signs) for sign disambiguation | CC BY-NC 4.0 |

**Both datasets are licensed for non-commercial use only.** Per Creative
Commons' own compliance guidance, satisfying that condition requires that
*no stage* of this project — training through distribution — be for
commercial gain. SignBridge is built around that constraint from the
start, not as an afterthought.

Raw video from either dataset is **not redistributed in this repository**.
Download each dataset directly from the links above after agreeing to
their usage terms; see `/docs` for download and preprocessing instructions.

## Installation

```bash
git clone https://github.com/NainicaD/SignBridge.git
cd SignBridge
pip install -r requirements.txt
```

Dataset download and preprocessing instructions are in `/docs` — raw
video is not included in this repo and must be obtained directly from
How2Sign and ASL-LEX (see above).

## Repository structure
/data — not committed; see /docs for download instructions
/src/preprocessing — MediaPipe extraction, normalization, alignment
/src/models/sign2gloss — pose model code + training scripts
/src/models/gloss2text — fine-tuning scripts for the text generation model
/src/pipeline — end-to-end inference pipeline
/notebooks — exploratory analysis and evaluation
/docs — project scoping document, architecture notes, data cards
/app — mobile/web demo app
README.md, LICENSE, requirements.txt

## Baseline

Before setting final success-criteria thresholds (see the project
scoping doc, Section 11), a baseline BLEU score is established using
published results from PoseStich-SLT (Exploration-Lab), an existing
pose-based, gloss-free sign language translation model benchmarked on
How2Sign and iSign.

- Source: https://github.com/Exploration-Lab/PoseStich-SLT
- Branch used for this work: `baseline-evaluation`
- Baseline results on How2Sign test set (cited from the paper, not
  locally reproduced): GloFE BLEU-4 = 1.97, PoseStich-SLT BLEU-4 = 4.56
- Reproduction note: cloning and running PoseStich-SLT's own evaluation
  failed because `IncrementalPT_linear_sch.py` hardcodes a path to an
  unreleased internal file (`/DATA3/vaibhav/isign/PretrainingISL/train_MT16M.csv`).
  The published numbers above are used as the baseline reference instead.
  A self-run baseline using How2Sign's own official repo
  (imatge-upc/slt_how2sign_wicv2023) is a candidate for future work.

## Roadmap

| Phase | Focus | Approx. Time |
|---|---|---|
| 1 | Data acquisition, preprocessing, MediaPipe keypoint extraction | Weeks 1–3 |
| 2 | Sign2Gloss model training and evaluation | Weeks 4–6 |
| 3 | Gloss2Text fine-tuning + target-language MT integration | Weeks 7–9 |
| 4 | TTS integration, end-to-end pipeline, mobile app shell | Weeks 10–11 |
| 5 | Evaluation, user study, documentation, final write-up | Weeks 12–14 |

## Intended use

SignBridge is a **communication aid**, not a certified interpreter
replacement. It should not be relied upon in safety-critical or legal
contexts (medical, legal) without a qualified human interpreter.

## License

This project is and will remain free and open-source, with no
monetization, paid tier, or investment path — ever — consistent with
the non-commercial license terms of How2Sign and ASL-LEX.

## Team

Nainica Dasari · Hitika Dalwadi · Nandita Mahendra · Abhishek Pramanik · Banu Teja Maram · Satwik Golla
