---
title: "📖 Thesis progress v3"
date: 2026-02-16 00:00:00 +0800
categories: [master]
tags: [progress, tech, thesis]
---

### Thesis Progress


### 1150216

### v3


### 遇到的問題

### 找不到比較論文的定位

### Ex1. 跟hateful meme detection 比較，準確率沒有提升

### Ex2. 如果只比較adv robust 的話，沒有意義，加入adv training後，準確率掉更多


### 重新定位

### 目標1: 先比較SOTA hateful meme論文，思考如何提升準確率

### 目標2: 嘗試結合adv training，看看有沒有辦法在不降低準確率上，還能夠讓模型robust

AdvKD-RGCL: Robust Retrieval-Guided Hateful Meme Detection with Adversarial LoRA Adaptation and VLM-Based Knowledge Distillation

### 基於對抗式低秩適應與視覺語言模型知識蒸餾的魯棒仇恨迷因偵測

### 1150216


### Outline

### Hateful Meme Detection

### RGCL 架構回顧 — Base Model

### Problem & Motivation — RGCL 的弱點

### Background — LLaVA, Knowledge Distillation, Adversarial Robustness & LoRA, AdvCLIP-LoRA

### Proposed Framework

### Solution 1: Label-Level KD — 在 label level 轉移 VLM 知識

### Solution 2: Adversarial LoRA — 用 LoRA 增強對抗魯棒性

### Experiment Design — 三個資料集、四種配置

### Results — Clean performance + 對抗魯棒性

### Compare Results with other papers

### 參數配置


### 1. Hateful Meme Detection

### What is Hateful Meme？

### 圖片 + 文字的多模態內容，用於傳達仇恨、歧視、攻擊性訊息

### 核心挑戰：圖文分開看都無害，組合起來才有害（cross-modal irony）

### 方法演進：

### Unimodal：只用文字（BERT, LSTM）或只用圖片（ResNet, VGG）

### Multimodal Fusion：圖文分別編碼再融合(HateCLIPper, MOMENTA)、圖文一起輸入 transformer(VisualBERT, UNITER, VILLA)

### VLM-Enhanced/Knowledge-Augmented：用大型 Vision-Language Model 提供額外知識(Pro-Cap, KIDDIN', PromptHate)

### Retrieval-Augmented +LMM-based：用相似樣本輔助判斷(RGCL, RA-HMD)、直接用大型多模態模型做分類

### 
| 挑戰 | 說明 |

| --- | --- |

| Cross-modal irony | 文字說 "love"，圖片暗示相反意思 |

| Implicit hate | 不直接使用攻擊性詞彙，依賴文化背景理解 |

| Benign confounders | 換圖或換文就變無害 → 需要真正理解圖文關係 |


### 2. RGCL 架構回顧 — Base Model

### How can I improve this hateful meme detection model?

### Contrastive loss：建立的類別分離結構

### Contrastive loss = ReLU(negative_sim - positive_sim + margin)

### Cross Entropy loss：直接優化分類準確率

### loss_CE = BCEWithLogitsLoss(output, labels)

### FAISS 找到的最近的異類

### FAISS 找到的最近的同類

![slide 7](/assets/img/2026-02-16/slide7_img1.png)

![slide 7](/assets/img/2026-02-16/slide7_img2.png)

### Contrastive learning 只能重新「排列」CLIP 給的 features，讓同類靠近、異類遠離

### CLIP features 遺漏 irony(反諷), sarcasm(挖苦), implicit intent(隱含意圖)

### LLaVA 能提供 200-400 字的詳細分析


### 3. Problem & Motivation — RGCL's Weakness

### CLIP-based 模型對 adversarial perturbation 極度脆弱

### Weakness 1：Semantip Gap

### Weakness 2：Adversarial Vulnerability

### 我們的方案：兩個互補的增強策略

### Label KD 填補 semantic gap + Adversarial LoRA 提升 robustness

REF: Yan, Mengqi. "Application Analysis of Multimodal Models in Hateful Meme Detection." ITM Web of Conferences. Vol. 78. EDP Sciences, 2025.

使用 LLaVA 產生的 caption，去做一個 caption-based teacher model，讓它幫每個 meme 產生一個 soft label（hateful 的機率值），然後用 Knowledge Distillation 把這個知識傳給 RGCL。

![slide 8](/assets/img/2026-02-16/slide8_img1.png)


### 4a. Background — LLaVA

### (Large Language and Vision Assistant)

### What is LLaVA？

### 7B params Vision-Language Model

### Combine CLIP visual encoder + LLaMA language model

### CLIP 看到的：

### → "豬" + "for muslims" (表面特徵)

### LLaVA 看到的：

### → Target: 穆斯林群體

### → Intent: 用豬的宗教禁忌來嘲諷

### → Offensiveness: 9/10

### But: LLaVA too big → 無法 real-time 部署

### Our Motivation：透過 Knowledge Distillation 將 LLaVA 的knowledge轉移到輕量級的 RGCL 模型中

![slide 9](/assets/img/2026-02-16/slide9_img1.png)

