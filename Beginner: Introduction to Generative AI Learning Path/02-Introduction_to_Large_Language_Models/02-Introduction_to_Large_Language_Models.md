# 02 | Introduction to Large Language Models

## [Introduction to Large Language Models](https://www.youtube.com/watch?v=RBzXsQHjptQ&t=74s)

- LLM
    - def: language models refer to large general purpose language models that can be pre-trained and then fine-tuned for specific purposes what do pre-trained and fine-tuned mean great questions
    - large
        - the enormous size of the training data
        - the parameter count in ML -> hyperparameters
            - basically the memories and the knowledge that machine learned
    - general purpose
        - models are sufficient to solve common problems
            - 人類語言共通性
            - 有限資源
    - last terms pre-trained and fine-tuned 
        - last terms pre-trained -> 大量資料
        - fine-tuned -> 少資料
    - 其他特色
        - 可以處理 zero-shot 與 few-shot
            - few shot: 在機器學習中，Few-Shot Learning (FSL) 是一種特殊的學習方式，目的是讓模型在只接觸少量訓練樣本的情況下，仍能學習並完成新任務。
            - zero shot: 模型可以識別在訓練中沒有明確教導過的事物。
- prompt design
    - design is the process of creating a prompt that is tailored to the specific task that the system is being asked to perform
    - general, essential
- prompt engineering 
    - the process of creating a prompt that is designed to improve performance
    - for accuracy, professional, specialized area, non-essential

### Tuning 
- 主要指對使用新資料集對模型進行調整和優化的過程，以提高其在特定任務或數據集上的性能。
- 方法
    - 超參數調整（hyperparameter tuning）
    - 模型微調（fine-tuning）
    - parameter-efficient tuning methods (PETM)
        - 通常只調整模型中的一小部分參數，減少資源消耗
        - e.g. prompt tuning


###  generic (raw) language model
目的是預測句子中的下一個詞，生成連貫的文本，或填充缺失的詞語。

