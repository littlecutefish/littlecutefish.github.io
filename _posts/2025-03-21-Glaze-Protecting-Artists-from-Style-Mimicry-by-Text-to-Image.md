---
title: "📖 Glaze_ Protecting Artists from Style Mimicry by Text-to-Image Models"
date: 2025-03-21 00:00:00 +0800
categories: [master]
tags: [paper, tech, face privacy]
---

### Glaze: Protecting Artists from Style Mimicry by Text-to-Image Models

### Shawn Shan, Jenna Cryan, Emily Wenger, Haitao Zheng, Rana Hanocka, Ben Y. Zhao


### Cite as: USENIX Security 2023

### Submitted on 2023/08


### Abstract

Models can learn to mimic the artistic style of specific artists after “fine-tuning” on samples of their art.

This paper implementation and evaluation of Glaze(style cloaks) to their art before sharing online. These cloaks apply barely perceptible perturbations to images, and when used as training data, mislead generative models that try to mimic a specific artist.

![slide 2](/assets/img/2025-03-21/slide2_img1.png)


### Disrupting Style Mimicry with Glaze

Transfer image(x) into target style T.

By minimizing this distance, we can ensure that the generated artwork matches the target style in style.

![slide 3](/assets/img/2025-03-21/slide3_img1.png)

![slide 3](/assets/img/2025-03-21/slide3_img2.png)

![slide 3](/assets/img/2025-03-21/slide3_img3.png)

![slide 3](/assets/img/2025-03-21/slide3_img4.png)


### Detailed System Design

### Step 1: Choose Target Style

Select a target style (T) to disrupt mimicry.

Use a public dataset to find different styles.

Randomly choose a style that is 50-75% different from the artist's original style.

### Step 2: Style Transfer

Use a style transfer model (Ω) to convert each artwork (x) into the target style (T).

This creates a new version of the artwork, Ω(x, T).

### Step 3: Compute Cloak Perturbation

Calculate the perturbation (δx).

Optimize δx using LPIPS to keep it visually similar to the original.

Minimize the difference between the cloaked artwork and the style-transferred version.

Brings the style-transferred image closer to the target style.

Ensures that the generated image remains visually similar to the original artwork.

![slide 4](/assets/img/2025-03-21/slide4_img1.png)


### Evaluation

### Participating artists: 1156

### No

### 92%

### Yes

### 93%

### Yes

### 88%

![slide 5](/assets/img/2025-03-21/slide5_img1.png)

![slide 5](/assets/img/2025-03-21/slide5_img2.png)

![slide 5](/assets/img/2025-03-21/slide5_img3.png)


### Evaluation Metrics

### Glaze has a high protection success rate

![slide 6](/assets/img/2025-03-21/slide6_img1.png)


### Result

![slide 7](/assets/img/2025-03-21/slide7_img1.png)
