---
description: 進行提示詞自動優化時的輸入輸出與流程示意
---

# 提示詞自動優化 - 流程說明

## 事前準備

### 資料集準備

GSM8K, HotPotQA 或是自已準備的資料集，需要有`question`搭配`answer`結構

```
{
    "question" : "What river is near the Crichton Collegiate Church?"
    "answer" : "the River Tyne"
}
```



### 設計評分標準

用於評估生成的 Prompt 是否合適，`dspy.evaluate.answer_exact_match` 表示字詞需要完全相同

```python
### Set Evaluate.
def validate_context_and_answer(example, pred, trace=None):
    answer_EM = dspy.evaluate.answer_exact_match(example, pred)
    return answer_EM
```



### 設計各區塊

一個完整的問答會包含 : `簽名` + `輸出入範例` + `問題與回應`(而 Prompt 的部分為 : `簽名` + `輸出入範例`)

```
<簽名>
---
<輸出入範例>
---
<問題與回應>
```

```python
class CoTSignature(dspy.Signature):
    """<初始簽名>"""

    question = dspy.InputField(desc="<問題樣式>")
    answer = dspy.OutputField(desc="<回答樣式>")
```

#### 建立初始簽名

設計一個`初始簽名`用於提示任務功能

```
Calculate the solution to the given math question and provide the answer as a numerical response.
```

#### 輸出入範例

設計輸出入`預期的樣式`

```
question : "question about math"
answer : "is numbers only"
```

<pre class="language-python"><code class="lang-python"><strong>class CoTSignature(dspy.Signature):
</strong>    """Calculate the solution to the given math question and provide the answer as a numerical response."""

    question = dspy.InputField(desc="question about math")
    answer = dspy.OutputField(desc="is numbers only")
</code></pre>

#### 問題與回應

```
Question: <實際問題>
Reasoning: Let's think step by step <LLM 回應>
```

### 完整問答樣式

```
Calculate the solution to the given math question and provide the answer as a numerical response.

---

Follow the following format.

Question: question about math
Reasoning: Let's think step by step in order to ${produce the answer}. We ...
Answer: is numbers only

---

Question: Megan pays $16 for a shirt that costs $22 before sales. What is the amount of the discount?
Reasoning: Let's think step by step in order to produce the answer. We can find the amount of the discount by subtracting the sale price from the original price. 

Answer: $22 - $16 = $6
```



## 提示詞生成（第一次）

