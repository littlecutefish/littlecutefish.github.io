---
title: "📖 Thesis Progress-v1"
date: 2025-12-29 00:00:00 +0800
categories: [master]
tags: [progress, tech, thesis]
---

### Thesis Progress

### V1


### Background

Robust Hateful Meme Detection: Combining Retrieval-Guided Contrastive Learning with Adversarial LoRA

### Background - The Challenge of Hateful Memes

Context: Hateful memes are multimodal (Image + Text), making detection hard due to "Confounders" (subtle differences changing meaning).

Base Reference Paper: Retrieval-Guided Contrastive Learning (RGCL)[1].

Uses Frozen CLIP as the backbone.

Effective at handling Semantic Confounders via hard-negative mining.

The Hidden Vulnerability: While RGCL handles semantic shifts, it assumes clean input. It remains vulnerable to Adversarial Attacks (imperceptible pixel noise).

[1] Mei, Jingbiao, et al. "Improving hateful meme detection through retrieval-guided contrastive learning." Proceedings of the 62nd Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers). 2024.


### Retrieval-Guided Contrastive Learning (RGCL)

### Vulnerable

### Clean Embeddings:

### L2 Norm Mean: 33.0717

### Range: [31.9634, 34.6611]

### Adversarial Embeddings:

### L2 Norm Mean: 32.3229

### Range: [29.1735, 34.9629]

![slide 3](/assets/img/2025-12-29/slide3_img1.png)

![slide 3](/assets/img/2025-12-29/slide3_img2.png)


### Research Gap & Motivation

### HateProof[2]:

Relies on outdated architectures (VisualBERT, Faster R-CNN).

High computational cost (Latency).

Not End-to-End (Region-based feature extraction).

RGCL: Object Detector-based models such as VisualBERT, OSCAR, and UNITER use Faster R-CNN based object detectors as the vision model.

### Standard Adversarial Training:

Often leads to a drop in Clean Accuracy (Catastrophic Forgetting).

Computationally expensive to retrain large VLMs like CLIP.

### Motivation:

We need a model that is Dual Robust (Semantic + Pixel).

We need a Parameter-Efficient solution (cannot retrain full CLIP).

[2] Aggarwal, Piush, et al. "Hateproof: Are hateful meme detection systems really robust?." Proceedings of the ACM Web Conference 2023. 2023.


### HateProof

### Spread

### Blur(text)

### Newsprint

### Salt & pepper

### Salt & pepper(text)

![slide 5](/assets/img/2025-12-29/slide5_img1.png)

![slide 5](/assets/img/2025-12-29/slide5_img2.png)

![slide 5](/assets/img/2025-12-29/slide5_img3.png)

![slide 5](/assets/img/2025-12-29/slide5_img4.png)

![slide 5](/assets/img/2025-12-29/slide5_img5.png)

![slide 5](/assets/img/2025-12-29/slide5_img6.png)


### HateProof Experiments

### 
|  | Accuracy |  | AUROC |  |

| --- | --- | --- | --- | --- |

| Scenario | No Adv | Adv (s=5) | No Adv | Adv (s=5) |

| Clean | 58.90% | 59.60% | 62.25% | 63.97% |

| Blur Text 5 | 58.60% | 58.70% | 61.40% | 62.96% |

| Spread 1 | 58.70% | 59.20% | 62.70% | 63.65% |

| Spread 3 | 57.20% | 58.90% | 60.56% | 61.79% |

| Newsprint | 55.40% | 56.20% | 57.66% | 59.80% |

| S&P | 57.00% | 57.40% | 59.88% | 60.02% |

| S&P 0.4 | 55.90% | 56.40% | 58.36% | 58.76% |

| S&P Text 0.2 | 59.30% | 60.30% | 62.70% | 64.23% |

### 
|  | Accuracy |  | AUROC |  |

| --- | --- | --- | --- | --- |

| Scenario | No Adv | Adv (s=5) | No Adv | Adv (s=5) |

| Clean | 72.60% | 73.45% | 76.54% | 78.68% |

| Blur Text 5 | 71.47% | 70.62% | 75.89% | 78.17% |

| Spread 1 | 73.73% | 72.32% | 69.12% | 77.02% |

| Spread 3 | 71.75% | 70.34% | 67.39% | 72.45% |

| Newsprint | 72.03% | 71.75% | 66.37% | 71.88% |

| S&P | 69.77% | 72.88% | 71.15% | 75.06% |

| S&P 0.4 | 70.62% | 72.88% | 65.39% | 72.55% |

| S&P Text 0.2 | 72.32% | 73.16% | 75.69% | 78.38% |

### HarmC Dataset

### FB Dataset

### Basic model: Uniter

### Adversarial Training: Villa [3]

[3] Gan, Zhe, et al. "Large-scale adversarial training for vision-and-language representation learning." Advances in neural information processing systems 33 (2020): 6616-6628.


