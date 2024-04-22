# Chain-of-Thought (CoT)

## **Few-shot CoT Prompting**

[Chain-of-Thought Prompting Elicits Reasoning in Large Language Models](https://arxiv.org/abs/2201.11903)

* 透過加入手動編寫 (Manual-CoT) 的少樣本推理步驟提示，實現較為複雜的問題

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption><p>Manual-CoT</p></figcaption></figure>

## **Zero-shot CoT Prompting**

[Large Language Models are Zero-Shot Reasoners](https://arxiv.org/abs/2205.11916)

* 只要加上非常簡單的一句話，就能讓此類任務有一個有效的效果

```bash
Let's think step by step.
```

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption><p>Zero-shot CoT</p></figcaption></figure>

## **Self-Consistency**

[Self-Consistency Improves Chain of Thought Reasoning in Language Models](https://arxiv.org/abs/2203.11171)

透過小樣本 CoT 對多個不同的推理路徑進行取樣，並在多個迭代中來選擇最一致的答案(出現次數最多)，這有助於提高 CoT 提示在涉及算術和常識推理的任務中的表現，但對於回應的類型會有限制(選擇題/固定結果)。

## **Automatic CoT Prompting**

[Automatic Chain of Thought Prompting in Large Language Models](https://arxiv.org/abs/2210.03493)

[https://github.com/amazon-science/auto-cot](https://github.com/amazon-science/auto-cot)

### 回顧 Zero-shot, Few-shot COT

由於 Zero-shot CoT (a) 使用固定的 Prompt **“Let's think step by step.”** 與 Few-shot CoT (Manual-CoT)(b) 使用**推理步驟範例**不一定適合所有的情境(arithmetic 、commonsense reasoning…)

<figure><img src="../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption><p>Zero-shot, Few-shot COT review</p></figcaption></figure>

### 兩階段工作拆解

**為了改善此狀況 Auto-CoT 針**對多樣性問題進行採樣，由以下兩個主要階段組成：

#### **Question Clustering :**

將資料集中的問題分成幾個集群(**Cluster**)。使用PCA投影可視化後。

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption><p>Question Clustering</p></figcaption></figure>

#### **Demonstration Sampling :**

從每個群集中選擇一個代表性問題，並使用帶有簡單啟發式方法(simple heuristics)的 Zero-Shot-CoT 產生推理鏈。



Simple Heuristics (簡單啟發式方法)

使用較少的Tokens (e.g., 60 tokens) 與較少的步驟 (e.g., 5 reasoning steps)

將各個代表性問題的啟發式方法(少量 tokens 與 推理步驟)使用“Let’s think step by step”自動生成這些代表性問題的回覆，這樣在後續遇到相似類性問題時能夠得到較好的回答效果。

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption><p>Demonstration Sampling</p></figcaption></figure>