完成[事前準備](ti-shi-ci-zi-dong-you-hua-liu-cheng-shuo-ming.md#shi-qian-zhun-bei)後開始主流程

### 候選提問詞（生成多個）

依據[初始簽名](ti-shi-ci-zi-dong-you-hua-liu-cheng-shuo-ming.md#jian-li-chu-shi-qian-ming)生成候選提問詞，結構如下

```
<說明候選提示詞的樣式>

---

<說明預期的格式化樣式>

---

<使用初始簽名做為優化的依據>
```

#### 說明候選提示詞的樣式（英文／中文對照）：&#x20;

```
You are an instruction optimizer for large language models. I will give you a ``signature`` of fields (inputs and outputs) in English. Your task is to propose an instruction that will lead a good language model to perform the task well. Don't be afraid to be creative.
---
您是大型語言模型的指令最佳化器。我會給你一個英文欄位（輸入和輸出）的「簽名」。您的任務是提出一條指令，以引導良好的語言模型很好地執行任務。不要害怕發揮創意。
```

#### 說明預期的（提示詞／評比）格式化樣式（英文／中文對照）：

```
Follow the following format.

Basic Instruction: The initial instructions before optimization
Proposed Instruction: The improved instructions for the language model
Proposed Prefix For Output Field: The string at the end of the prompt, which will help the model start solving the task
---
請遵循以下格式

基本指令：優化前的初始指令
提議指令：語言模型的改進指令
提議輸出欄位前綴：提示末尾的字串，這將幫助模型開始解決任務
```

使用[初始簽名](ti-shi-ci-zi-dong-you-hua-liu-cheng-shuo-ming.md#jian-li-chu-shi-qian-ming)做為優化的方向依據（英文／中文對照）：

```
Basic Instruction: 
Proposed Instruction: 
---
基本指令：<初始簽名>
提議指令：<LLM 回應>
```

#### 完整問答內容

```
You are an instruction optimizer for large language models. I will give you a ``signature`` of fields (inputs and outputs) in English. Your task is to propose an instruction that will lead a good language model to perform the task well. Don't be afraid to be creative.

---

Follow the following format.

Basic Instruction: The initial instructions before optimization
Proposed Instruction: The improved instructions for the language model
Proposed Prefix For Output Field: The string at the end of the prompt, which will help the model start solving the task

---

Basic Instruction: Calculate the solution to the given math question and provide the answer as a numbers only response.
Proposed Instruction: <Calculate the solution to the math question below and provide the answer as a numerical response. Make sure to show your working steps, if any.

Proposed Prefix For Output Field: \"The answer is \">
```

#### 設定生成提示詞的數量（依據需求可以設定<mark style="background-color:red;">每次要生成多少候選提示詞</mark>）

<pre><code>"model": "gpt35_16k",
"n": 4, &#x3C;- 生成4個後選詞
"presence_penalty": 0,
<strong>"stop": null,
</strong><strong>"temperature": 1.4,
</strong>"top_p": 0.95
</code></pre>

### 初始簽名與候選提示詞

比較初始簽名與經過 LLM 生成的候選詞（英文／中文對照）：

總共有 <mark style="color:red;">`初始簽名(1)`</mark>+ <mark style="color:red;">`候選提示詞(4)`</mark> =<mark style="color:red;">`5 個`</mark>

```
Basic Instruction: Calculate the solution to the given math question and provide the answer as a numbers only response.
Proposed Instruction-1: Calculate the solution to the math question below and provide the answer as a numerical response. Make sure to show your working steps, if any.
Proposed Instruction-2: Evaluate the mathematical expression and return the numerical result.
Proposed Instruction-3: Please solve the following math question and provide the answer as a numerical value. Remember to exclude any non-numeric characters from your response.
Proposed Instruction-4: Solve the math question provided below and provide the answer as a numerical response, without any additional text.
---
基本指令：計算給定數學問題的解決方案，並以僅數字形式提供答案。
提議指令-1：計算下面數學問題的解決方案，並以數字形式提供答案。如果有請務必展示您的工作步驟。
提議指令-2：計算數學表達式並傳回數值結果。
提議指令-3：請解決以下數學問題並以數值形式提供答案。請記住從您的回覆中排除任何非數字字元。
提議指令-4：解決下面提供的數學問題，並以數字形式提供答案，無需任何其他文本。

```

### 自動評比

自動依序將以上五個提示詞進行評分判斷好壞並記錄，包含以下資訊&#x20;

* [評分](ti-shi-ci-zi-dong-you-hua-liu-cheng-shuo-ming.md#she-ji-ping-fen-biao-zhun)
* [提示詞](ti-shi-ci-zi-dong-you-hua-liu-cheng-shuo-ming.md#hou-xuan-ti-wen-ci-sheng-cheng-duo-ge)
* [輸出入格式](ti-shi-ci-zi-dong-you-hua-liu-cheng-shuo-ming.md#shu-chu-ru-fan-li)

```
{'score': <評分>,
  'program': predictor = ChainOfThought(CoTSignature(question -> answer
      instructions=<提示詞>
      question = Field(annotation=str required=True json_schema_extra={'desc': <格式化輸入樣式>, '__dspy_field_type': 'input', 'prefix': 'Question:'})
      answer = Field(annotation=str required=True json_schema_extra={'desc': <格式化輸出樣式>, '__dspy_field_type': 'output', 'prefix': 'Answer:'})
  )),
  'instruction': 'Calculate the solution to the math question below and provide the answer as a numerical response. Make sure to show your working steps, if any.',
  'prefix': 'The answer is',
  'depth': 0}
```

```
[{'score': 90.0,
  'program': predictor = ChainOfThought(CoTSignature(question -> answer
      instructions='Calculate the solution to the given math question and provide the answer as a numbers only response.'
      question = Field(annotation=str required=True json_schema_extra={'desc': 'The question about math', '__dspy_field_type': 'input', 'prefix': 'Question:'})
      answer = Field(annotation=str required=True json_schema_extra={'desc': 'Return the numbers only', '__dspy_field_type': 'output', 'prefix': 'Answer:'})
  )),
  'instruction': 'Calculate the solution to the math question below and provide the answer as a numerical response. Make sure to show your working steps, if any.',
  'prefix': 'The answer is',
  'depth': 0},
 {'score': 90.0,
  'program': predictor = ChainOfThought(CoTSignature(question -> answer
      instructions='Calculate the solution to the given math question and provide the answer as a numbers only response.'
      question = Field(annotation=str required=True json_schema_extra={'desc': 'The question about math', '__dspy_field_type': 'input', 'prefix': 'Question:'})
      answer = Field(annotation=str required=True json_schema_extra={'desc': 'Return the numbers only', '__dspy_field_type': 'output', 'prefix': 'Answer:'})
  )),
  'instruction': 'Please solve the following math question and provide the answer as a numerical value. Remember to exclude any non-numeric characters from your response.',
  'prefix': 'Answer:',
  'depth': 0},
 {'score': 90.0,
  'program': predictor = ChainOfThought(CoTSignature(question -> answer
      instructions='Calculate the solution to the given math question and provide the answer as a numbers only response.'
      question = Field(annotation=str required=True json_schema_extra={'desc': 'The question about math', '__dspy_field_type': 'input', 'prefix': 'Question:'})
      answer = Field(annotation=str required=True json_schema_extra={'desc': 'Return the numbers only', '__dspy_field_type': 'output', 'prefix': 'Answer:'})
  )),
  'instruction': 'Calculate the solution to the given math question and provide the answer as a numbers only response.',
  'prefix': 'Answer:',
  'depth': 0},
 {'score': 80.0,
  'program': predictor = ChainOfThought(CoTSignature(question -> answer
      instructions='Calculate the solution to the given math question and provide the answer as a numbers only response.'
      question = Field(annotation=str required=True json_schema_extra={'desc': 'The question about math', '__dspy_field_type': 'input', 'prefix': 'Question:'})
      answer = Field(annotation=str required=True json_schema_extra={'desc': 'Return the numbers only', '__dspy_field_type': 'output', 'prefix': 'Answer:'})
  )),
  'instruction': 'Evaluate the mathematical expression and return the numerical result.',
  'prefix': 'The solution is:',
  'depth': 0},
 {'score': 80.0,
  'program': predictor = ChainOfThought(CoTSignature(question -> answer
      instructions='Calculate the solution to the given math question and provide the answer as a numbers only response.'
      question = Field(annotation=str required=True json_schema_extra={'desc': 'The question about math', '__dspy_field_type': 'input', 'prefix': 'Question:'})
      answer = Field(annotation=str required=True json_schema_extra={'desc': 'Return the numbers only', '__dspy_field_type': 'output', 'prefix': 'Answer:'})
  )),
  'instruction': 'Solve the math question provided below and provide the answer as a numerical response, without any additional text.',
  'prefix': 'The answer is:',
  'depth': 0}]
```

以上就是一次的完整流程，可以依據需求進行更多次的疊代以找出合適的提示詞

## 提示詞生成(第N次)

### 合併過往成果提供 LLM 參考

將前幾次的提示詞與對應的評分結果結合，讓 LLM 參考並生成新的提示詞。

```
You are an instruction optimizer for large language models. I will give some task instructions I've tried, along with their corresponding validation scores. The instructions are arranged in increasing order based on their scores, where higher scores indicate better quality.

Your task is to propose a new instruction that will lead a good language model to perform the task even better. Don't be afraid to be creative.

---

Follow the following format.

Attempted Instructions: ${attempted_instructions}
Proposed Instruction: The improved instructions for the language model
Proposed Prefix For Output Field: The string at the end of the prompt, which will help the model start solving the task

---

Attempted Instructions:
[1] «Instruction #1: Please solve the given math question and provide the answer as a numerical response.»
[2] «Prefix #1: The solution is»
[3] «Resulting Score #1: 70.0»
[4] «Instruction #2: Calculate the solution to the given math question and provide the answer as a numerical response.»
[5] «Prefix #2: Answer:»
[6] «Resulting Score #2: 80.0»
[7] «Instruction #3: Solve the math problem below and provide the numerical answer.»
[8] «Prefix #3: Answer:»
[9] «Resulting Score #3: 90.0»
[10] «Instruction #4: Calculate the solution to the given math question and provide the answer as a numerical response. Make sure to show all steps and explanations in your work.»
[11] «Prefix #4: The solution is»
[12] «Resulting Score #4: 90.0»
Proposed Instruction:
```

### 生成下一階段提示詞

設定要在產生多少提示詞後<mark style="background-color:red;">進行下一次的流程</mark>

```
"model": "gpt35_16k",
"n": 4, <- 生成4個後選詞
"presence_penalty": 0,
"stop": null,
"temperature": 1.4,
"top_p": 0.95
```

