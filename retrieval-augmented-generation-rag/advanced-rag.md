---
description: >-
  採用 pre-retrieval (預檢索) 和 post-retrieval (檢索後)
  策略，使用滑動視窗方法、細粒度分段和合併元資料來改進其索引技術，結合了多種最佳化方法來簡化檢索過程。
---

# Advanced RAG

## Pre-retrieval process (預檢索過程)

> 提高索引內容的品質：增強資料粒度、優化索引結構、添加元資料、對齊優化、混合檢索。優化查詢讓使用者的原始問題更清晰，更適合檢索任務，常見的方法包括查詢重寫查詢變換、查詢擴充等技術。

## Post-Retrieval Process (檢索後過程)

*   重新排序區塊

    > 將檢索到的訊息以關聯性重新定位到提示的邊緣(edges of prompt)。
*   上下文壓縮

    > 如果將所有檢索到相關的文件直接餵入會導致資訊過多過於混亂，而關聯性過低的則會稀釋掉資訊，所以在檢索後進行選擇、強調關鍵部分並縮短時間。



<figure><img src="../.gitbook/assets/image (5) (1).png" alt="" width="270"><figcaption><p>Advanced RAG</p></figcaption></figure>
