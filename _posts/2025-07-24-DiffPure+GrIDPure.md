---
title: "📖 DiffPure+GrIDPure"
date: 2025-07-24 00:00:00 +0800
categories: [master]
tags: [paper, tech, adversarial purification]
---

### 1. (DiffPure) Diffusion Models for Adversarial Purification
ICML (2022)

Submitted on 2022/04

### 2. (GrIDPure) Can Protective Perturbation Safeguard Personal Data from Being Exploited by Stable Diffusion?
CVPR (2024)

Submitted on 2023/11


REF:

https://arxiv.org/abs/2205.07460

https://arxiv.org/abs/2312.00084

### 1. DiffPure


### Abstract

- DiffPure 是第一個利用預訓練擴散模型的前向和逆向過程來淨化對抗性影像的對抗性淨化方法。

- 前向擴散過程（Forward Diffusion Process）：給定一個對抗性範例，首先通過少量噪聲進行擴散。
→ 理論分析表明，前向擴散過程能使乾淨數據分佈和對抗性擾動數據分佈更接近，從而「沖刷掉」對抗性擾動。

- 逆向生成過程（Reverse Generative Process）：然後通過逆向生成過程恢復乾淨影像。

- 擴散時間步長 (t∗) 的選擇：重要設計參數，它代表了前向過程中添加的噪聲量。
→ 理論分析顯示，噪聲需要足夠高才能消除對抗性擾動，但又不能太大以免破壞淨化影像的標籤語義。

- 梯度計算：為了有效且可擴展地評估該方法對抗強自適應攻擊的能力，論文提出使用伴隨方法（adjoint method）來計算逆向生成過程的完整梯度。


### illustration

![slide 5](/assets/img/2025-07-24/slide5_img1.png)


### Method (Diffusion purification)

Theorem 3.1: 存在一個最小時間步長 t∗∈[0,1]，使得 DKL​(pt∗​∣∣qt∗​)≤ϵ 。

![slide 6](/assets/img/2025-07-24/slide6_img1.png)

在 t∗ 時，對抗性擾動已經被有效移除，因為對抗性樣本分佈與乾淨數據分佈足夠接近 。

- Step 1. 擴散對抗性範例（Diffuse Adversarial Examples）

對於特定的 VP-SDE (Variance Preserving Stochastic Differential Equation)，在擴散時間步長 t∗∈[0,1] 處的擴散對抗性樣本可以使用公式(3)。

![slide 6](/assets/img/2025-07-24/slide6_img2.png)

- Step 2. 恢復乾淨影像（Recover Clean Images）

DiffPure 利用擴散模型的去噪階段，但由於逆向 SDE 沒有閉合形式解，直接反向傳播會引發記憶體問題。因此DiffPure 引入了伴隨方法(Adjoint Method)來處理 SDE 求解器的梯度計算。



![slide 6](/assets/img/2025-07-24/slide6_img3.png)

![slide 6](/assets/img/2025-07-24/slide6_img4.png)


### Method (Diffusion purification)

Theorem 3.2:

量化擴散時間步長 t∗ 對於 DiffPure 淨化後影像品質的影響 。它透過數學不等式來描述，從原始乾淨影像 x 到經 DiffPure 淨化後的影像 x^ (0) 之間的 L2 距離 。

這個定理直接回應了論文中提到的 t∗  選擇的兩難：即 t∗ 既要夠大以去除對抗性擾動，又不能太大而破壞影像的全局語義 。

![slide 7](/assets/img/2025-07-24/slide7_img1.png)


### Result

![slide 8](/assets/img/2025-07-24/slide8_img1.png)

![slide 8](/assets/img/2025-07-24/slide8_img2.png)

### 2. GrIDPure


### Abstract

提出新的淨化方法GrIDPure：論文進一步介紹了一種新的淨化方法，能夠在最大程度保留原始圖像結構的同時，移除這些保護性擾動 。

實驗證明，Stable Diffusion可以有效地從經過淨化的圖像中學習，無論是針對哪種保護方法 。


### Comparison of different fine-tuning methods

這圖展示了不同微調方法在 Stable Diffusion 模型中修改了哪些核心組件，尤其區分了是否訓練文本編碼器對這些方法的影響 。

只微調文本編碼器的方法（如 Textual Inversion）對保護性擾動的魯棒性最差 。

![slide 11](/assets/img/2025-07-24/slide11_img1.png)

![slide 11](/assets/img/2025-07-24/slide11_img2.png)


### Method

- GrIDPure 是一種專門設計用於在去除圖像中保護性擾動的同時，保留圖像分辨率和複雜細節的淨化方法 。

- GrIDPure 通過「GDP 淨化」與「與原圖混合」的迭代過程，逐步地、精細地去除圖像中的保護性擾動，最終生成一個高質量且淨化徹底的圖像。

- GDP (Grid Diffusion-based Purification)：這是一個關鍵的步驟，代表「基於網格的擴散淨化」 。

![slide 12](/assets/img/2025-07-24/slide12_img1.png)

![slide 12](/assets/img/2025-07-24/slide12_img2.png)


### Grid Diffusion-Based Purification (GDP)

1. Grid Crop (網格裁剪):
重疊裁剪 (stride = 128)，淨化模型（UDM）是為特定分辨率（例如 256×256）訓練的，所以輸入必須是這個尺寸 。

2. Unconditional Diffusion Model (256x256) (無條件擴散模型):

3. Average Merge (平均合併):

![slide 13](/assets/img/2025-07-24/slide13_img1.png)


### Result

![slide 14](/assets/img/2025-07-24/slide14_img1.png)

![slide 14](/assets/img/2025-07-24/slide14_img2.png)

![slide 14](/assets/img/2025-07-24/slide14_img3.png)
