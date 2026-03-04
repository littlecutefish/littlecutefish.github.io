---
title: "📖 Thesis Progress-v2"
date: 2026-01-29 00:00:00 +0800
categories: [master]
tags: [progress, tech, thesis]
---

### Thesis Progress

### V2


### Contents

### Introduction & Motivation

### Research Methods

### Research Results

### Conclusion

### Introduction & Motivation

### 01


### 1.1 Background

### 仇恨迷因的興起與危害：隨著社群媒體發展，利用網路散播仇恨言論的情況日益嚴重 。這類內容結合了圖片與文字，具有極強的傳播力與煽動性。

### 多模態理解的必要性：迷因的特殊之處在於，單獨看圖片或單獨看文字可能都是無害的，但兩者結合後卻可能產生極具攻擊性的負面涵義。

### best holiday gift …

### for muslims

### Is this meme harmful?

![slide 4](/assets/img/2026-01-29/slide4_img1.png)


### 1.2 Motivation

### RGCL 的脆弱性：依賴 CLIP Image Encoder 提取的特徵來檢索輔助樣本，PGD 攻擊所施加的像素級擾動，會導致圖片在高維特徵空間中發生位移。


### Vulnerable

### Clean Embeddings:

### L2 Norm Mean: 33.0717

### Range: [31.9634, 34.6611]

### Adversarial Embeddings:

### L2 Norm Mean: 32.3229

### Range: [29.1735, 34.9629]

![slide 5](/assets/img/2026-01-29/slide5_img1.png)

![slide 5](/assets/img/2026-01-29/slide5_img2.png)


### 1.3 Research Objective

### 如何在不犧牲 Clean Accuracy 的情況下，提升模型對像素級攻擊的魯棒性？

### 參數高效微調 (PEFT) 能否作為有效的防禦手段？

### 語義錨點 (Entity Grounding) 能否進一步增強穩健性？

### 核心貢獻：

### 揭露 RGCL 在 PGD 攻擊下的極度脆弱性

### 提出基於 Adv-LoRA 的參數高效防禦機制

### 引入 Entity Grounding 作為輔助語義監督

### Research Methods

### 02


### 2.1 Overall Framework

![slide 8](/assets/img/2026-01-29/slide8_img1.png)


### 2.2 Threat Model

### Min-Max Saddle Point(鞍點)：

### PGD 迭代更新：

### Eps(ε)=[8/255, 16/255, 32/255]

### Steps = 100

### Step size(α) = ε/10

![slide 9](/assets/img/2026-01-29/slide9_img1.png)

![slide 9](/assets/img/2026-01-29/slide9_img2.png)


### 2.3 Defense: Adv-LoRA

### LoRA

### 原始權重矩陣：W ∈ Rd×k

### W ′ = W + ∆W = W + BA

### Adv-LoRA (Adversarial Training)

### Class-Prompt 設計：

### Hateful: "a photo of hateful content"

### Benign: "a photo of normal content"

### 訓練目標：

### 優勢：

### r << min(d, k)，參數量大幅減少

### 不破壞預訓練特徵

### 訓練效率高

### 確保模型在乾淨圖像上分類正確

### 確保模型在對抗樣本上仍能分類正確

### Parameters:

### Eps = [2.0/255, 4.0/255]

### Iters = [2, 5]

### LoRA rank = [8, 16]

### Batch size = 16

### Epochs = 5

![slide 10](/assets/img/2026-01-29/slide10_img1.png)

![slide 10](/assets/img/2026-01-29/slide10_img2.png)

![slide 10](/assets/img/2026-01-29/slide10_img3.png)

![slide 10](/assets/img/2026-01-29/slide10_img4.gif)


### 2.4 Entity Grounding

### 生成 LLaVA captions

### Entity Extraction


| {
    "57630.png": "The meme features a small piglet with a pinkish hue, standing upright on its hind legs with its front legs stretched out in front of it. The piglet has a contented expression, with its mouth slightly open as if it's smiling or panting. The background is a plain, light color, which contrasts with the piglet, making it the focal point of the image.\n\nThe text overlay on the meme reads \"best holiday gift for muslims.\" The text is in a bold, sans-serif font, with the words \"best holiday gift\" in a larger size than \"for muslims.\" The phrase \"for muslims\" is in a smaller font size and is positioned below the main text.\n\nThe meme appears to be a humorous take on holiday gift-giving, suggesting that a piglet might be an appropriate or amusing gift for Muslims during the holiday season. The meme is likely intended to be light-hearted and playful, rather than serious or offensive. The piglet's innocent and cheerful demeanor, combined with the unexpected suggestion of a piglet as a holiday gift, creates a whimsical and humorous effect.",
…
} |

| --- |


| {
    "id": "57630",
    "res": {
        "sent": "best holiday gift for muslims the meme features a small piglet with a pinkish hue standing upright on its hind legs...",
        "qc": ["pig", "holiday", "gift", "muslim", "meme", "humor", "christmas", "smile", ...]
    }
} |

| --- |

Li, Feng, et al. "Llava-next-interleave: Tackling multi-image, video, and 3d in large multimodal models." arXiv preprint arXiv:2407.07895 (2024).


### 2.4 Entity Grounding

### PGD 攻擊會擾亂視覺特徵，但語義概念相對穩定

### Entity InfoNCE Loss

### 目標：讓 model embedding 與 entity embedding 對齊

### ei 為 model embedding（projected）

### pi 為 entity embedding（projected）

### τ 為溫度參數（此處 τ = 0.1）

### Total Loss：

### α = 0.5（CE 權重）

