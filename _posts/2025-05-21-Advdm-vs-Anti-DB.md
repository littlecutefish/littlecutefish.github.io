---
title: "📖 Advdm vs Anti-DB"
date: 2025-05-21 00:00:00 +0800
categories: [master]
tags: [paper, tech, adversarial attack]
---

### 目標差異
防禦重點不同：

- GLAZE 和 AdvDM：主要專注於防止藝術風格模仿（artistic mimicry），目的是保護藝術家的創作風格不被 AI 模型複製。

- Anti-DreamBooth：專注於防止生成假冒或有害的個人化圖像，更關注保護個人隱私和防止身份濫用的負面影響。


### 技術方法差異

實現機制不同：

- GLAZE：在噪聲優化過程中使用複雜的風格轉換引導（style-transferred guidance），這種方法難以適應一般用戶保護場景 。

- AdvDM：僅專注於較簡單的 Textual Inversion 技術，其中生成器是固定的，不需要進行模型微調

- Anti-DreamBooth：直接應對更具挑戰性的 DreamBooth 微調過程，其中模型本身會被重新訓練，這使得防禦難度更高。


### 技術挑戰層面

面對的技術複雜度：
Anti-DreamBooth 需要處理 DreamBooth 的完整微調過程，這比 Textual Inversion 更複雜，因為後者只是優化文本嵌入而不改變模型參數。

這使得 Anti-DreamBooth 能夠防禦更強大、更靈活的個人化圖像生成技術，提供更全面的用戶保護。
