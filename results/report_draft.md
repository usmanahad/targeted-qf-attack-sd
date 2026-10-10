# Query-free attack on CompVis/stable-diffusion-v1-4 (12 untargeted + 6 targeted prompts, 5 seeds, 20 steps)

Objective on `sequence` embedding; 5-char perturbations from 68 characters.

## Text-space attack strength (untargeted)

| position   | method   |   cos_mean |   cos_std |   seconds |   encoder_queries |
|:-----------|:---------|-----------:|----------:|----------:|------------------:|
| prefix     | genetic  |      0.536 |     0.075 |     2.246 |           442.750 |
| prefix     | greedy   |      0.576 |     0.097 |     1.527 |           340.000 |
| suffix     | genetic  |      0.620 |     0.057 |     2.209 |           442.750 |
| suffix     | greedy   |      0.650 |     0.059 |     1.509 |           340.000 |
| suffix     | pgd      |      0.694 |     0.075 |     2.222 |           198.417 |
| suffix     | random   |      0.855 |     0.023 |     0.027 |             1.000 |

## Text-space (targeted)

| condition        |   masked_cos |   non_key_cos |   full_cos |   key_dims |   seconds |
|:-----------------|-------------:|--------------:|-----------:|-----------:|----------:|
| genetic          |        0.619 |         0.554 |      0.576 |    228.500 |     2.348 |
| genetic_prefix   |        0.572 |         0.545 |      0.555 |    228.500 |     2.453 |
| genetic_preserve |        0.767 |         0.782 |      0.777 |    228.500 |     2.484 |
| greedy           |        0.637 |         0.583 |      0.603 |    228.500 |     1.642 |
| pgd              |        0.709 |         0.669 |      0.684 |    228.500 |     2.155 |
| random           |        0.854 |         0.818 |      0.831 |    228.500 |     0.012 |

## Untargeted CLIP scores

| judge                  | condition      |   score_mean |   score_std |   img_sim_to_clean_mean |   img_sim_to_clean_std |
|:-----------------------|:---------------|-------------:|------------:|------------------------:|-----------------------:|
| clip-vit-base-patch32  | none           |        0.321 |       0.025 |                   1.000 |                  0.000 |
| clip-vit-base-patch32  | random         |        0.322 |       0.025 |                   0.938 |                  0.074 |
| clip-vit-base-patch32  | greedy         |        0.309 |       0.047 |                   0.887 |                  0.146 |
| clip-vit-base-patch32  | genetic        |        0.299 |       0.040 |                   0.841 |                  0.125 |
| clip-vit-base-patch32  | pgd            |        0.313 |       0.034 |                   0.880 |                  0.123 |
| clip-vit-base-patch32  | greedy_prefix  |        0.289 |       0.058 |                   0.790 |                  0.170 |
| clip-vit-base-patch32  | genetic_prefix |        0.268 |       0.068 |                   0.768 |                  0.197 |
| clip-vit-large-patch14 | none           |        0.269 |       0.026 |                   1.000 |                  0.000 |
| clip-vit-large-patch14 | random         |        0.270 |       0.030 |                   0.928 |                  0.092 |
| clip-vit-large-patch14 | greedy         |        0.255 |       0.049 |                   0.875 |                  0.148 |
| clip-vit-large-patch14 | genetic        |        0.251 |       0.041 |                   0.844 |                  0.124 |
| clip-vit-large-patch14 | pgd            |        0.261 |       0.037 |                   0.875 |                  0.125 |
| clip-vit-large-patch14 | greedy_prefix  |        0.237 |       0.053 |                   0.783 |                  0.167 |
| clip-vit-large-patch14 | genetic_prefix |        0.217 |       0.074 |                   0.754 |                  0.196 |

## Targeted CLIP scores

