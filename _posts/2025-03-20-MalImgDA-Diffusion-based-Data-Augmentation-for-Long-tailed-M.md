---
title: "📖 MalImgDA_ Diffusion-based Data Augmentation for Long-tailed Malware Family Classification"
date: 2025-03-20 00:00:00 +0800
categories: [master]
tags: [paper, tech, diffusion model, malware]
---

Gang Yang; Jun He; Bo Wu; Tao Xia; Linna Fan; Lin Ni

Cite as: IEEE ICASSP 2025

Submitted on 2025/03


### Abstract

Novel Framework: The paper proposes a new data augmentation framework using a diffusion model to address long-tailed malware family classification.

Fine-Tuning: The pre-trained diffusion model is fine-tuned on few-shot data to synthesize malware images that closely resemble the target family.

Re-Balanced Dataset: Synthetic samples are combined with existing data to create a re-balanced dataset for training classifiers.

Data Diversity: The framework enhances the diversity of the data and proactively generates plausible malware variants while preserving the characteristics of the target family.

Experimental Validation: Experiments on two publicly available datasets show the effectiveness of the proposed method compared to other commonly used approaches.


### Methodology

1. Diffusion Model Pre-training and Fine-tuning

2. Diffusion Model-based Malware Image Generation

3. Dataset Re-balancing for Image Model Training

![slide 3](/assets/img/2025-03-20/slide3_img1.png)


### 1. DM Pre-training and Fine-tuning

Limitations of Data Availability: For certain malware families, there may not be enough sample data available for effective training.

Impact of Scarcity: When training data is scarce, diffusion models are prone to overfitting, which leads to a significant decline in diversity. This means that the model may learn irrelevant or non-generalizable features, making it difficult to apply its learning to new samples.

Constructing a Pre-trained Model: Researchers utilize all existing malware image data to build a pre-trained model. This model is capable of synthesizing generic malware images, helping to address the challenges posed by limited data.

Enhancing Model Understanding: After establishing a pre-trained model, fine-tuning techniques are used to refine the model’s ability to recognize and differentiate the specific features or texture styles that characterize the target malware family. This step improves the model's accuracy in classifying those specific families.


### 2. DM-based Malware Image Generation

Seed Images: Images from the target malware family are used as seeds in the fine-tuned diffusion model for iterative synthesis.

Mathematical Computations: Details of the image generation process.

Image Generation: Newly generated images closely resemble the seed images, ensuring they maintain the target family’s characteristics. These images are continuously added to the existing set, serving as new seeds for further iterations, which helps expand the dataset.

Localized Perturbations: The method utilizes the forward and inverse processes of the diffusion model, introducing small localized changes to the seed images while preserving their textures.

Re-balancing the Dataset: Once sufficient images are generated, they are combined with the original images to form a single dataset for re-balancing the representation of different malware families.


### 3. Dataset Re-balancing for Image Model Training

Apply data augmentation to the families located in the long-tail portion, thereby expanding the sample size of these families.

The long-tail distribution will gradually tend towards smoothness and balance, leading to a more evenly distributed dataset. Thus, the re-balanced dataset will be fed into the designed image model for more effective training.

![slide 6](/assets/img/2025-03-20/slide6_img1.png)


### Experiments and Analysis

![slide 7](/assets/img/2025-03-20/slide7_img1.png)

![slide 7](/assets/img/2025-03-20/slide7_img2.png)


### Experiments and Analysis

![slide 8](/assets/img/2025-03-20/slide8_img1.png)