![截圖 2024-10-16 下午2.52.30](https://hackmd.io/_uploads/Hy6rQ16kJx.png)

### instruction tuned models
經過微調以遵循特定指令或任務上表現更好的語言模型。例如，在通用數據上進行預訓練後，指令調優模型可能會進一步調整，使模型學會如何恰當回應像是「總結這段文本」、「將這句話翻譯成法語」或「將這條評論分類為正面或負面」的提示。


### dialog tuned
經過專門訓練來進行對話的模型，可以更有效地理解和生成與人類對話互動的回應。 Dialog-tuned 模型通常會被暴露於大量的對話資料，學習如何回應問題、提供相關資訊、維持對話的流暢性以及理解上下文間的轉換。這使它們能夠應對更複雜的對話場景，例如長期的多輪對話、追蹤不同話題或參與具體情境的討論。

### Chain of Thought reasoning
一種技術，在面對複雜的問題時，能分解成多個簡單的推理步驟，這些步驟形成一條邏輯推理的鏈條。例如，當語言模型面對數學或邏輯問題時，它不僅給出結論，還會逐步列出每個計算步驟或推理過程，這樣可以幫助模型避免因為忽略細節而導致錯誤的結果。




### Google 
- vertex AI Studio 
- gemini -> 多模態












## [Introduction to Large Language Models](https://developers.google.com/machine-learning/resources/intro-llms?hl=zh-tw)

### 語言模型
- **定義**: 語言模型是一種機器學習模型，旨在預測並生成合理的語言，例如，自動完成建議就是一種語言模型。
- **運作方式**: 
  - 語言模型估算符記或符記序列在較長符記序列中的機率。  
  - 例如句子：
    ```
    When I hear rain on my roof, I _______ in my kitchen.
    ```
  - 如果假設符號是字詞，則語言模型會判斷不同字詞或字詞序列取代底線的可能性，如下：
    - cook soup: 9.4%
    - warm up a kettle: 5.2%
    - cower: 3.6%
    - nap: 2.5%
    - relax: 2.2%
- **符記序列**: 可以是整個句子或一系列句子，語言模型可以計算不同整句或文字區塊的可能性。

### 大型語言模型

- **定義**: 大規模建立人類語言模型是一項極為複雜且耗用大量資源的任務，語言模型和大型語言模型的功能是經過數十年的努力才達到目前的程度。
- **模型複雜度**: 隨著模型建構的規模更大，模型的複雜度和效能也會提升。早期的語言模型可以預測單字的機率，而現代大型語言模型則能預測句子、段落甚至整份文件的機率。
- **技術進步**: 
  - 電腦記憶體、資料集大小和運算能力的提升，還有更有效的長文字序列建模技術的開發，促使語言模型的大小和功能在過去幾年大幅成長。
- **大小範圍**: 「大型」一詞已用於描述 BERT (1.1 億個參數) 和 PaLM 2 (最多 3400 億個參數)，參數是模型在訓練期間學到的權重，用於預測序列中的下一個符記。

### Transformers

- **重要性**: 語言模型開發的一大重點是 2017 年的 Transformer 架構。該架構能專注於輸入內容中最重要的部分，處理較長的序列，解決先前模型的記憶體問題。
- **組成**: 完整的 Transformer 由編碼器和解碼器組成，編碼器將輸入文字轉換為中繼表示法，然後再將中繼表示法轉化為實用文字。

### 自我注意 Self-attention

- **定義**: 自我注意力的「自我」部分是指詞彙庫中每個符號的「自我中心」焦點，實際上，這個機制會詢問每個輸入符記對其他輸入符記的重要性。
- **示例**: 
  - 在句子「動物太累，所以沒有跨越街道。」中，每個字詞都會注意其他字詞，思考每個字詞對自己的重要性。
  
### 大型語言模型的用途

- **應用範圍**: 
  - LLM (大型語言模型) 可以根據輸入內容產生最合理的文字，並顯示在摘要、問題解答及文字分類等工作上的優異表現。
  - LLM 還能解決某些數學問題並編寫程式碼，雖然建議檢查它們的作業。
- **模仿能力**: LLM 擅長模仿人類的說話模式，能將不同風格和色調的資訊結合。


## [Language Models are Few-Shot Learners](https://proceedings.neurips.cc/paper/2020/file/1457c0d6bfcb4967418bfb8ac142f64a-Paper.pdf)
// 論文

## [LangChain on Vertex AI](https://cloud.google.com/vertex-ai/generative-ai/docs/reasoning-engine/overview)

### 系統流程圖
此過程始於使用者向代理提交查詢。

![image](https://hackmd.io/_uploads/SJ_gO02kJg.png)


---

## [Transformer: A Novel Neural Network Architecture for Language Understanding](https://research.google/blog/transformer-a-novel-neural-network-architecture-for-language-understanding/)

### 背景
- **神經網絡**，尤其是**遞迴神經網絡**（RNNs），在語言理解任務（如語言建模、機器翻譯和問答系統）中扮演核心角色。
- **Transformer**是一種基於自注意力機制的新型神經網絡架構，特別適合於語言理解。

### Transformer的優勢
- Transformer在學術英語到德語及英語到法語的翻譯基準測試中表現優於RNN和CNN -> BLEU 高
    > BLEU分數（越高越好）用於評估模型在翻譯任務上的表現。
- 相較於傳統模型，Transformer不僅提供更高的翻譯質量，還需要更少的計算資源，並且更適合現代機器學習硬件，訓練速度提高了十倍。

![image](https://hackmd.io/_uploads/rkFiq0n1Jl.png)


### Transformer架構
- Transformer以自注意力機制為基礎，每次只進行固定的少量步驟，能夠直接建模句子中所有單詞之間的關係。
    - 例如，Transformer 第五層到第六層的編碼器自注意力分佈可以看出，可視化編碼器在計算“它”這個單詞的最終表示時所關注的單詞，
![image](https://hackmd.io/_uploads/Sk2N3C2kJl.png)
- 在計算每個單詞的表示時，Transformer會比較當前單詞（例如“bank”）與句中所有其他單詞，並根據注意力分數加權計算其表示。
- 在翻譯過程中，編碼器生成輸入句子的表示，而解碼器則基於這些表示逐字生成輸出句子。



### RNN vs Transformer
Transformer 對 RNN 與 CNN 帶來威脅

| **特性**                     | **Transformer**                                     | **RNN (Recurrent Neural Network)**          |
|-----------------------------|----------------------------------------------------|-------------------------------------------|
| **架構**                     | 基於自注意力機制 (Self-Attention)                 | 基於序列處理，逐字讀取                      |
| **處理方式**                | 同時考慮所有單詞，能夠捕捉長距離依賴              | 逐步處理，每次只讀取一個單詞                |
| **學習效率**                | 由於平行處理，大幅提高訓練速度                     | 隨著距離增大，學習決策所需步驟數增加       |
| **計算效率**                | 能充分利用 TPUs 和 GPUs 的並行處理能力            | 序列性結構限制了計算效率                    |
| **創造性**                  | 高溫度設定下可生成多樣化的輸出                      | 通常較為保守，較難生成創意性內容             |
| **應用範疇**                | 適用於機器翻譯、文本生成、問答系統等多種任務       | 通常用於時間序列預測、文本生成等             |
| **參數可視化**              | 自注意力機制使得可以清晰可視化注意力分佈          | 难以追踪和可視化單詞間的依賴關係             |
| **核心挑戰**                | 對於長文本依賴的內存需求高                           | 學習遠距離依賴的難度高                       |


![image](https://hackmd.io/_uploads/BynNoChk1l.png)

### 注意力機制的可視化
- Transformer的注意力分布可以視覺化，幫助了解模型如何處理或翻譯特定單詞。
- 例如，在指代解析的挑戰中，Transformer能夠正確識別“it”所指代的名詞，反映其在不同上下文中的選擇。

## [Attention is All You Need](https://research.google/pubs/attention-is-all-you-need/)

此論文提出一個 transformer，這是第一個完全基於注意力的序列轉換模型，取代了在編碼-解碼架構中常用的循環層，使用multi-headed self-attention機制。並說明他在翻譯上的優勢。

![截圖 2024-10-16 下午2.31.25](https://hackmd.io/_uploads/HJhTpAnkyg.png)

---


## [What is Temperature in NLP?](https://lukesalamone.github.io/posts/what-is-temperature/)


### 溫度（Temperature）定義
- 溫度是一個用於自然語言處理模型的參數，用來調整模型對其最可能回應的“信心”程度。

### 溫度的影響
- **高溫度**（θ）：
  - 使模型的輸出更“具創意”，有助於生成隨意或多樣化的文本（例如散文）。
  - 會“激發”原本機率較低的輸出。
- **低溫度**（θ）：
  - 使模型的輸出更“自信”，適合用於需要準確性的應用（如問答系統）。
  - 會降低較小輸出的影響，相對於最大輸出，讓模型更專注於最有可能的選擇。

### 數學背景
- **原始輸出示例**：
  - 當模型預測句子“The mouse ate the _____”的最後一個單詞時，可能的輸出及其對應的logit值如下：
  
| token    | logit |
|----------|-------|
| cat      | 3     |
| cheese   | 70    |
| pizza    | 40    |
| cookie   | 65    |
| fondue   | 55    |
| banana   | 10    |
| baguette | 15    |
| cake     | 12    |

- **正規化過程**：
  - 這些logit值不會加總到100，因此通常使用softmax進行正規化：
  $$
  \sigma(z_i) = \frac{e^{z_i}}{\sum_{j=0}^{N} e^{z_j}}
  $$
- **調整溫度的softmax**：
  - 當引入溫度變數θ時，修改softmax公式為：
  $$
  \sigma(z_i) = \frac{e^{z_i/\theta}}{\sum_{j=0}^{N} e^{z_j/\theta}}
  $$

![截圖 2024-10-16 下午2.10.16](https://hackmd.io/_uploads/S1IRdR2JJl.png) ![截圖 2024-10-16 下午2.10.23](https://hackmd.io/_uploads/Bk2AdC2ykl.png)