| judge                  | condition        |   score_mean |   score_std |   keep_score_mean |   keep_score_std |   img_sim_to_clean_mean |   img_sim_to_clean_std |
|:-----------------------|:-----------------|-------------:|------------:|------------------:|-----------------:|------------------------:|-----------------------:|
| clip-vit-base-patch32  | none             |        0.279 |       0.029 |             0.279 |            0.030 |                   1.000 |                  0.000 |
| clip-vit-base-patch32  | random           |        0.277 |       0.036 |             0.286 |            0.024 |                   0.923 |                  0.106 |
| clip-vit-base-patch32  | greedy           |        0.253 |       0.039 |             0.260 |            0.040 |                   0.776 |                  0.222 |
| clip-vit-base-patch32  | genetic          |        0.244 |       0.043 |             0.264 |            0.045 |                   0.771 |                  0.217 |
| clip-vit-base-patch32  | pgd              |        0.258 |       0.046 |             0.273 |            0.035 |                   0.827 |                  0.160 |
| clip-vit-base-patch32  | genetic_prefix   |        0.268 |       0.036 |             0.266 |            0.041 |                   0.783 |                  0.204 |
| clip-vit-base-patch32  | genetic_preserve |        0.269 |       0.040 |             0.283 |            0.023 |                   0.829 |                  0.162 |
| clip-vit-large-patch14 | none             |        0.215 |       0.035 |             0.225 |            0.035 |                   1.000 |                  0.000 |
| clip-vit-large-patch14 | random           |        0.219 |       0.039 |             0.235 |            0.029 |                   0.902 |                  0.124 |
| clip-vit-large-patch14 | greedy           |        0.194 |       0.042 |             0.215 |            0.040 |                   0.762 |                  0.218 |
| clip-vit-large-patch14 | genetic          |        0.184 |       0.047 |             0.210 |            0.044 |                   0.752 |                  0.215 |
| clip-vit-large-patch14 | pgd              |        0.197 |       0.047 |             0.220 |            0.042 |                   0.815 |                  0.145 |
| clip-vit-large-patch14 | genetic_prefix   |        0.207 |       0.034 |             0.213 |            0.040 |                   0.761 |                  0.215 |
| clip-vit-large-patch14 | genetic_preserve |        0.202 |       0.045 |             0.227 |            0.028 |                   0.826 |                  0.157 |

## Targeted: object presence / erasure

| judge                  | condition        |   target_present |   keep_present |   n_eligible |   erase_rate |   clean_erase_rate |
|:-----------------------|:-----------------|-----------------:|---------------:|-------------:|-------------:|-------------------:|
| clip-vit-base-patch32  | none             |            0.897 |          0.966 |           26 |        0.000 |              0.000 |
| clip-vit-base-patch32  | random           |            0.893 |          1.000 |           25 |        0.040 |              0.040 |
| clip-vit-base-patch32  | greedy           |            0.655 |          0.793 |           26 |        0.308 |              0.154 |
| clip-vit-base-patch32  | genetic          |            0.517 |          0.862 |           26 |        0.423 |              0.385 |
| clip-vit-base-patch32  | pgd              |            0.714 |          0.964 |           26 |        0.231 |              0.192 |
| clip-vit-base-patch32  | genetic_prefix   |            0.862 |          0.931 |           26 |        0.115 |              0.115 |
| clip-vit-base-patch32  | genetic_preserve |            0.862 |          1.000 |           26 |        0.115 |              0.115 |
| clip-vit-large-patch14 | none             |            0.862 |          0.931 |           25 |        0.000 |              0.000 |
| clip-vit-large-patch14 | random           |            0.893 |          1.000 |           24 |        0.042 |              0.042 |
| clip-vit-large-patch14 | greedy           |            0.690 |          0.862 |           25 |        0.320 |              0.280 |
| clip-vit-large-patch14 | genetic          |            0.517 |          0.862 |           25 |        0.440 |              0.400 |
| clip-vit-large-patch14 | pgd              |            0.750 |          0.893 |           25 |        0.240 |              0.240 |
| clip-vit-large-patch14 | genetic_prefix   |            0.897 |          0.793 |           25 |        0.120 |              0.120 |
| clip-vit-large-patch14 | genetic_preserve |            0.759 |          0.966 |           25 |        0.200 |              0.200 |


