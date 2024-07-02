---
description: 合成數據的定義與收集
---

# 合成數據

### 合成數據的定義與取得手法

#### 什麼是合成數據？

合成數據是指通過人工方法或使用生成模型（如GPT-3.5）創建的數據，而不是直接從真實世界收集的數據。這些數據旨在模擬真實世界中的數據，但可以更精確地控制其特性和質量。

#### 合成數據的優勢

1. **控制性強**: [可以根據需求生成特定類型的數據](shi-yong-da-xing-wen-zi-yu-yan-mo-xing-sheng-cheng-he-cheng-shu-ju.md#shao-yang-ben-he-cheng-shu-ju-sheng-cheng)，確保數據的多樣性和覆蓋面。
2. **無敏感信息**: [合成數據不包含個人隱私或敏感信息](yu-yan-mo-xing-he-cheng-zi-liao-de-zui-jia-shi-jian-he-jing-yan-jiao-xun.md#gai-jin-sheng-cheng-mo-xing)，適合用於公開共享和研究。
3. **量大且快速生成**: 使用生成模型可以快速生成大量數據，滿足訓練大型模型的需求。

#### 合成數據的取得手法

1. **數據生成模型**: [使用預訓練的大型語言模型（如GPT-3.5）來生成特定領域的數據(Phi-1)](../phi-xi-lie/phi-1.md#id-2.-xun-lian-xi-jie-ji-gao-pin-zhi-shu-ju-de-zhong-yao-xing)。這些模型可以根據提供的提示或範例生成高質量的文本數據。
2. **數據擴充**: [將現有的高質量數據作為基礎，通過模型進行數據擴充(Phi-1.5)](../phi-xi-lie/phi-1.5.md#jia-gou)。例如，將少量的真實教科書數據輸入生成模型，擴充生成更多類似風格和內容的數據。
3. **數據過濾與優化**: [使用自動化工具和人工標註對生成的數據進行過濾和優化(Phi-3)](../phi-xi-lie/phi-3-phi-3-vision.md#id-1.-hua-shi-dai-de-xing-neng-yu-xiao-xing-hua)，確保數據的質量和適用性。
