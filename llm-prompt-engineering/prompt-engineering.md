---
description: >-
  LLM 模型使用大量的數據集進行訓練具有龐大的參數量，所以在 FineTune 上需要耗費許多的時間與費用，因為這些缺點所以發展出提示工程 (Prompt
  Engineering)
---

# Prompt Engineering

## Prompts (提示詞)

* 生成的結果與品質取決於提示詞 (Prompts) 的設計，可以包含指令或問題等訊息，並包含其他詳細訊息，如上下文、輸入或範例。
* 因為自然語言的特性，即使Prompt只有微小的差異也可能導致相差甚遠的結果。
* 使用合適的 Prompt 能夠提升大語言模型處理複雜任務場景的能力。
* 要找到合適的 Prompt需要大量的實驗迭代與訓練資料。



## Prompt Formatting (提示格式化)

### zero-shot prompting (零樣本提示)

*   沒有提供期望的輸出示範直接進行提問，此輸出結果較無法預期。

    ```bash
    Q: What is prompt engineering?
    ```
*   更甚至可以省略 “Q:”

    ```bash
    What is prompt engineering?
    ```



### few-shot prompting (少樣本提示)

提供一些期望的輸出樣式，讓模型能提供預期的回復。

*   提問 :

    ```bash
    This is awesome! // Positive
    This is bad! // Negative
    Wow that movie was rad! // Positive
    What a horrible show! //
    ```


*   回應 :

    ```bash
    Negative
    ```



## **Prompt Elements  (提示元素)**

* **Instruction (指令)** : 希望模型執行特定任務或指令。
  *   "Write", "Classify", "Summarize", "Translate” …

      ```bash
      ### Instruction ###
      Translate the text below to Spanish:
      Text: "hello!"
      ```

      ```bash
      ¡Hola!
      ```
* **Context (語境)** : 引導模型做出更好回應的外部資訊或附加上下文。
* **Input Data (輸入資料)** : 希望尋找答案的輸入或問題。
* **Output Indicator (輸出指示)** : 指定輸出的類型或格式。