![slide 9](/assets/img/2026-02-16/slide9_img2.png)


### 4b. Background — Knowledge Distillation

### 只對齊最終輸出（logit level）

### 不干擾中間層 embedding space

### 適合 contrastive learning 框架

### 三種KD的類型：

### 我們的選擇：Response-based

### Distilling the Knowledge in a Neural Network: 讓小模型學習大模型的 "dark knowledge"

### 
| 類型 | 對齊 |

| --- | --- |

| Response-based | 輸出logits |

| Feature-based | 中間特徵層 |

| Relation-based | 樣本間關係 |

### https://zhuanlan.zhihu.com/p/586364069

### 代入 T=1.0

### 因為 teacher 本身 confidence 就不高（AUC 0.57-0.63），再軟化反而加入更多 noise。

![slide 10](/assets/img/2026-02-16/slide10_img1.png)

![slide 10](/assets/img/2026-02-16/slide10_img2.png)

![slide 10](/assets/img/2026-02-16/slide10_img3.png)

![slide 10](/assets/img/2026-02-16/slide10_img4.png)


### 4b. Background — Knowledge Distillation

### Feature-level KD 無法改善 contrastive structure（甚至略微降低），而 Label-level KD 顯著提升了 class separation

![slide 11](/assets/img/2026-02-16/slide11_img1.png)


### 4c. Background — Adversarial Robustness & LoRA

### Low-Rank Adaptation：凍結預訓練模型，只訓練低秩矩陣

### ,                         ,

### 優點：

### <1% 額外參數即可適應新任務

### 保持預訓練知識（避免 catastrophic forgetting）

### 可合併回原模型，零推理成本

### Add invisible perturbation → 模型分類錯誤

### PGD Attack：

### White-box adversarial attack

### 多步迭代，maximize loss in embedding space

### RAP Attack：

### Black-box adversarial attack

### 用 surrogate model 生成對抗樣本，不需要目標模型 gradient

### 先加 reverse perturbation 再攻擊，提高跨模型遷移性

### 我們用兩種攻擊評估 robustness

### Adversarial Examples

### LoRA

![slide 12](/assets/img/2026-02-16/slide12_img1.png)

![slide 12](/assets/img/2026-02-16/slide12_img2.png)

![slide 12](/assets/img/2026-02-16/slide12_img3.png)

![slide 12](/assets/img/2026-02-16/slide12_img4.png)

![slide 12](/assets/img/2026-02-16/slide12_img5.png)


### 4d. Background — AdvCLIP-LoRA

### Combine Adversarial Training and LoRA

### Minimax 訓練公式:

REF: Ghiasvand, Sajjad, et al. "Few-shot adversarial low-rank fine-tuning of vision-language models." arXiv preprint arXiv:2505.15130 (2025).

### 內層 max：找到最強的擾動

### 外層 min：更新 LoRA 參數，讓模型面對攻擊，也能正確分類

![slide 13](/assets/img/2026-02-16/slide13_img1.png)

![slide 13](/assets/img/2026-02-16/slide13_img2.png)


### 4. Proposed Framework — AdvKD-RGCL

![slide 14](/assets/img/2026-02-16/slide14_img1.png)


### 4. Proposed Framework — 整體架構

### Stage 0 (LoRA): 提供 adversarially robust 的 CLIP features

### CLIP + Adv-LoRA → Robust Features


### 4. Proposed Framework — 整體架構

### Stage 1 (Teacher): 提供 VLM knowledge 的 soft labels

### LLaVA → SBERT → MLP Teacher → Soft labels


### 4. Proposed Framework — 整體架構

### Stage 2 (Student): 結合兩者，保持 retrieval quality

### Adv-LoRA-CLIP Features →  RGCL + Label KD → Lcon + LCE + αLKD


### 5. Solution 1 — Label-Level Knowledge Distillation

### Label KD 是 Response-based KD 的一種，核心思想是：大模型（LLaVA 7B）的知識太豐富無法直接塞進小模型，但可以透過 soft label（軟標籤）間接傳遞。

### Stage 1: Teacher Pipeline ：LLaVA (7B VLM) → 結構化文字分析 → SBERT 編碼 → MLP Teacher → Soft Label

### 問題：Teacher 要幫 train set 產生 soft label，但 teacher 本身就是用 train set 訓練的。

### 解法：5-Fold Cross-Validation 避免 data leakage

### Stage 2: Student Training（RGCL + Label KD）

### KD Loss ：BCE(student_logit/T, teacher_logit/T) × T²

### Confidence Filtering：

### conf=0.2, mask = (soft_label > 0.8) | (soft_label < 0.2)

### 只蒸餾 teacher 有信心的樣本（P > 0.8 或 P < 0.2）

### 避免引入噪音

### Additive Loss：

### 0.0            0.2            0.5             0.8            1.0

### non-hateful                  uncertain                    hateful

![slide 18](/assets/img/2026-02-16/slide18_img1.png)

![slide 18](/assets/img/2026-02-16/slide18_img2.png)


