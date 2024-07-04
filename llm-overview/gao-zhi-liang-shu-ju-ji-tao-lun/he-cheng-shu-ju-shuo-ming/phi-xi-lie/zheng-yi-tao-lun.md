# 爭議討論

* 懷疑 phi-1.5 直接在測試資料集上訓練，其能夠對 GSM8K 資料集中的原始問題給出完全正確的回答，但只要稍微修改一下格式（例如換行），phi-1.5 就不會回答了。

<figure><img src="../../../../.gitbook/assets/image (25).png" alt="" width="563"><figcaption></figcaption></figure>

* 在一個點餐問題中修改問題中的數據卻會回答原本的答案，當修改了「披薩的價格」，phi-1.5 回答了修改前的正確答案。

<figure><img src="../../../../.gitbook/assets/image (26).png" alt="" width="563"><figcaption></figcaption></figure>

* 輪文作者回應 phi-1.5 的答案對 prompt 的格式是非常「脆弱」，由於沒有進行任何指令微調和對齊工作，phi-1.5 在穩健性上的確不如 GPT-4

<figure><img src="../../../../.gitbook/assets/image (27).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/image (29).png" alt="" width="563"><figcaption></figcaption></figure>