| judge                  | prompt                               | target          |   clean_target_present |   clean_keep_present |
|:-----------------------|:-------------------------------------|:----------------|-----------------------:|---------------------:|
| clip-vit-large-patch14 | A black bicycle against a brick wall | a black bicycle |                  1.000 |                1.000 |
| clip-vit-large-patch14 | A red apple on a white plate         | a white plate   |                  0.600 |                1.000 |
| clip-vit-large-patch14 | A snake and a young man              | a young man     |                  0.500 |                0.750 |
| clip-vit-large-patch14 | A wooden cabin next to a lake        | a lake          |                  1.000 |                1.000 |
| clip-vit-large-patch14 | A cat sitting in a basket            | a basket        |                  1.000 |                1.000 |
| clip-vit-large-patch14 | A sailboat and a lighthouse          | a lighthouse    |                  1.000 |                0.800 |
| clip-vit-base-patch32  | A black bicycle against a brick wall | a black bicycle |                  1.000 |                1.000 |
| clip-vit-base-patch32  | A red apple on a white plate         | a white plate   |                  0.800 |                1.000 |
| clip-vit-base-patch32  | A snake and a young man              | a young man     |                  0.500 |                0.750 |
| clip-vit-base-patch32  | A wooden cabin next to a lake        | a lake          |                  1.000 |                1.000 |
| clip-vit-base-patch32  | A cat sitting in a basket            | a basket        |                  1.000 |                1.000 |
| clip-vit-base-patch32  | A sailboat and a lighthouse          | a lighthouse    |                  1.000 |                1.000 |

## Paired tests

| exp        | judge                  | attack           | vs     |   mean_diff |   n_prompts |   wilcoxon_p |
|:-----------|:-----------------------|:-----------------|:-------|------------:|------------:|-------------:|
| untargeted | clip-vit-large-patch14 | random           | none   |       0.002 |          12 |        0.677 |
| untargeted | clip-vit-large-patch14 | greedy           | none   |      -0.014 |          12 |        0.110 |
| untargeted | clip-vit-large-patch14 | greedy           | random |      -0.016 |          12 |        0.000 |
| untargeted | clip-vit-large-patch14 | genetic          | none   |      -0.018 |          12 |        0.002 |
| untargeted | clip-vit-large-patch14 | genetic          | random |      -0.020 |          12 |        0.003 |
| untargeted | clip-vit-large-patch14 | pgd              | none   |      -0.008 |          12 |        0.204 |
| untargeted | clip-vit-large-patch14 | pgd              | random |      -0.010 |          12 |        0.129 |
| untargeted | clip-vit-large-patch14 | greedy_prefix    | none   |      -0.032 |          12 |        0.000 |
| untargeted | clip-vit-large-patch14 | greedy_prefix    | random |      -0.034 |          12 |        0.000 |
| untargeted | clip-vit-large-patch14 | genetic_prefix   | none   |      -0.052 |          12 |        0.001 |
| untargeted | clip-vit-large-patch14 | genetic_prefix   | random |      -0.054 |          12 |        0.001 |
| untargeted | clip-vit-base-patch32  | random           | none   |       0.001 |          12 |        0.470 |
| untargeted | clip-vit-base-patch32  | greedy           | none   |      -0.012 |          12 |        0.266 |
| untargeted | clip-vit-base-patch32  | greedy           | random |      -0.013 |          12 |        0.007 |
| untargeted | clip-vit-base-patch32  | genetic          | none   |      -0.022 |          12 |        0.001 |
| untargeted | clip-vit-base-patch32  | genetic          | random |      -0.022 |          12 |        0.001 |
| untargeted | clip-vit-base-patch32  | pgd              | none   |      -0.008 |          12 |        0.204 |
| untargeted | clip-vit-base-patch32  | pgd              | random |      -0.009 |          12 |        0.034 |
| untargeted | clip-vit-base-patch32  | greedy_prefix    | none   |      -0.031 |          12 |        0.003 |
| untargeted | clip-vit-base-patch32  | greedy_prefix    | random |      -0.032 |          12 |        0.003 |
| untargeted | clip-vit-base-patch32  | genetic_prefix   | none   |      -0.053 |          12 |        0.001 |
| untargeted | clip-vit-base-patch32  | genetic_prefix   | random |      -0.054 |          12 |        0.001 |
| targeted   | clip-vit-large-patch14 | random           | none   |       0.001 |           6 |        0.844 |
| targeted   | clip-vit-large-patch14 | greedy           | none   |      -0.019 |           6 |        0.094 |
| targeted   | clip-vit-large-patch14 | greedy           | random |      -0.019 |           6 |        0.156 |
| targeted   | clip-vit-large-patch14 | genetic          | none   |      -0.029 |           6 |        0.062 |
| targeted   | clip-vit-large-patch14 | genetic          | random |      -0.029 |           6 |        0.156 |
| targeted   | clip-vit-large-patch14 | pgd              | none   |      -0.018 |           6 |        0.156 |
| targeted   | clip-vit-large-patch14 | pgd              | random |      -0.018 |           6 |        0.094 |
| targeted   | clip-vit-large-patch14 | genetic_prefix   | none   |      -0.005 |           6 |        0.844 |
| targeted   | clip-vit-large-patch14 | genetic_prefix   | random |      -0.006 |           6 |        0.438 |
| targeted   | clip-vit-large-patch14 | genetic_preserve | none   |      -0.011 |           6 |        0.844 |
| targeted   | clip-vit-large-patch14 | genetic_preserve | random |      -0.011 |           6 |        0.562 |
| targeted   | clip-vit-base-patch32  | random           | none   |      -0.005 |           6 |        0.562 |
| targeted   | clip-vit-base-patch32  | greedy           | none   |      -0.024 |           6 |        0.062 |
| targeted   | clip-vit-base-patch32  | greedy           | random |      -0.019 |           6 |        0.156 |
| targeted   | clip-vit-base-patch32  | genetic          | none   |      -0.033 |           6 |        0.031 |
| targeted   | clip-vit-base-patch32  | genetic          | random |      -0.028 |           6 |        0.219 |
| targeted   | clip-vit-base-patch32  | pgd              | none   |      -0.021 |           6 |        0.156 |
| targeted   | clip-vit-base-patch32  | pgd              | random |      -0.016 |           6 |        0.031 |
| targeted   | clip-vit-base-patch32  | genetic_prefix   | none   |      -0.009 |           6 |        0.312 |
| targeted   | clip-vit-base-patch32  | genetic_prefix   | random |      -0.004 |           6 |        0.688 |
| targeted   | clip-vit-base-patch32  | genetic_preserve | none   |      -0.008 |           6 |        1.000 |
| targeted   | clip-vit-base-patch32  | genetic_preserve | random |      -0.003 |           6 |        1.000 |

