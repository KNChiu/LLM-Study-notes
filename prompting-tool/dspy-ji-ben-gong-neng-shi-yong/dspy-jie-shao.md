# DSPy 介紹

## 關於 DSPy

DSPy 是一種針對 <mark style="color:red;">`Prompt (提示)`</mark>最佳化的框架，不是重新訓練 LLM 模型而是找出一個合適的 Prompt，提供了完整的 Pipline 與提示模板；依據以往經驗 Prompt 的微小差異將大大影響 LLM 模型最終的輸出， 所以 DSPy 希望減少人為干預 Prompt 的選用而是自動找出最適當的 Prompt。



## 為什麼使用 DSPy

以往在替換 LLM 模型或是資料集時，都使用相同的提示下將可能導致輸出效果下降，為了避免每次都需要人工去找出合適的提示，使用 DSPy 只需重新編譯(自動化找出最佳Prompt)，就可以優化整個 Pipeline。



## 運作流程

為了減少人為干預 Prompt，DSPy 提出了以下三個新概念

#### Signatures (簽名)

> 將人工建立的提示和微調抽象化，使用 <mark style="color:red;">`Signatures (簽名)`</mark> 取代，在每次呼叫時都必須有一個自然語言簽名，取代傳統的手寫提示。簽名是一個簡短的函數，它規定了任務的樣式("輸入 -> 輸出")，而不是如何提示 LLM 如何回答（例如，"參考問題和上下文後返回答案"）。

<figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption><p>由左邊的Hand-written 替換成右邊的簡短簽名</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption><p>簽名的樣式("輸入 -> 輸出")</p></figcaption></figure>

範例 :

```
"question -> answer"

"long-document -> summary"

"context, question -> answer"
```



#### Modules

> 一些有效的 Prompting Techniques 如 Chain of Thought(CoT) 、 RAG 等...被抽象化為模組以方便調用

* CoT : 依據簽名自動/自定義生成

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption><p>依據簽名自動生成包含 CoT 的 Prompt</p></figcaption></figure>



* RAG : 在 CoT 中加入 Retrieve

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption><p>自定義 RAG</p></figcaption></figure>

#### Teleprompters (提詞器)

> 透過 DSPy 編譯器實現自動最佳化，詳細見[提示詞自動優化章節](ti-shi-ci-zi-dong-you-hua-fan-li-cheng-shi.md)，自動將 [Signatures ](dspy-jie-shao.md#signatures-qian-ming)與 [Modules ](dspy-jie-shao.md#modules)進行結合與自動優化。

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption><p>以CoT為例，使用最佳的提示範例</p></figcaption></figure>

#### 自動優化流程

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption><p>流程說明</p></figcaption></figure>
