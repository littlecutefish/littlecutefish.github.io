---
title: "📖 Impress+CAT"
date: 2025-06-12 00:00:00 +0800
categories: [master]
tags: [paper, tech]
---

### Progress


### Paper

### NeurIPS (2023)

### Submitted on 2023/10

1. IMPRESS: Evaluating the Resilience of Imperceptible Perturbations Against Unauthorized Data Usage in Diffusion-Based Generative AI

### ArXiv:2502.07225

### Submitted on 202502

2. CAT: Contrastive Adversarial Training for Evaluating the Robustness of Protective Perturbations in Latent Diffusion Models

### REF:

### https://arxiv.org/abs/2310.19248

### https://arxiv.org/abs/2502.07225

### 1. IMPRESS


### IMPRESS: Abstract

Analysis: Identified vulnerabilities in existing protection methods - imperceptible perturbations create detectable inconsistencies in diffusion-reconstructed images.

IMPRESS Platform: Developed system to purify protected images using consistency-based loss functions.

Adaptive Protection: Demonstrated that even enhanced protection methods struggle to create effective perturbations, highlighting need for more advanced protection mechanisms.


### IMPRESS: Framework

### 相似性損失 (Similarity Loss)

### 確保淨化後的圖像 (xpur) 在視覺上與受保護的圖像 (xptb) 保持相似。

### 一致性損失 (Consistency Loss)

### 目的：確保淨化後的圖像 (xpur) 經過編碼器 (ℰ) 和解碼器 (𝒟) 重建後，仍然與自身保持一致。

### 通過同時優化這兩種損失函數，IMPRESS 能夠有效地移除保護性擾動，同時保留圖像的原始視覺特徵，從而使擴散模型能夠正確處理這些圖像。

![slide 5](/assets/img/2025-06-12/slide5_img1.png)

![slide 5](/assets/img/2025-06-12/slide5_img2.png)


### IMPRESS: Algorithm

![slide 6](/assets/img/2025-06-12/slide6_img1.png)

### 2. CAT


### CAT: Abstract

Identify that the primary reason adversarial examples effectively serve as protective perturbations in LDM customization is the distortion of their latent representations.

Propose the Contrastive Adversarial Training (CAT) utilizing adapters as an adaptive attack against these protection methods, highlighting their lack of robustness.

CAT method significantly reduces the effectiveness of protective perturbations in customization configurations.


### CAT Framework

### CAT的新想法：不是嘗試「淨化」受保護的圖像，

### 而是調整模型本身來適應這些受保護的圖像。

### Φ0 = 原始 VAE 編碼器 (Encoder) 的、被凍結的權重 。

### Adapter (Δϕ) 看到輸入的圖片 xa 後，會計算出一個「修正量」，專門用來抵銷保護性擾動所造成的扭曲的。

### za(cat) = 這是最終的輸出。它等於「原始的扭曲結果」(ϕ0xa) 加上 「Adapter 計算出的修正量」(Δϕxa)。

![slide 9](/assets/img/2025-06-12/slide9_img1.png)

![slide 9](/assets/img/2025-06-12/slide9_img2.png)

![slide 9](/assets/img/2025-06-12/slide9_img3.png)


### CAT Framework

![slide 10](/assets/img/2025-06-12/slide10_img1.png)


### Experiments

### python code/finetune_dreambooth_cat/finetune_dreambooth_cat.py \

### --exp_type=cat \

### --dataset=wikiart \

### --adv_type=mist \

### --target_module=both \

### --cat_adapter_rank=4 \

### --cat_adapter_train_steps=2000 \

### --class_data_dir=dataset/db-reference-wikiart \

### --subfolder=camille-pissarro
