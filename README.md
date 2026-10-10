# Targeted Query-Free Attacks on Stable Diffusion

A from-scratch reproduction and analysis of **"A Pilot Study of Query-Free Adversarial Attack against Stable Diffusion"**
(Zhuang, Zhang & Liu, CVPRW 2023, [arXiv:2303.16378](https://arxiv.org/abs/2303.16378)), focused on the **targeted** attack:
removing one object from a generated image by appending a meaningless 5-character string to the prompt, without ever
querying the diffusion model.

> "A wooden cabin next to a lake **+6"dk**" → the lake disappears in 4 of 5 seeds; the cabin stays.

![Clean vs attacked, same seeds](results/figures/example_lake.png)

## Key results (Stable Diffusion v1.4, 6 two-object prompts × 5 seeds)

| Condition | Target erased (of eligible) | Kept object present |
| --- | --- | --- |
| Random 5 characters (baseline) | 4% (1/24) | 100% |
| PGD (+ discrete refinement) | 24% | 89% |
| Greedy | 32% | 86% |
| **Genetic (paper's best)** | **44% (11/25)** | 86% |
| Genetic, prefix placement | 12% | 79% |
| Genetic + preserve term (extension) | 20% | 97% |

"Eligible" means the clean image with the same seed actually contained the target. CLIP ViT-L/14 judge; the
independent ViT-B/32 judge agrees (42% erased with Genetic).

1. **The targeted attack works.** Genetic removes the target in 44% of eligible images, vs 4% for random (Fisher exact p ≈ 0.001).
   Averaged CLIP scores, as reported in the paper, understate this because the clean model often omits the target on its own.
2. **Genetic > Greedy > PGD > Random**, in text space and in image space.
3. **A suffix steers, a prefix destroys.** CLIP's causal attention mask leaves the prompt's own token states unchanged
   under a suffix (to within 1e-7), so the attack can only alter the tail positions. A prefix rewrites every token: stronger
   untargeted (−0.052 vs −0.018 CLIP score), but poor at removing one specific object.
4. **The preserve term (my extension) trades strength for precision:** every erasure it causes keeps the other object
   (97% retention), but it erases half as often.

The full write-up is in the report. The raw numbers are in [`results/`](results/).

## What is implemented

- Steerable key dimensions (n = 10 scenes, ε = 0.9) and the masked cosine objective on the full 77 × 768 hidden states
- Four optimizers: Random, Greedy, Genetic (50 generations × 20, mutation 0.3), and PGD on the probability simplex
  (100 steps, lr 0.1) with a gradient-guided discrete refinement step
- Extensions: prefix placement, and a preserve term that rewards keeping non-key dimensions unchanged
- Evaluation: CLIP score with two judges, image-to-image similarity, paired Wilcoxon tests, and a calibrated
  per-image object-presence test (threshold = 95th percentile on unrelated clean images)
- Analyses: per-position hidden-state similarity (causal mask), perturbation budget 1–10 characters, text-space vs
  image-space damage

## Run it

Open `qf_attack_stable_diffusion.ipynb` in **Colab** (Runtime → T4 GPU) or **Kaggle** (GPU, Internet on) and run all
cells. A full run takes about 50 minutes on a T4. Set `FAST = True` for a quick check (about 6 minutes). Outputs go to
`qf_results/`.

Colab note: the first cell uninstalls `torchao`, because the version Colab ships breaks recent `diffusers`. If you
hit `cannot import name 'FqnToConfig'`, restart the session after that cell.

## Repository layout

```
qf_attack_stable_diffusion.ipynb   notebook: attacks, image generation, evaluation, figures
results/
  targeted_attacks.csv             every suffix found, with key-dim / non-key / full cosines
  targeted_presence.csv            erase rates per condition and judge
  clip_scores.csv                  per-image CLIP scores (both judges)
  paired_tests.csv                 Wilcoxon tests vs no attack and vs random
  untargeted_attacks.csv, table_*.csv, report_draft.md
  figures/                         grids, causal-mask plot, budget ablation, examples
```

## Scope

This is an academic robustness evaluation of a public model using benign prompts. Stable Diffusion's safety checker
stays on, and flagged images (7 of 630) are excluded from scoring.

## Reference

```bibtex
@inproceedings{zhuang2023pilot,
  title     = {A Pilot Study of Query-Free Adversarial Attack against Stable Diffusion},
  author    = {Zhuang, Haomin and Zhang, Yihua and Liu, Sijia},
  booktitle = {CVPR Workshops},
  year      = {2023}
}
```