### β ∈ {0.1, 0.3, 0.5}（Entity 權重）

Oord, Aaron van den, Yazhe Li, and Oriol Vinyals. "Representation learning with contrastive predictive coding." arXiv preprint arXiv:1807.03748 (2018).

![slide 12](/assets/img/2026-01-29/slide12_img1.png)

![slide 12](/assets/img/2026-01-29/slide12_img2.png)

![slide 12](/assets/img/2026-01-29/slide12_img3.png)

### Research Results

### 03


### 3.1 Experiment Settings

### 資料集：

### 模型配置：

### Backbone: CLIP ViT-L/14@336

### Fusion: Align (element-wise multiply)

### MLP: 3 layers, dim=1024

### 評估指標： Accuracy, AUROC

### 
|  | FB | HarmC (Harmeme) |

| --- | --- | --- |

| Train/Dev/Test | 8500/500/1000 | 3013/177/354 |

| Hate % | 37.56 | 26.21 |

| Domin | In the wild | Covid-19 |

| Bit depth (Avg.) | 9.54 | 43.90 |


### Exp 1: RGCL model under PGD-100 attacks

### 
|  | Scenario | Accuracy | AUROC |

| --- | --- | --- | --- |

| Baseline | Clean | 0.8192 | 0.8848 |

|  | pgd_e=0.03137 | 0.7345 | 0.7362 |

|  | pgd_e=0.06274 | 0.7006 | 0.6558 |

|  | pgd_e=0.1255 | 0.7288 | 0.6630 |

### Harmeme

### FB

### 
|  | Scenario | Accuracy | AUROC |

| --- | --- | --- | --- |

| Baseline | Clean | 0.7380 | 0.8529 |

|  | pgd_e=0.03137 | 0.6380 | 0.6997 |

|  | pgd_e=0.06274 | 0.5710 | 0.6121 |

|  | pgd_e=0.1255 | 0.5530 | 0.5712 |


### Exp2: Full Result

### Harmeme

### AUROC 對照：

### 
| Method | Clean Acc | Clean AUROC | ε=8/255 Acc | ε=16/255 Acc | ε=32/255 Acc |

| --- | --- | --- | --- | --- | --- |

| RGCL (Baseline) | 0.8192 | 0.8848 | 0.7345 | 0.7006 | 0.7288 |

| +Adv-LoRA (r=8,i=2) | 0.8305 | 0.8950 | 0.8305 | 0.8277 | 0.8277 |

| +Adv-LoRA+Ent(0.5) | 0.8531 | 0.9194 | 0.8475 | 0.8446 | 0.8446 |

### 
| Method | Clean | ε=8/255 | ε=16/255 | ε=32/255 |

| --- | --- | --- | --- | --- |

| RGCL (Baseline) | 0.8848 | 0.7362 | 0.6558 | 0.6630 |

| +Adv-LoRA | 0.8950 | 0.8920 | 0.8923 | 0.9004 |

| +Adv-LoRA+Ent(0.5) | 0.9194 | 0.9150 | 0.9151 | 0.9232 |


### Ablation Study (Harmeme)

### LoRA Hyperparameter

### Entity Weight

### 
| LoRA Config | Clean Acc | Clean AUROC | Robust Acc (ε=8/255) | Robust AUROC | AUROC Drop |

| --- | --- | --- | --- | --- | --- |

| No LoRA | 0.8277 | 0.9232 | 0.7373 | 0.7074 | -23.4% |

| r=8, iter=2 | 0.8531 | 0.9194 | 0.8475 | 0.9150 | -0.5% |

| r=8, iter=5 | 0.8362 | 0.9091 | 0.7825 | 0.8112 | -10.8% |

| r=16, iter=2 | 0.8277 | 0.9232 | 0.8305 | 0.9228 | -0.04% |

| r=16, iter=5 | 0.8418 | 0.9159 | 0.8220 | 0.8603 | -6.1% |

### r=16, iter=2 + Entity 0.5 達到最佳魯棒性

### LoRA 顯著提升 Robust Accuracy

### iter=5 不一定比 iter=2 好（可能過擬合）

### 
| Entity Weight | Clean Acc | Clean AUROC | Robust Acc (ε=8/255) | Robust AUROC | Δ AUROC |

| --- | --- | --- | --- | --- | --- |

| 0.0 (Baseline) | 0.8305 | 0.8950 | 0.8305 | 0.8920 | baseline |

| 0.1 | 0.8503 | 0.8749 | 0.8362 | 0.8908 | -0.1% |

| 0.3 | 0.8362 | 0.9039 | 0.8277 | 0.9066 | +1.6% |

| 0.5 | 0.8531 | 0.9194 | 0.8475 | 0.9150 | +2.6% |

### Entity 0.5 在 LoRA r=8 iter=2 下達到最佳效果

### Entity 提升 Clean AUROC（0.8950 → 0.9194）

### Entity 同時提升 Robust AUROC（0.8920 → 0.9150）

### Conclusion

### 04


### Research Contributions

### 1. 揭露問題

### 首次系統性分析 RGCL 對 PGD 攻擊的脆弱性

### 證明語義級防禦無法抵抗像素級攻擊

### 2. 提出解決方案

### Adv-LoRA：參數高效的對抗防禦（僅 0.252% 參數）

### Entity Grounding：語義錨點輔助監督

### 3. 實驗驗證


### TODO

解決多模態模型（如 CLIP）常見的「語義鴻溝」（Semantic Gap）問題——即模型雖然能看懂圖像的像素（Pixels）和文字的字面意思（Tokens），但往往缺乏圖像背後的隱性背景知識（Implicit Context）。
