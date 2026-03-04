---
title: "📖 Thesis progress v4"
date: 2026-03-02 00:00:00 +0800
categories: [master]
tags: [progress, tech, thesis]
---

### Thesis Progress


### 1150302

### v4


### Compare Results with other papers

### Compare with CLIP、MOMENTA、Hateclipper、RGCL、ExplainHM、KID-VLM

### TODO:

### Compare with LLM methods (Flamingo-80B1, LLaVA (Vicuna-13B))

### Compare with Object Detector based models (ERNIE-Vil, UNITER, OSCAR)

### [2021]

### [2021]

### [2022]

### [2024]

### [2024]

### [2025]

![slide 2](/assets/img/2026-03-02/slide2_img1.png)


### TODO

### ExplainHM [2024]

### Just Kiddin [2025]

### 從 AdvLoRA review 教訓中學到的：

### PGD + AutoAttack + black-box attack(RAP)

### 完整的 ablation study（每個模組的貢獻）

### Training time / memory cost 比較

### 清楚解釋 "why" 每個組件是必要的

### CLIP model comparison: clip-ViT-B/16 & clip-ViT-L/14


### Adversarial Robustness

PGD: The strongest single white-box attack.

RAP: A transfer-based attack specifically designed to boost adversarial transferability across models.

AutoAttack: The most comprehensive standard robustness benchmark, combining both white-box and black-box attacks.

![slide 4](/assets/img/2026-03-02/slide4_img1.png)


### Adversarial LoRA’s Problem

### AdvLoRA (ICLR 2025, Withdrawn)

### AdvCLIP-LoRA (ICLR 2026, Withdrawn)

Lack of novelty — Simply combining LoRA with adversarial training, without non-trivial insights or new technical contributions

No explanation of "why it works" — Only showing numerical improvements without analyzing why LoRA-based adversarial training outperforms other approaches

Limited attack evaluation — Only evaluating against PGD; missing AutoAttack, CW, and black-box attacks

No computational cost analysis — Missing training time, memory usage, and parameter count comparisons

Insufficient ablation studies — Failing to demonstrate the necessity and individual contribution of each component


### Adversarial LoRA

Adversarial LoRA is a “tool” not a contribution.

### Why we need adversarial training?

Adversarial training has been established as the most effective defense against adversarial attacks on VLMs.

TeCoA (Mao et al., ICLR 2023) first demonstrated that text-guided contrastive adversarial training can significantly improve the robustness of CLIP while preserving vision-language alignment.

### Subsequent works:

### FARE (Schlarmann et al., ICML 2024)

### PMG-AFT (Wang et al., CVPR 2024)

### FAP (Zhou et al., NeurIPS 2024)


### Adversarial LoRA

### Why I choose to use LoRA?

PMG-AFT adopts full fine-tuning, which updates all parameters of the CLIP image encoder (~88M). This causes the model to drift far from its original pre-trained weights, destroying the vision-language alignment learned during pre-training. PMG-AFT must then introduce an additional generalization loss to pull the model back toward the original representations.

LoRA avoids this problem by design: the original CLIP weights remain completely frozen, and only small low-rank adapter matrices are trained (~1–3% of total parameters). This provides a structural safeguard for pre-trained knowledge — no additional regularization loss is needed to prevent forgetting. Recent concurrent works (AdvLoRA, Ji et al., 2024; AdvCLIP-LoRA, Ghiasvand et al., 2025) have also explored LoRA-based adversarial training for VLMs, but focused solely on this combination for standard classification tasks and were criticized for limited novelty. We do not claim adversarial LoRA as our contribution; it is simply the parameter-efficient vehicle through which we deliver robustness.


### Adversarial LoRA

### My contribution

Our contribution is NOT adversarial LoRA itself, but the discovery and solution of a new problem: adversarial training degrades the cross-modal reasoning ability of hateful meme detection models.

(Problem) We find that naively applying adversarial training to the RGCL framework significantly degrades clean accuracy, because adversarial perturbations disrupt the contrastive alignment between hateful image-text pairs that RGCL carefully learned.

(Solution) We propose a synergistic framework where three components each play a distinct and necessary role:

### LoRA (frozen weights) → preserves CLIP's pre-trained vision-language representations

Response-based KD with temperature → distills the clean teacher's fine-grained hateful/non-hateful discrimination ability into the adversarially-trained student

RGCL (contrastive learning) → maintains cross-modal semantic alignment using external knowledge (ConceptNet + LLaVA captions)

(Validation) Ablation studies show that removing any single component leads to significant performance drops, confirming that their interaction is non-trivial and synergistic — not a simple stacking of existing techniques.


### TODO

### LLM Model

### BLIP-2 (2.7B) →2023

### LLaVA-1.5 (7B) →2023

### Qwen2-VL (7B) →2024

### Qwen3-VL (8B) →2025
