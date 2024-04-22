---
description: 使用 zero/few shot 對語言模型的提示詞進行微調改進，優化語言模型的輸出結果
---

# 提示詞自動優化

## 建立 Pipeline

### OpenAI 設定

設定 API Key 與模型選擇

```python
import dspy
import os

os.environ["OPENAI_API_KEY"] = 'sk-'
turbo = dspy.OpenAI(model='gpt-3.5-turbo')

dspy.settings.configure(lm=turbo)
dspy.configure(lm=turbo)
```

### AzureOpenAI

如果要使用 Azure OpenAI 需進行以下設定

```python
import dspy
import os

turbo = dspy.AzureOpenAI(
                        api_base = "https://openai-infra-gpt4.openai.azure.com/", 
                        api_version="2024-02-15-preview",
                        model="gpt35_16k",
                        api_key="1d46cc75968e40c28dee71ec63855728",  
                        )

dspy.settings.configure(lm=turbo)
dspy.configure(lm=turbo)
```

### 資料切分

使用 DSPy 提供的<mark style="color:red;">`HotPotQA`</mark>資料切分

```python
from dspy.datasets import HotPotQA
dataset = HotPotQA(train_seed=1, train_size=10, eval_seed=2023, dev_size=20, test_size=0)
original_trainset, original_devset = dataset.train, dataset.dev
```

### 建立 CoT 模組

定義一個類似 <mark style="color:red;">`question->answer`</mark> 的基礎`signature`並傳遞給 <mark style="color:red;">`ChainOfThought`</mark> 模組，輸出基於此`signature`的 CoT 結果

```python
class CoTSignature(dspy.Signature):
    """回答問題並給出相似的推理。"""

    question = dspy.InputField(desc="請使用繁體中文對問題提出疑問")
    answer = dspy.OutputField(desc="回答為 1 到 5 個繁體中文單詞")

class CoTPipeline(dspy.Module):
    def __init__(self):
        super().__init__()

        self.signature = CoTSignature
        self.predictor = dspy.ChainOfThought(self.signature)

    def forward(self, question):
        result = self.predictor(question=question)
        return dspy.Prediction(
            answer=result.answer,
            reasoning=result.rationale,
        )
```

### 測試與評估

使用 DSPy 提供的 <mark style="color:red;">`Evaluate`</mark> 類並建立<mark style="color:red;">`validate_context_and_answer`</mark>評分，使用 <mark style="color:red;">`dspy.evaluate.answer_exact_match`</mark> 檢視 pred 和 example 是否相同

```python
from dspy.evaluate import Evaluate

def validate_context_and_answer(example, pred, trace=None):
    answer_EM = dspy.evaluate.answer_exact_match(example, pred)
    return answer_EM
    
NUM_THREADS = 5
evaluate = Evaluate(devset=trans_devest, metric=validate_context_and_answer, num_threads=NUM_THREADS, display_progress=True, display_table=False)

```

