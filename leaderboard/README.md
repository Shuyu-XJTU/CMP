# 🏆 PAB Leaderboard

<!-- markdownlint-disable MD013 -->

[![Task](https://img.shields.io/badge/Task-Text--based%20Person%20Anomaly%20Search-0b7285)](https://openaccess.thecvf.com/content/ICCV2025/html/Yang_Beyond_Walking_A_Large-Scale_Image-Text_Benchmark_for_Text-based_Person_Anomaly_ICCV_2025_paper.html)
[![Paper](https://img.shields.io/badge/ICCV-2025%20Highlight-6f42c1)](https://openaccess.thecvf.com/content/ICCV2025/html/Yang_Beyond_Walking_A_Large-Scale_Image-Text_Benchmark_for_Text-based_Person_Anomaly_ICCV_2025_paper.html)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/Shuyu-XJTU/CMP/pulls)

This page tracks published results for **Text-based Person Anomaly Search** on
the **Pedestrian Anomaly Behavior (PAB)** benchmark and its
out-of-distribution **UCC** test set.

Results are separated by training protocol. Scores from zero-shot evaluation,
fine-tuning with 0.1M PAB pairs, fine-tuning with the full 1M PAB set, and
test-time adaptation are **not directly comparable**.

**Last updated:** 2026-07-25

[Paper](https://openaccess.thecvf.com/content/ICCV2025/html/Yang_Beyond_Walking_A_Large-Scale_Image-Text_Benchmark_for_Text-based_Person_Anomaly_ICCV_2025_paper.html)
· [Dataset and code](https://github.com/Shuyu-XJTU/CMP)
· [Checkpoint](https://drive.google.com/drive/folders/1qdOIwyXD72gbccqGGRxvqg4gWEVI2ceA?usp=sharing)
· [2026 challenge](https://www.aicitychallenge.org/2026-track4/)
· [Submit a result](https://github.com/Shuyu-XJTU/CMP/issues/new?template=leaderboard-submission.yml)

## Contents

- [Benchmark](#benchmark)
- [Evaluation protocols](#evaluation-protocols)
- [PAB: fine-tuned with 1M pairs](#pab-fine-tuned-with-1m-pairs)
- [PAB: fine-tuned with 0.1M pairs](#pab-fine-tuned-with-01m-pairs)
- [PAB: zero-shot](#pab-zero-shot)
- [PAB: test-time adaptation](#pab-test-time-adaptation)
- [UCC: out-of-distribution evaluation](#ucc-out-of-distribution-evaluation)
- [Official challenges](#official-challenges)
- [Submit a result](#submit-a-result)
- [Citation](#citation)

## Benchmark

| Split | Scale | Source | Purpose |
| :--- | ---: | :--- | :--- |
| PAB train | 1,013,605 image-text pairs | Synthetic | Training and fine-tuning |
| PAB test | 1,978 image-text pairs | Real-world videos | Primary benchmark evaluation |
| UCC test | 5,320 image-text pairs | UCF-Crime keyframes | Out-of-distribution evaluation |
| AICity'26 Track 4 test | 1,978 queries and 36,773 gallery images | PAB test plus 34,795 distractors | Challenge evaluation |

The query describes the target pedestrian's **appearance, action, and scene**.
The standard metrics are Recall@K (**R@1**, **R@5**, and **R@10**) and mean
Average Precision (**mAP**). Higher is better.

## Evaluation protocols

| Protocol | PAB training pairs | Unlabeled target-set adaptation? | Ranked separately? |
| :--- | :---: | :---: | :---: |
| Zero-shot | 0 | No | Yes |
| Fine-tuned with 0.1M PAB | 0.1M | No | Yes |
| Fine-tuned with 1M PAB | 1M | No | Yes |
| Pretrain-then-Adapt | 0 labeled pairs | Yes | Yes |
| UCC OOD | Reported per table | No UCC adaptation | Yes |

Ranking rules:

1. Tables are ordered by R@1.
2. Only results reported in a peer-reviewed paper or an official benchmark
   source are ranked.
3. A dash means that the paper did not report the metric or release the code.
4. Reproduced baselines are marked with their result source.
5. Ensemble, re-ranking, and large-model components must be disclosed.

## PAB: fine-tuned with 1M pairs

| Method | Venue | Model / setting | R@1 | R@5 | R@10 | mAP | Result source | Code |
| :--- | :--- | :--- | ---: | ---: | ---: | ---: | :---: | :---: |
| [Hybrid, Unified and Iterative](https://doi.org/10.1145/3701716.3717653) | WWW Companion 2025 | BEiT-3 + UIT + iterative ensemble | **89.23** | **99.70** | **99.85** | - | [Table 1](https://arxiv.org/abs/2511.22470) | [Code](https://github.com/AIVIETNAM-Hub/Hybrid-Unified-and-Iterative-A-Novel-Framework-for-Text-based-Person-Anomaly-Retrieval) |
| [SSDC](https://aclanthology.org/2026.findings-acl.197/) | Findings of ACL 2026 | ViT-B/16 + BERT-Base + Qwen3-VL-8B cascade | 87.21 | 99.09 | 99.75 | **92.87** | [Table 2](https://aclanthology.org/2026.findings-acl.197.pdf) | - |
| [CMP](https://openaccess.thecvf.com/content/ICCV2025/html/Yang_Beyond_Walking_A_Large-Scale_Image-Text_Benchmark_for_Text-based_Person_Anomaly_ICCV_2025_paper.html) | ICCV 2025 Highlight | Swin-B + BERT-Base, pose-aware | 84.93 | 99.09 | 99.75 | 91.66 | [Table 3](https://openaccess.thecvf.com/content/ICCV2025/papers/Yang_Beyond_Walking_A_Large-Scale_Image-Text_Benchmark_for_Text-based_Person_Anomaly_ICCV_2025_paper.pdf) | [Code](https://github.com/Shuyu-XJTU/CMP) |
| [IRRA](https://openaccess.thecvf.com/content/CVPR2023/html/Jiang_Cross-Modal_Implicit_Relation_Reasoning_and_Aligning_for_Text-to-Image_Person_Retrieval_CVPR_2023_paper.html) | CVPR 2023 | ViT-B/16 + Transformer | 78.67 | 97.98 | 98.94 | 87.74 | [SSDC Table 2](https://aclanthology.org/2026.findings-acl.197.pdf) | [Code](https://github.com/anosorae/IRRA) |
| [RDE](https://openaccess.thecvf.com/content/CVPR2024/html/Qin_Noisy-Correspondence_Learning_for_Text-to-Image_Person_Re-identification_CVPR_2024_paper.html) | CVPR 2024 | ViT-B/16 + Transformer | 76.74 | 96.97 | 98.38 | 86.12 | [SSDC Table 2](https://aclanthology.org/2026.findings-acl.197.pdf) | [Code](https://github.com/QinYang79/RDE) |

> **Protocol note:** Hybrid, Unified and Iterative reports R@K but not mAP.
> Its result uses an iterative multi-model ensemble. IRRA and RDE were
> reproduced on 1M PAB by the SSDC authors.

## PAB: fine-tuned with 0.1M pairs

All results in this section are reported in Table 3 of the
[CMP paper](https://openaccess.thecvf.com/content/ICCV2025/papers/Yang_Beyond_Walking_A_Large-Scale_Image-Text_Benchmark_for_Text-based_Person_Anomaly_ICCV_2025_paper.pdf).

| Method | Venue | R@1 | R@5 | R@10 | mAP | Code |
| :--- | :--- | ---: | ---: | ---: | ---: | :---: |
| [CMP](https://openaccess.thecvf.com/content/ICCV2025/html/Yang_Beyond_Walking_A_Large-Scale_Image-Text_Benchmark_for_Text-based_Person_Anomaly_ICCV_2025_paper.html) | ICCV 2025 Highlight | **83.06** | **98.89** | 99.49 | **90.41** | [Code](https://github.com/Shuyu-XJTU/CMP) |
| [X-VLM](https://proceedings.mlr.press/v162/zeng22c.html) | ICML 2022 | 81.95 | 98.84 | 99.19 | 89.86 | [Code](https://github.com/zengyan-97/X-VLM) |
| [RaSa](https://www.ijcai.org/proceedings/2023/62) | IJCAI 2023 | 80.79 | **98.89** | 99.65 | 89.20 | [Code](https://github.com/Flame-Chasers/RaSa) |
| [CLIP](https://proceedings.mlr.press/v139/radford21a.html) | ICML 2021 | 77.60 | 98.84 | **99.75** | 87.35 | [Code](https://github.com/openai/CLIP) |
| [IRRA](https://openaccess.thecvf.com/content/CVPR2023/html/Jiang_Cross-Modal_Implicit_Relation_Reasoning_and_Aligning_for_Text-to-Image_Person_Retrieval_CVPR_2023_paper.html) | CVPR 2023 | 76.39 | 97.62 | 99.14 | 86.33 | [Code](https://github.com/anosorae/IRRA) |
| [WoRA](https://doi.org/10.1145/3696410.3714788) | WWW 2025 | 74.47 | 96.82 | 98.48 | 84.60 | [Code](https://github.com/JT-Sun/Filtering-WoRA) |
| [CAMeL](https://doi.org/10.1109/TIFS.2025.3565392) | TIFS 2025 | 74.30 | 96.79 | 98.84 | 84.20 | - |
| [APTM](https://arxiv.org/abs/2306.02898) | ACM MM 2023 | 72.14 | 95.30 | 97.17 | 82.78 | [Code](https://github.com/Shuyu-XJTU/APTM) |
| [MRA](https://arxiv.org/abs/2507.10195) | Pattern Recognition 2026 | 70.53 | 94.69 | 97.47 | 81.59 | [Code](https://github.com/Shuyu-XJTU/MRA) |

## PAB: zero-shot

No PAB pair is used for training or adaptation. All results are reported in
Table 3 of the
[CMP paper](https://openaccess.thecvf.com/content/ICCV2025/papers/Yang_Beyond_Walking_A_Large-Scale_Image-Text_Benchmark_for_Text-based_Person_Anomaly_ICCV_2025_paper.pdf).

| Method | Venue | R@1 | R@5 | R@10 | mAP | Code |
| :--- | :--- | ---: | ---: | ---: | ---: | :---: |
| [X-VLM](https://proceedings.mlr.press/v162/zeng22c.html) | ICML 2022 | **71.94** | **97.78** | **98.99** | **83.96** | [Code](https://github.com/zengyan-97/X-VLM) |
| [CLIP](https://proceedings.mlr.press/v139/radford21a.html) | ICML 2021 | 47.57 | 81.55 | 89.03 | 62.73 | [Code](https://github.com/openai/CLIP) |
| [IRRA](https://openaccess.thecvf.com/content/CVPR2023/html/Jiang_Cross-Modal_Implicit_Relation_Reasoning_and_Aligning_for_Text-to-Image_Person_Retrieval_CVPR_2023_paper.html) | CVPR 2023 | 30.59 | 59.61 | 68.91 | 44.41 | [Code](https://github.com/anosorae/IRRA) |
| [CAMeL](https://doi.org/10.1109/TIFS.2025.3565392) | TIFS 2025 | 24.47 | 50.00 | 58.75 | 36.75 | - |
| [APTM](https://arxiv.org/abs/2306.02898) | ACM MM 2023 | 22.90 | 45.80 | 52.38 | 33.56 | [Code](https://github.com/Shuyu-XJTU/APTM) |
| [WoRA](https://doi.org/10.1145/3696410.3714788) | WWW 2025 | 22.25 | 45.91 | 53.54 | 33.39 | [Code](https://github.com/JT-Sun/Filtering-WoRA) |
| [RaSa](https://www.ijcai.org/proceedings/2023/62) | IJCAI 2023 | 21.74 | 27.30 | 27.96 | 24.35 | [Code](https://github.com/Flame-Chasers/RaSa) |
| [MRA](https://arxiv.org/abs/2507.10195) | Pattern Recognition 2026 | 9.91 | 23.66 | 31.45 | 17.15 | [Code](https://github.com/Shuyu-XJTU/MRA) |

## PAB: test-time adaptation

This protocol adapts a pretrained model using **unlabeled target samples** and
therefore is ranked separately from both zero-shot evaluation and supervised
PAB fine-tuning.

| Method | Venue | Backbone | Post-train | R@1 | R@5 | R@10 | mAP | Code |
| :--- | :--- | :--- | :--- | ---: | ---: | ---: | ---: | :---: |
| [UATTA](https://doi.org/10.1145/3805712.3809598) | SIGIR 2026 | X-VLM | 0.08 h, 1 GPU | **76.13** | **98.02** | **99.09** | **86.14** | [Code](https://github.com/nkuzjh/UATTA) |

Scores are from Table 1 of the
[UATTA paper](https://arxiv.org/abs/2604.08598).

## UCC: out-of-distribution evaluation

UCC contains 5,320 unseen image-text pairs extracted from UCF-Crime. UCC labels
must not be used for training or adaptation. Training scales are separated below.

### Trained with 1M PAB pairs

| Method | Venue | R@1 | R@5 | R@10 | mAP | Result source |
| :--- | :--- | ---: | ---: | ---: | ---: | :---: |
| [SSDC](https://aclanthology.org/2026.findings-acl.197/) | Findings of ACL 2026 | **59.45** | **72.68** | **78.30** | **45.25** | [Table 3](https://aclanthology.org/2026.findings-acl.197.pdf) |
| [CMP](https://openaccess.thecvf.com/content/ICCV2025/html/Yang_Beyond_Walking_A_Large-Scale_Image-Text_Benchmark_for_Text-based_Person_Anomaly_ICCV_2025_paper.html) | ICCV 2025 Highlight | 55.23 | 71.67 | 77.99 | 44.35 | [Table 4](https://openaccess.thecvf.com/content/ICCV2025/papers/Yang_Beyond_Walking_A_Large-Scale_Image-Text_Benchmark_for_Text-based_Person_Anomaly_ICCV_2025_paper.pdf) |
| [RDE](https://openaccess.thecvf.com/content/CVPR2024/html/Qin_Noisy-Correspondence_Learning_for_Text-to-Image_Person_Re-identification_CVPR_2024_paper.html) | CVPR 2024 | 32.69 | 48.55 | 56.64 | 27.42 | [SSDC Table 3](https://aclanthology.org/2026.findings-acl.197.pdf) |

### Trained with 0.1M PAB pairs

| Method | Venue | R@1 | R@5 | R@10 | mAP | Result source |
| :--- | :--- | ---: | ---: | ---: | ---: | :---: |
| [CMP](https://openaccess.thecvf.com/content/ICCV2025/html/Yang_Beyond_Walking_A_Large-Scale_Image-Text_Benchmark_for_Text-based_Person_Anomaly_ICCV_2025_paper.html) | ICCV 2025 Highlight | **54.12** | **71.07** | **77.90** | **43.13** | [Table 4](https://openaccess.thecvf.com/content/ICCV2025/papers/Yang_Beyond_Walking_A_Large-Scale_Image-Text_Benchmark_for_Text-based_Person_Anomaly_ICCV_2025_paper.pdf) |
| [RaSa](https://www.ijcai.org/proceedings/2023/62) | IJCAI 2023 | **54.12** | 70.32 | 75.96 | 39.71 | [CMP Table 4](https://openaccess.thecvf.com/content/ICCV2025/papers/Yang_Beyond_Walking_A_Large-Scale_Image-Text_Benchmark_for_Text-based_Person_Anomaly_ICCV_2025_paper.pdf) |
| [X-VLM](https://proceedings.mlr.press/v162/zeng22c.html) | ICML 2022 | 52.33 | 66.73 | 72.54 | 40.87 | [CMP Table 4](https://openaccess.thecvf.com/content/ICCV2025/papers/Yang_Beyond_Walking_A_Large-Scale_Image-Text_Benchmark_for_Text-based_Person_Anomaly_ICCV_2025_paper.pdf) |
| [CLIP](https://proceedings.mlr.press/v139/radford21a.html) | ICML 2021 | 51.60 | 68.31 | 76.43 | 43.05 | [CMP Table 4](https://openaccess.thecvf.com/content/ICCV2025/papers/Yang_Beyond_Walking_A_Large-Scale_Image-Text_Benchmark_for_Text-based_Person_Anomaly_ICCV_2025_paper.pdf) |
| [IRRA](https://openaccess.thecvf.com/content/CVPR2023/html/Jiang_Cross-Modal_Implicit_Relation_Reasoning_and_Aligning_for_Text-to-Image_Person_Retrieval_CVPR_2023_paper.html) | CVPR 2023 | 40.28 | 57.24 | 65.98 | 33.53 | [CMP Table 4](https://openaccess.thecvf.com/content/ICCV2025/papers/Yang_Beyond_Walking_A_Large-Scale_Image-Text_Benchmark_for_Text-based_Person_Anomaly_ICCV_2025_paper.pdf) |
| [APTM](https://arxiv.org/abs/2306.02898) | ACM MM 2023 | 27.86 | 40.41 | 46.77 | 22.61 | [CMP Table 4](https://openaccess.thecvf.com/content/ICCV2025/papers/Yang_Beyond_Walking_A_Large-Scale_Image-Text_Benchmark_for_Text-based_Person_Anomaly_ICCV_2025_paper.pdf) |

## Official challenges

### AICity'26 ECCV Workshop Track 4

The 2026 challenge uses the original PAB training set and a **new test protocol
with distractors**:

- 1,978 name-masked text queries;
- 1,978 corresponding ground-truth gallery images;
- 34,795 distractor images;
- mAP as the official leaderboard metric.

This protocol must not be mixed with the standard PAB tables above.

[Challenge page](https://www.aicitychallenge.org/2026-track4/)
· [Data and submission format](https://github.com/Shuyu-XJTU/PAB-for-ECCV26-Workshop-Track4)

### WWW'25 challenge archive

The original Text-based Person Anomaly Search Challenge is archived on
[CodaLab](https://codalab.lisn.upsaclay.fr/competitions/21001).

## Submit a result

Open a
[leaderboard submission issue](https://github.com/Shuyu-XJTU/CMP/issues/new?template=leaderboard-submission.yml)
or submit a pull request that edits this file.

Please provide:

- method and paper title;
- peer-reviewed venue and year;
- paper and code links;
- benchmark and exact protocol;
- R@1, R@5, R@10, and mAP;
- table or page number containing the result;
- training-data scale and any ensemble, re-ranking, or test-time adaptation.

Results that use a different split, extra private data, or test-set supervision
will be listed only in a clearly separated protocol.

## Citation

If you use PAB, UCC, or CMP, please cite:

```bibtex
@inproceedings{yang2025beyond,
  title={Beyond Walking: A Large-Scale Image-Text Benchmark for Text-based Person Anomaly Search},
  author={Yang, Shuyu and Wang, Yaxiong and Zhu, Li and Zheng, Zhedong},
  booktitle={Proceedings of the IEEE/CVF International Conference on Computer Vision},
  year={2025}
}
```

Please also cite the original paper for every method whose result you use.
