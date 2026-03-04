---
title: "📖 Anti-Diffusion"
date: 2025-06-04 00:00:00 +0800
categories: [master]
tags: [paper, tech, diffusion model]
---

### Anti-Diffusion

### Preventing Abuse of Modifications of Diffusion-Based Models


### ArXiv: 2503.05595

### Submitted on 2025/03

### REF: https://arxiv.org/abs/2503.05595


### Abstract

Extended Defense Scope: Expand the defense to include both tuning-based and editing-based methods, while other baselines focus only on tuning-based methods.

Prompt Tuning (PT) Strategy: Introduce the PT strategy for ensuring a better representation of protected images and providing more generalized protection for unexpected prompts.

Semantic Disturbance Loss (SDL): Integrate the SDL to disrupt the semantic information of protected images, enhancing the performance of defense against both tuning-based and editing-based methods.

Defense-Edit Dataset: Contribute a dataset called Defense-Edit for evaluating the defense performance against editing-based methods.


### Framework

![slide 3](/assets/img/2025-06-04/slide3_img1.png)


### Prompt Tuning Strategy

### Text-embedding f_j will undergo fine-tuning with the L_LDM

### By continuously optimizing the text embedding f, the model can predict the correct noise

### 沒有創建新的損失函數來優化文本嵌入，而是直接使用了原始擴散模型的標準損失函數。

### 創新點在於優化對象的選擇：他們固定了圖像編碼器和 UNet 的參數，只優化文本嵌入 f。這與傳統的擴散模型訓練（通常優化 UNet 參數）不同。

### 這種方法允許文本嵌入逐漸適應並更好地表示輸入圖像的特徵，而不需要手動選擇特定的提示詞。

![slide 4](/assets/img/2025-06-04/slide4_img1.png)


### Adversarial Noise Optimization