## Text cos vs CLIP score
Spearman rho = 0.451 (p = 1.69e-05)


## Budget ablation (mean cos)

|      k |   prefix |   suffix |
|-------:|---------:|---------:|
|  1.000 |    0.785 |    0.855 |
|  2.000 |    0.738 |    0.804 |
|  3.000 |    0.658 |    0.757 |
|  4.000 |    0.579 |    0.697 |
|  5.000 |    0.538 |    0.656 |
|  6.000 |    0.512 |    0.635 |
|  7.000 |    0.480 |    0.599 |
|  8.000 |    0.459 |    0.576 |
|  9.000 |    0.437 |    0.559 |
| 10.000 |    0.410 |    0.544 |

## Figures
fig_causal_positions.png, fig_budget.png, fig_text_vs_image.png, fig_grid_untargeted.png, fig_grid_targeted.png


## Questions to answer in the write-up

- Which optimizer is strongest in text space and in image space, and at what query cost? Do you reproduce Genetic > PGD > Greedy > Random?

- Is the CLIP-score drop significant (paired tests) and does the independent ViT-B/32 judge agree?

- What does the causal-mask plot imply about *why* suffix attacks work, and why prefixes are stronger?

- Targeted: is the target removed while the rest is kept (erase_rate, clean_erase_rate)? Does the preserve term (our extension) or prefix placement help?

- How well does text-embedding cosine predict image change? How large was PGD's relaxation gap and how much did discrete refinement recover?

- Defences / limitations: perplexity or character-pattern filtering of prompts, robust text encoders, small sample size, CLIP score as a proxy.