### Uniter (UNiversal Image-TExt Representation Learning)

### 屬於「上一代」架構： UNITER 代表了 OD-based (Object Detector-based) 的多模態模型。

### 它的瓶頸在於：

### 視野受限：如果 Faster R-CNN 沒框到的背景或細節，UNITER 就看不到（例如迷因中的氛圍）。

### 計算昂貴：跑 Faster R-CNN 取特徵非常耗時，導致推理速度慢 (High Latency)。

### 錯誤傳播：如果前面的 Object Detector 抓錯了，後面的 Transformer 再強也救不回來。

### 對比你的 CLIP (ViT-based)： 你的 CLIP 是 Pixel-based 且 End-to-End。

### UNITER: 圖像 -> Faster R-CNN -> 區域特徵 -> Transformer

### CLIP (你): 圖像 -> 切 Patch -> 線性投影 -> Transformer

### 結論: 你的方法能捕捉更完整的全局語境 (Global Context)，且運算效率更高，更適合處理需要理解「整體意境」的仇恨迷因。

### https://vocus.cc/article/68a4d5cffd89780001c1710c


### Research Goal

Objective: Develop a robust Hateful Meme Detection framework that defends against PGD attacks without sacrificing clean data performance.

### Key Innovations:

Base: RGCL for semantic context.

Defense: Adv-LoRA for efficient robust adaptation.

Optimization: TRADES Loss to align clean/adversarial distributions.


### Methodology I - Base Framework (RGCL)

Model: Frozen CLIP Image/Text Encoders + MLP Classifier.

Mechanism: Uses a retrieval mechanism to find "Pseudo-gold positives" and "Hard negatives".

### Core Formula:

LCE: Cross-Entropy Loss (Standard Classification).

LRGCLL: Retrieval-Guided Contrastive Loss.

![slide 9](/assets/img/2025-12-29/slide9_img1.png)

![slide 9](/assets/img/2025-12-29/slide9_img2.png)

![slide 9](/assets/img/2025-12-29/slide9_img3.png)

![slide 9](/assets/img/2025-12-29/slide9_img4.png)


### Attack Stage (Standard PGD[4])

### Standard PGD Attack:

### : CLIP image encoder

### : CLIP text encoder

### : cosine similarity

### t  :  text

### :  perturb

### Constructive loss:

[4] Madry, Aleksander, et al. "Towards deep learning models resistant to adversarial attacks." arXiv preprint arXiv:1706.06083 (2017).

![slide 10](/assets/img/2025-12-29/slide10_img1.png)

![slide 10](/assets/img/2025-12-29/slide10_img2.png)

![slide 10](/assets/img/2025-12-29/slide10_img3.png)

![slide 10](/assets/img/2025-12-29/slide10_img4.png)

![slide 10](/assets/img/2025-12-29/slide10_img5.png)

![slide 10](/assets/img/2025-12-29/slide10_img6.png)


### Methodology II - Defense Strategy (AdvCLIP-LoRA[5])

Concept: Apply Low-Rank Adaptation (LoRA) to the CLIP encoder, trained on adversarial examples.

### Why LoRA?

Keeps Pre-trained CLIP frozen (Retains general knowledge).

Updates only 0.25% of parameters (Efficient).

### trainable params: 1,081,344 || all params: 429,025,537 || trainable%: 0.2520

### LoRA Formulation:

### (where                                    )

[5] Ghiasvand, Sajjad, et al. "Few-Shot Adversarial Low-Rank Fine-Tuning of Vision-Language Models." arXiv preprint arXiv:2505.15130 (2025).

![slide 11](/assets/img/2025-12-29/slide11_img1.png)

![slide 11](/assets/img/2025-12-29/slide11_img2.png)

![slide 11](/assets/img/2025-12-29/slide11_img3.png)


### Methodology III - Advanced PGD Training

Attack Type: Adapted from TRADES[6]. Instead of attacking the classification boundary directly, we attack the Vision-Language Alignment by maximizing the deviation in the feature space.

### Mathematical Formulation:

Purpose: To evaluate robustness against semantic feature corruption, simulating a scenario where the visual encoder is compromised to disrupt downstream hateful detection.

[6] Zhang, Hongyang, et al. "Theoretically principled trade-off between robustness and accuracy." International conference on machine learning. PMLR, 2019.

![slide 12](/assets/img/2025-12-29/slide12_img1.png)

![slide 12](/assets/img/2025-12-29/slide12_img2.png)


### Methodology IV - Optimization (TRADES Loss)

The Problem: Pure adversarial training hurts accuracy on clean memes.

The Solution: TRADES (Theoretically Principled Trade-off).

Mechanism: Enforce Distribution Alignment between Clean and Adv outputs.

### Our Proposed Loss Function:

(Trade-off parameter).

