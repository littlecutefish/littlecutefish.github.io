---
title: "📖 DisDiff (Disrupting Diffusion)+Advdm"
date: 2025-05-15 00:00:00 +0800
categories: [master]
tags: [paper, tech, diffusion model, adversarial attack]
---


Liu et al. 2024.

ACM International Conference on Multimedia (MM ’24)

REF: Disrupting Diffusion: Token-Level Attention Erasure Attack against Diffusion-based Customization

### Review about protect methods

GLAZE, AdvDM and Numwan’s thesis: employ invisible perturbations on personal images, protecting users from style theft and painting imitation.

-> Focuses on disrupting style extraction.

Anti-Dreambooth, SimAC, Dis-Diff: generate adversarial noises and protects personal identities from being used in fake image synthesis.

-> Focuses on disrupting personalized model training processes.

Comparison of DreamBooth, Anti-DreamBooth with DisDiff.

![slide 2](/assets/img/2025-05-15/slide2_img1.png)

![slide 2](/assets/img/2025-05-15/slide2_img2.png)


### Analysis of varying timesteps

![slide 3](/assets/img/2025-05-15/slide3_img1.png)

![slide 3](/assets/img/2025-05-15/slide3_img2.png)


### Framework

### Key Technical Innovations:

Cross-Attention Erasure Module: DisDiff erases the model’s attention on “sks” token and dramatically distorts the model’s outputs.

Merit Sampling Scheduler: Adaptively modulates perturbation updating amplitude in a step-aware manner.

### Cross-Attention Erasure loss:

Time-dependent function to adjust the perturbation updating steps in PGD adaptively.

### Overall Projected Gradient Descent:

![slide 4](/assets/img/2025-05-15/slide4_img1.png)

![slide 4](/assets/img/2025-05-15/slide4_img2.png)

![slide 4](/assets/img/2025-05-15/slide4_img3.png)

![slide 4](/assets/img/2025-05-15/slide4_img4.png)


### Performance

- FDFR (Face Detection and Face Recognition): 評估模型防止人臉識別的能力

- ISM (Identity Similarity Metric): 測量生成圖像與原始身份的相似度

- FID (Fréchet Inception Distance): 評估生成圖像與真實圖像分佈之間的差異

- BRISQUE (Blind/Referenceless Image Spatial Quality Evaluator): 評估圖像的無參考質量

![slide 5](/assets/img/2025-05-15/slide5_img1.png)