### 6. Solution 2: Adversarial LoRA Adaptation

### 問題：CLIP 對 Adversarial Attack 極度脆弱

→ CLIP 是 frozen pretrained model，從未見過 adversarial examples。攻擊者只需對圖片加上人眼不可見的微小擾動，就能讓 CLIP 提取出錯誤的 features → downstream 分類完全崩壞。

### 攻擊方法：

### PGD (Projected Gradient Descent)

### RAP (Boosting the Transferability of Adversarial Attacks with Reverse Adversarial Perturbation)


### 7. Experiment Design

### 三個資料集：

### Ablation配置：

### 
| Dataset | Train | Test |

| --- | --- | --- |

| HarMeme | 3013 | 354 |

| FB | 8500 | 1000 |

| MultiOFF | 445 | 148 |

### 
| # | Config | Label KD | LoRA | 目的 |

| --- | --- | --- | --- | --- |

| 1 | + Label KD + LoRA | v | v | 我們的完整方法 |

| 2 | + Label KD | v | - | 測試 KD 貢獻 |

| 3 | + LoRA | - | v | 測試 robustness 貢獻 |

| 4 | RGCL (baseline) | - | - | 基準 |


### 8. Results

### Clean Performance (Test Retrieval @ Epoch 30)

### comparison across datasets (seed=0, T=1.0, conf=0.2, kd=0.1)

### Adv-LoRA:  r=2, a=4, dropout=0.25

### Our Methods

![slide 21](/assets/img/2026-02-16/slide21_img1.png)


### 8. Results

### Adversarial Robustness

### PGD-100 (Projected Gradient Descent)

### 每一步計算損失函數的梯度，沿著梯度方向加入微小擾動，重複 100 步。每步都把擾動投影回 L∞ 範圍內，確保擾動不超過 epsilon。

### RAP (Reverse Adversarial Perturbation)

### 在 PGD 的基礎上加入「反向擾動」機制：先用普通梯度攻擊若干步，之後每一步先用內部 PGD 找到一個讓模型「更正確」的小擾動（反向），再在這個更難的起點上繼續攻擊。

![slide 22](/assets/img/2026-02-16/slide22_img1.png)


### 8. Results

### α Ablation (T=1.0, conf=0.2)

### T Ablation (α=0.2, conf=0.2)

### → 二分類的 soft label 能提供的

### 「暗知識」非常有限。

### Conf Ablation (α=0.2, T=1.0)

### (AdvKD-RGCL)

![slide 23](/assets/img/2026-02-16/slide23_img1.png)

![slide 23](/assets/img/2026-02-16/slide23_img2.png)

![slide 23](/assets/img/2026-02-16/slide23_img3.png)


### 8. Results

### LoRA Hyperparameter Analysis

### r=4, a=8 在多數情況下達到最佳 clean-adv 平衡

### Rank 過大（r=8）反而損害泛化

### 所有配置的 adv drop 都很小 → AdvCLIP-LoRA 有效提升 robustness

![slide 24](/assets/img/2026-02-16/slide24_img1.png)


### 9. Compare Results with other papers

### 分別和 CLIP、MOMENTA、Hateclipper、RGCL 比較

### TODO:

### 和 LLM methods 比較 (Flamingo-80B1, LLaVA (Vicuna-13B))

### 和 Object Detector based models 比較 (ERNIE-Vil, UNITER, OSCAR)

![slide 25](/assets/img/2026-02-16/slide25_img1.png)


### 8. Results (TODO)

### Hyperparameter Ablation

### KD 相關

### α (kd weight)：0.05, 0.2, 0.5

### T (temperature)：1.5, 2.0, 5.0

### conf (threshold)：0.0, 0.3, 0.4

### LoRA 相關

### rank：4, 8

### LoRA α：2, 8

### PGD ε (train)：2/255

### epochs：5, 20


### 8. Results (TODO)

### Retrieval-based KNN classifier results on HatefulMemes. LR refers to logistic regression

### (I) Zero shot based on Large Multimodal Models

### Flamingo-80B

### Lens (Flan-T5 11B)

### InstructBLIP (Flan-T5 11B)

### InstructBLIP (Vicuna 13B)

### LLaVA (Vicuna 13B) [fine-tuned on HarMeme]

### (II) Train and retrieve on HarMeme

### (III) Train on HarMeme, retrieve on HatefulMemes

### (IV) Train and retrieve on HatefulMemes

### Ablation study on various VL encoders on the HatefulMemes dataset

### CLIP (Original, https://arxiv.org/abs/2211.06679)

### AltCLIP (https://arxiv.org/abs/2211.06679)

### OpenCLIP (https://zenodo.org/records/5143773)

### ALIGN (https://arxiv.org/pdf/2102.05918)


### 10. 參數配置

![slide 28](/assets/img/2026-02-16/slide28_img1.png)

![slide 28](/assets/img/2026-02-16/slide28_img2.png)


### 10. 參數配置

![slide 29](/assets/img/2026-02-16/slide29_img1.png)
