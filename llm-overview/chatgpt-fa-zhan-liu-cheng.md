---
description: 簡單敘述 LLM 歷史發展
---

# ChatGPT 發展流程

## 參考資料

*   [拆解追溯 GPT-3.5 各项能力的起源](https://www.notion.so/GPT-3-5-360081d91ec245f29029d37b54573756?pvs=21)

    分析OpenAI各模型的演進關係，理解OpenAI中各個模型API能力及ChatGPT發展
*   [State of GPT (ChatGPT 原理及现状介绍)](https://blog.csdn.net/kebijuelun/article/details/130917362)

    介紹ChatGPT 原理與訓練流程，包含如何訓練與應用([影片簡報](https://karpathy.ai/stateofgpt.pdf))



## ChatGPT(GPT Assistant) 開發流程

### 訓練階段如下&#x20;

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1).png" alt=""><figcaption><p>GPT 訓練流程</p></figcaption></figure>

* [Pretraining](chatgpt-fa-zhan-liu-cheng.md#pretraining)(佔整體約99%的時間)
* Finetuning
  * [Supervised](chatgpt-fa-zhan-liu-cheng.md#supervised)
  * [Reward Modeling](chatgpt-fa-zhan-liu-cheng.md#reward-modeling)
  * [Reinforcement Learning](chatgpt-fa-zhan-liu-cheng.md#reinforcement-learning)

### Pretraining

#### GPT-3的出現與展示如下 :

* zero-shot prompting : 不須微調與訓練就能有基本的效果。
* 語言生成 : 訓練包含 300B 的 Tokens，實現了由提示詞(prompt) → 生成補全的句子(completion)。
  * 60% : 2016 - 2019 的 C4
  * 22% : WebText2
  * 16% : Books
  * 3% : Wikipedia
* 情境學習(in-context learning) : 給予配對範例(如: 動物→幾隻腳，再問新的動物是幾隻腳)。
* 世界知識(world knowledge) : 由於訓練資料(Large Unsupervised Dataset)包含大量事實知識(factual knowledge)與常識(commonsense)，其175B的參數實現了知識的存儲，所以知識與常識密集的任務與參數量有較大關聯。

Dataset Tokens 與 Parameters 比較

| 比較           | GPT-3 | LLaMA |
| ------------ | ----- | ----- |
| Time         | 2020  | 2023  |
| Train Tokens | 300B  | 1.4T  |
| Parameters   | 178B  | 65B   |
| 生成效果         | 較差    | 較優    |

### Supervised

使用少量人工標註的數據進行訓練

prompt(人類指令約 10K) ↔ response(針對人類指令標註約 100K)

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>資料集準備</p></figcaption></figure>

### Reward Modeling

#### RM Dataset :

* 一個Prompt → 生成多個回復，並由人工進行排名

#### RM Training :

* <mark style="background-color:blue;">**藍區塊**</mark> : 相同的 Prompt Token
* <mark style="background-color:yellow;">**黃區塊**</mark> : 不同的 Completion Token
* <mark style="background-color:green;">**綠區塊**</mark> : 預測的結果
* 在<mark style="background-color:blue;">**藍區塊**</mark>後加入一個<mark style="background-color:yellow;">**黃區塊**</mark>來獲得<mark style="background-color:green;">**綠區塊**</mark>的預測結果

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Reward Modeling Training</p></figcaption></figure>



### Reinforcement Learning

#### RLHF（Reinforcement Learning from Human Feedback)

使用RLHF 從人類的反饋中進行強化學習微調，希望能符合人類期望，以限制幻覺的產生。

*   使用RLHF後所觸發/突現的能力(本來模型就有相關能力)

    * 公正的回應(<mark style="color:blue;">**Impartial responses**</mark>) : ChatGPT能夠對多個實體利益的事件（如政治事件）給予平衡的回應。
    * 拒絕不恰當的問題(<mark style="color:blue;">**Rejecting improper questions**</mark>) : 內容過濾器和RLHF誘導的結合。
    * 拒絕其知識範圍之外的問題(<mark style="color:blue;">**Rejecting questions outside its knowledge scope**</mark>)：拒絕回應2021 年6 月之後發生的新事件。使模型能夠隱式地分類哪些資訊在其知識範圍內。

    <figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>RLHF 效果比較</p></figcaption></figure>



* RLHF 搭配 SFT Model 去生成數據並由人工進行優劣的排序，可以大幅減少工作量與提供較有區別度的效果。
* 基於獎勵機制進行模型的訓練(假設有三段文字如下)
  * Great (佳) → 較高採樣概率
  * Bad (差) → 較低採樣概率
  * Ok (堪用)

<figure><img src="../.gitbook/assets/image (6) (1) (1).png" alt=""><figcaption><p>RL Training</p></figcaption></figure>