(Temperature).

Effect: Smooths the decision boundary, maintaining clean accuracy while gaining robustness.

![slide 13](/assets/img/2025-12-29/slide13_img1.png)

![slide 13](/assets/img/2025-12-29/slide13_img2.png)

![slide 13](/assets/img/2025-12-29/slide13_img3.png)


### Experiment Settings

### Attack: Advance-PGD (But I will change to standard PGD)

### AdvCLIP-LoRA training

### tau=10 (attack-iteration)

### rank=8

### dropout=0.1

### lr=0.0002

### kl=0.1

### Random mix perturbation [1/255, 4/255, 8/255, 16/255]

### epoch=5, batch size = 8

### Training AdvCLIP-LoRA Time:

### HarmC: 3h 35m

### FB: 7h 19m


### Results (Standard PGD)

### HarmC Dataset

### FB Dataset

### 
|  | Scenario | Acc | AUROC | Acc
Drop % | AUROC
Drop % |

| --- | --- | --- | --- | --- | --- |

| img attack |  |  |  |  |  |

|  |  |  |  |  |  |

|  |  |  |  |  |  |

|  |  |  |  |  |  |

### 
|  | Scenario | Acc | AUROC | Acc
Drop % | AUROC
Drop % |

| --- | --- | --- | --- | --- | --- |

| tau=10
r=8
dropout=0.1
lr=0.0002
kl=0.1 | Clean | 0.6060 | 0.7198 | 0.0000 | 0.0000 |

|  | eps=0.03137 | 0.6210 | 0.7203 | -2.4753 | -0.0759 |

|  | eps=0.06274 | 0.6020 | 0.7037 | 0.6601 | 2.2383 |

|  | eps=0.1255 | 0.5800 | 0.6686 | 4.2904 | 7.1065 |

### 
|  | Scenario | Acc | AUROC | Acc
Drop % | AUROC
Drop % |

| --- | --- | --- | --- | --- | --- |

| img attack | Clean | 0.8362 | 0.8939 | 0.0000 | 0.0000 |

|  | eps=0.03137 | 0.7260 | 0.6725 | 13.1757 | 24.7597 |

|  | eps=0.06274 | 0.6864 | 0.5580 | 17.9054 | 37.5750 |

|  | eps=0.1255 | 0.6582 | 0.4931 | 21.2838 | 44.8398 |

### 
|  | Scenario | Acc | AUROC | Acc
Drop % | AUROC
Drop % |

| --- | --- | --- | --- | --- | --- |

| tau=10
r=8
dropout=0.1
lr=0.0002
kl=0.1 |  |  |  |  |  |

|  |  |  |  |  |  |

|  |  |  |  |  |  |

|  |  |  |  |  |  |

![slide 15](/assets/img/2025-12-29/slide15_img1.png)


### L2 Norm Distribution Comparison

### HarmC

### Clean Embeddings Statistics:

### L2 Norm - Mean: 32.6061, Std: 0.2913

### L2 Norm - Range: [31.2778, 33.3580]

### Adversarial Embeddings Statistics:

### L2 Norm - Mean: 32.5624, Std: 0.3116

### L2 Norm - Range: [31.3560, 33.2852]

### FBClean Embeddings Statistics:

### L2 Norm - Mean: 32.0168, Std: 0.2552

### L2 Norm - Range: [31.0672, 32.7405]

### Adversarial Embeddings Statistics:

### L2 Norm - Mean: 31.9813, Std: 0.2644

### L2 Norm - Range: [30.9351, 32.7237]

![slide 16](/assets/img/2025-12-29/slide16_img1.png)

![slide 16](/assets/img/2025-12-29/slide16_img2.png)


### 2. 训练阶段 (TRADES Training - V4)

### 完整的 TRADES 训练目标 (Zhang et al., ICML 2019):  其中:

### $\theta$: 模型参数 (LoRA 参数)

### $\mathcal{L}_{\text{CE}}$: 交叉熵损失

### $\mathcal{D}_{KL}$: KL 散度

### $\lambda$: 平衡系数 (你的 V4 用 LAMBDA_KL = 0.1)

### $\delta^*$: 由攻击阶段生成的对抗扰动

### 展开形式:

![slide 17](/assets/img/2025-12-29/slide17_img1.png)

![slide 17](/assets/img/2025-12-29/slide17_img2.png)


### 完整的訓練流程

### Two-Step Optimization:

### Step 1 - Inner Maximization (生成對抗樣本):

### 通過 PGD 迭代求解:

### Step 2 - Outer Minimization (更新模型):

![slide 18](/assets/img/2025-12-29/slide18_img1.png)

![slide 18](/assets/img/2025-12-29/slide18_img2.png)

![slide 18](/assets/img/2025-12-29/slide18_img3.png)

![slide 18](/assets/img/2025-12-29/slide18_img4.png)
