# 基礎問答方式

## 範例說明

> 每次呼叫都需要有一個簽章，依據需求定義一個簡短字串(不需要過於詳細，旨在讓模型自動找出合適的Prompt)
>
> 1. Question Answering: `"question -> answer"`
> 2. Sentiment Classification: `"sentence -> sentiment"`
> 3. Summarization: `"document -> summary"`
> 4. 自定義 …

## 內建方式 (選擇範本)

> 設定 **Signatures**
>
> ```python
> qa = dspy.ChainOfThought('question -> answer')
> response = qa(question="1+1=?")
> ```

自動生成對應 Prompt 並問答

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

## 自定方式(簡單 QA)

> 自定 Predictor
>
> ```python
> class BasicQA(dspy.Signature):
>     """Answer questions with short factoid answers."""
>
>     question = dspy.InputField()
>     answer = dspy.OutputField(desc="often between 1 and 5 words")
>
> # Define the predictor.
> generate_answer = dspy.Predict(BasicQA)
>
> # Call the predictor on a particular input.
> pred = generate_answer(question="1+1=?")
> ```

依據自訂Prompt 生成問句

<figure><img src="../../.gitbook/assets/image (2).png" alt="" width="375"><figcaption></figcaption></figure>

## 自定方式(COT + 簡單 QA)

> 使用 CoT 模組
>
> ```python
> generate_answer_with_chain_of_thought = dspy.ChainOfThought(BasicQA)
> pred = generate_answer_with_chain_of_thought(question="1+1=?")
> ```

自動進行 CoT 問答

<figure><img src="../../.gitbook/assets/image (3).png" alt="" width="375"><figcaption></figcaption></figure>
