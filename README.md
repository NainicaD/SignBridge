# SignBridge
ASL-to-foreign-language translation pipeline


## Baseline

Before setting final success-criteria thresholds (see Section 11 of the
project scoping doc), a baseline BLEU score is established using published
results from PoseStich-SLT (Exploration-Lab), an existing pose-based,
gloss-free sign language translation model benchmarked on How2Sign and iSign.

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
