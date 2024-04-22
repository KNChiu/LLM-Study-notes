# 檢索問答方式

## 檢索問答流程

### 設定檢索問答

與簡單問答方式比多了 `context`

```python
class GenerateAnswer(dspy.Signature):
    """Answer questions with short factoid answers."""

    context = dspy.InputField(desc="may contain relevant facts")
    question = dspy.InputField()
    answer = dspy.OutputField(desc="often between 1 and 5 words")
```

### 建立 RAG 流程

```python
class RAG(dspy.Module):
    def __init__(self, num_passages=3):
        super().__init__()

        self.retrieve = dspy.Retrieve(k=num_passages)
        self.generate_answer = dspy.ChainOfThought(GenerateAnswer)
    
    def forward(self, question):
        context = self.retrieve(question).passages
        prediction = self.generate_answer(context=context, question=question)
        return dspy.Prediction(context=context, answer=prediction.answer)
```

### 檢查回答內容 & 檢索準確度

```python
# Validation logic: check that the predicted answer is correct.
# Also check that the retrieved context does actually contain that answer.
def validate_context_and_answer(example, pred, trace=None):
    answer_EM = dspy.evaluate.answer_exact_match(example, pred)
    answer_PM = dspy.evaluate.answer_passage_match(example, pred)
    return answer_EM and answer_PM
```

### 使用 `BootstrapFewShotr`建構題詞器 (**Teleprompters**)

```python
# Set up a basic teleprompter, which will compile our RAG program.
teleprompter = BootstrapFewShot(metric=validate_context_and_answer)

# Compile!
compiled_rag = teleprompter.compile(RAG(), trainset=trainset)
```

#### 訓練資料樣式

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

### 進行資料檢索並找出關聯性最高的前N名

使用 ColBERTv2 對 Wikipedia 2017 年「摘要」（每篇文章的第一段) 進行檢索

```python
# Ask any question you like to this simple RAG program.
my_question = "What is the total population of Taiwan?"

# Get the prediction. This contains `pred.context` and `pred.answer`.
pred = compiled_rag(my_question)
```

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

### 將前N名關聯性資料提供給LLM，並搭配 CoT 進行問答

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>
