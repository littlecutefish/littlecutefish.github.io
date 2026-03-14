---
title: "📖 AdvCLIP-LoRA"
date: 2025-12-08 00:00:00 +0800
categories: [master]
tags: [paper, tech, adversarial attack, CLIP]
---

Cite as: ArXiv(2505.15130)

Submitted on 2025/09


### Abstract

問題：CLIP 這類大型 VLM 模型很容易受到 Adversarial Attack 攻擊 。

雖然 Full Fine-Tuning 可以提升防禦力，但成本太高；而現有的 PEFT 方法（如 Adversarial Prompt Tuning）在 Few-shot 的時候，雖然提升了魯棒性，但 Clean Accuracy 掉得很慘 。

解法：作者提出了 AdvCLIP-LoRA，這是第一個結合 LoRA 與 Adversarial Training 來做 Few-shot VLM 微調的方法 。


![slide 3](/assets/img/2025-12-08/slide3_img1.png)


### Algorithm

![slide 4](/assets/img/2025-12-08/slide4_img1.png)
