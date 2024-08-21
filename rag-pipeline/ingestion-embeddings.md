---
description: 當將文件 Chunking 完成後將進行 Embedding
---

# Ingestion - Embeddings

## Embedding 常見架構

{% embed url="https://huggingface.co/spaces/mteb/leaderboard" %}
Embedding 效果排名
{% endembed %}

### **Sparse embedding(**稀疏嵌入)

將提示與文件進行匹配，計算強度較低，但可能無法取得文本中更深層的語義。



### **Semantic embedding (**語意嵌入**)**

#### **BERT**

適合擷取文件和查詢上下文的細微差別。與稀疏嵌入相比，需要更多的運算資源，但提供語意更豐富的嵌入。

#### **SentenceBERT**

適合句子層級的上下文和意義。在 BERT 的深入上下文理解和簡潔、有意義的句子表示的需求之間取得了平衡。

{% embed url="https://codewithzichao.github.io/2020/08/04/NLP-%E9%A2%84%E8%AE%AD%E7%BB%83%E6%A8%A1%E5%9E%8B-SentenceBERT/" %}



<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Basic RAG Pipeine</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>
