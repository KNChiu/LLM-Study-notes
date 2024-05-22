---
description: Llama 3 的 Prompt template 介紹與使用範例
---

# Prompt formats (Llama 3)



## 參考資料

{% embed url="https://llama.meta.com/docs/model-cards-and-prompt-formats/meta-llama-3/" %}

## Special Tokens

Prompt 可以包含單一系統訊息，或多個交替使用者和助理 (assistant)，但都需以<|start\_header\_id|>assistant<|end\_header\_id|> 結尾。

### <|begin\_of\_text|>

與 BOS token 相當表示序列的開頭

> BOS(Begining of sequence) : 代表開頭。
>
> EOS(End of sequence) : 代表結尾。

### <|eot\_id|>

&#x20;表示該次序列的結束

### <|start\_header\_id|>{role}<|end\_header\_id|>

用來標記所包含特定的角色

> system, user, assistant.

### <|end\_of\_text|>

與 EOS token 想當表示序列的結尾，將停止產生更多 tokens.



## 範例 - 單次問答

### Prompt 格式

```
<|begin_of_text|><|start_header_id|>system<|end_header_id|>

{{system message}}<|eot_id|><|start_header_id|>user<|end_header_id|>

{{user message}}<|eot_id|><|start_header_id|>assistant<|end_header_id|>
```

### Prompt 說明

```
<|begin_of_text|>：表示 Prompt 開始

====================================================================

<|start_header_id|>system<|end_header_id|>：表示下列訊息為 "system"

{system message} : 放置系統訊息

<|eot_id|>：表示輸入訊息的結尾

====================================================================

<|start_header_id|>user<|end_header_id|>：表示下列訊息為 "user"

{{user message}} : 放置使用者訊息

<|eot_id|>：表示輸入訊息的結尾

====================================================================

<|start_header_id|>assistant<|end_header_id|>：使用 "assistant" 表示訊息開始產生

Llama 3 接續產生 {{assistant_message}} 
並透過產生 <|eot_id|> 來表示 {{assistant_message}} 結束
```



## 範例 - 多次問答

### Prompt 說明

<mark style="background-color:green;">**system**</mark>

<mark style="background-color:blue;">**第一次問題**</mark><mark style="background-color:purple;">**第一次回答**</mark>

<mark style="background-color:yellow;">**第二次問題**</mark><mark style="background-color:orange;">**第二次回答**</mark>

<mark style="background-color:red;">**第三次問題**</mark>

<mark style="background-color:green;"><|begin\_of\_text|><|start\_header\_id|>system<|end\_header\_id|>You are a helpful AI assistant for travel tips and recommendations<|eot\_id|></mark><mark style="background-color:blue;"><|start\_header\_id|>user<|end\_header\_id|>What is France's capital?<|eot\_id|></mark><mark style="background-color:purple;"><|start\_header\_id|>assistant<|end\_header\_id|>Bonjour! The capital of France is Paris!<|eot\_id|></mark><mark style="background-color:yellow;"><|start\_header\_id|>user<|end\_header\_id|>What can I do there?<|eot\_id|><|start\_header\_id|>assistant<|end\_header\_id|></mark><mark style="background-color:orange;">Paris, the City of Light, offers a romantic getaway with must-see attractions like the Eiffel Tower and Louvre Museum, romantic experiences like river cruises and charming neighborhoods, and delicious food and drink options, with helpful tips for making the most of your trip.<|eot\_id|></mark><mark style="background-color:red;"><|start\_header\_id|>user<|end\_header\_id|>Give me a detailed list of the attractions I should visit, and time it takes in each one, to plan my trip accordingly.<|eot\_id|><|start\_header\_id|>assistant<|end\_header\_id|></mark>
