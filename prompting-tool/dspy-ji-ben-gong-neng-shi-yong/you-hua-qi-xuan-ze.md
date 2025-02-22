---
description: 依據不同使用場景選擇不同優化器
---

# 優化器選擇

## 參考資料

{% embed url="https://dspy-docs.vercel.app/docs/building-blocks/optimizers#what-dspy-optimizers-are-currently-available" %}

## DSPy 優化器原理

傳統神經網路透過梯度下降方法進行優化，而 DSPy 則是使用`DSPy模組`多次呼叫 LLM ，而模組中包含以下三個參數進行優化，可以選擇多種訓練方式。

* LM weights
* Instructions
* Input / Output behavior

## 優化器類型

### **Automatic Few-Shot Learning (**&#x81EA;動少樣本學習)

#### LabeledFewShot&#x20;

> 使用 few-shot 範例中提供的標籤( Q/A )

#### BootstrapFewShot&#x20;

> 每次的迭代會自行產生完整的提示。僅使用這些生成的提示（如果通過評比）而不進行任何進一步的優化

#### **BootstrapFewShotWithRandomSearch**

> 多次調用 `BootstrapFewShot`，透過生成的提示進行隨機搜索，並選擇最佳方案

#### BootstrapFewShotWithOptuna

> 透過 Optuna 超參數優化方法，對提示集進行 `BootstrapFewShot`，進行實驗以最大化評估指標。



### **Automatic Instruction Optimization (**&#x81EA;動指令優化)

#### COPRO&#x20;

在每個跌代中生成並完善新指示，並透過協調上升 (Coordinate Ascent) 對其進行最佳化



