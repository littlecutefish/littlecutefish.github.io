---
title: "📖 Weekly Progress - Paper Reading & Survey"
date: 2025-02-24 00:00:00 +0800
categories: [master]
tags: [diffusion model, adversarial attack, malware, face privacy, paper, tech]
---

## Paper #1: MalImgDA

**MalImgDA: Diffusion-based Data Augmentation for Long-tailed Malware Family Classification**

> Gang Yang; Jun He; Bo Wu; Tao Xia; Linna Fan; Lin Ni
> 2025 IEEE International Conference on Acoustics, Speech, and Signal Processing (ICASSP), Published 2025/03

**Problem:**
- Imbalanced or long-tailed distribution among various malware families.
- Difficulty in classifying few-shot malware families, which reduces classifier effectiveness.

**Proposed Solution:** MalImgDA

**Results:**
- Experiments on two publicly available datasets demonstrate the effectiveness of MalImgDA.
- Outperforms other commonly-used methods in malware classification.

### Methodology

![MalImgDA overview](/assets/img/2025-02-24/slide4_img1.png)

![MalImgDA pipeline](/assets/img/2025-02-24/slide4_img2.png)

1. Diffusion Model Pre-training and Fine-tuning
2. Diffusion Model-based Malware Image Generation
3. Dataset Re-balancing for Image Model Training

![Re-balancing](/assets/img/2025-02-24/slide4_img3.png)

---

## Paper #2: ADAF

**Towards Prompt-robust Face Privacy Protection via Adversarial Decoupling Augmentation Framework**

> Ruijia Wu, Yuhang Wang, Huafeng Shi, Zhipeng Yu, Yichao Wu, Ding Liang
> ArXiv:2305.03980 [cs.CV], Published 2023/05

**Problem:**
- The image-text fusion module is a vital component of text-to-image diffusion models, responsible for combining textual information with the image generation process.
- Current privacy protection methods neglect this crucial fusion module, leading to unstable defense performance against different attacker prompts.

**Proposed Solution:** Adversarial Decoupling Augmentation Framework (ADAF)

**Results:**
- ADAF outperforms existing algorithms, demonstrating promising performance in enhancing facial privacy protection in text-to-image models.

### Abstract

Text-to-image diffusion models methods are unstable against different attacker prompts.

![ADAF abstract](/assets/img/2025-02-24/slide6_img4.png)

### Methodology

![ADAF methodology](/assets/img/2025-02-24/slide7_img5.png)

#### Vision-Adversarial Loss

$$L_{VAL} = \|x_{rec} - x_{target}\|_2$$

The goal is to bring the reconstructed image closer to the target image, distancing it from training samples.
→ Aims to improve generation quality, reduce overfitting, enhance model robustness, and facilitate effective feature learning.

#### Prompt-Robust Augmentation (PRA)

text-to-image: "A cute cat sitting by the window."
Overfitting problem: "a cat playing in the garden" → fail

**1. At the Text Input Level** - Reducing Overfitting
- Use two special characters:
  - Underscore: `"A cute cat_sitting by the window."`
  - Empty: `"A cute cat,   "`

**2. At the Text Feature Level** - Diversifying Text
- When generating adversarial perturbations, perform data augmentation on the text features.
- Ex. "A cat in a tree" → "A cat in a tree, possibly with a dog."

#### Attention Decoupling Loss (ADL)

Focus on the attention matrix within the cross-attention mechanism to interfere with the image-text fusion process and prevent the network from establishing effective image-text mapping relationships.

![ADL formula](/assets/img/2025-02-24/slide10_img7.png)

![ADL equation](/assets/img/2025-02-24/slide10_img8.png)

#### Total Loss

![Total loss](/assets/img/2025-02-24/slide11_img9.png)

### Experiments

![Experiment results table](/assets/img/2025-02-24/slide12_img10.png)

![Experiment visual results](/assets/img/2025-02-24/slide13_img11.png)

---

## Paper #3: Glaze

**Glaze: Protecting Artists from Style Mimicry by Text-to-Image Models**

> Shawn Shan, Jenna Cryan, Emily Wenger, Haitao Zheng, Rana Hanocka, Ben Y. Zhao
> 2023 USENIX Security, Published 2023/08

![Glaze overview](/assets/img/2025-02-24/slide14_img12.png)

### Disrupting Style Mimicry with Glaze

Transfer image(x) into target style T. By minimizing this distance, we can ensure that the generated artwork matches the target style in style.

![Style transfer process](/assets/img/2025-02-24/slide15_img13.png)

![Formula 1](/assets/img/2025-02-24/slide15_img14.png)

![Formula 2](/assets/img/2025-02-24/slide15_img15.png)

### Detailed System Design

**Step 1: Choose Target Style**
- Select a target style (T) to disrupt mimicry.
- Use a public dataset to find different styles.
- Randomly choose a style that is 50-75% different from the artist's original style.

**Step 2: Style Transfer**
- Use a style transfer model (Ω) to convert each artwork (x) into the target style (T).
- This creates a new version of the artwork, Ω(x, T).

**Step 3: Compute Cloak Perturbation**
- Calculate the perturbation (δx).
- Optimize δx using LPIPS to keep it visually similar to the original.
- Minimize the difference between the cloaked artwork and the style-transferred version.

![Cloak perturbation formula](/assets/img/2025-02-24/slide16_img17.png)

- Brings the style-transferred image closer to the target style.
- Ensures that the generated image remains visually similar to the original artwork.

### Evaluation

Glaze has a high protection success rate.

![Evaluation metrics](/assets/img/2025-02-24/slide17_img18.png)

### Results

![Glaze results](/assets/img/2025-02-24/slide18_img19.png)

---

## Survey of Large Model Safety

### Intellectual Property Protection

- **Natural Data Protection:** DUAW, AdvDM, Anti-Dreambooth, Numwan's research...
- **Generated Data Protection**
- **Model Protection**

![IP Protection taxonomy](/assets/img/2025-02-24/slide19_img20.png)

![IP Protection details](/assets/img/2025-02-24/slide19_img21.png)

### Backdoor Attack & Defense

![Backdoor Attack & Defense](/assets/img/2025-02-24/slide20_img22.png)
