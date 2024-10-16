# Operationalizing AI Principles: Setting Up and Running Reviews


## Google AI governance
1. **定義AI原則後的下一步**：建立審查流程以實踐這些原則。
2. **AI原則的作用**：它們是建立價值觀的起點，但不能立即解答所有AI倫理問題，需要持續努力來應用。
3. **AI治理的必要性**：負責任的AI技術工具是有用的，但只有在清晰的責任目標下才有效。強大的AI治理流程是確立目標的關鍵。
4. **誤解1**：僱用有倫理的人不會保證產生倫理的AI產品，因為不同背景的人對相同問題可能有不同結論。必須建立系統來支持倫理決策和討論。
5. **誤解2**：無法依賴負責任AI的清單檢查表，因為每個產品都有其技術細節和使用情境，需要單獨評估。檢查表可能限制思維，導致倫理盲點。
6. **審查過程的重要性**：程序應支持技術審查並激發道德想像力，幫助團隊發現問題，這比依賴僵化的規則更有效。
7. **Google的AI治理架構**：
   - **負責創新團隊**：負責指導各產品團隊實施AI原則的審查，並確保公司內的決策一致性。
   - **專家小組**：提供技術、功能和應用方面的專業知識，並在需要時參與審查。
   - **高層委員會**：處理最複雜和具有先例性的重要決策，並在公司最高層級提供問責。
   - **定制化審查委員會**：嵌入各產品領域，考慮其獨特的技術、數據和社會背景，與負責創新團隊緊密合作。
8. **多元參與的重要性**：多樣化的參與者有助於達成更可靠和可信的審查結果，並需要營造心理安全的討論環境。


## Google CLoud's review process

1. **Google Cloud 的技術與平台**：
   - 提供企業端技術如 Vertex AI、ML Ops、API 及端到端解決方案，協助企業實施 AI。
2. **Google Cloud 的 AI 審查過程**：
   - **AI 客戶交易審查**：針對早期客戶專案，確認提案是否符合 AI 原則。
   - **AI 產品開發審查**：評估 Google Cloud 所開發的產品，確保其設計與使用符合 AI 原則。
3. **審查的關鍵問題**：
   - 提案是否符合 AI 原則？
   - 如何設計產品以實現預期效益並降低潛在風險？
4. **AI 客戶交易審查的流程**：
   - 銷售提交提案，進行初步審查。
   - AI 原則團隊負責確定提案是否需要進一步審查。
   - 決策委員會討論和做出是否繼續交易的決定，如必要則升級至高層決策。
5. **AI 產品開發審查流程**：
   - **產品開發的管道跟踪**：確保產品在開發早期階段就被審查，納入「設計即倫理」的概念。
   - **審查簡報**：團隊制定簡報，包括產品目標、社會效益、使用數據等，供委員會成員提前審閱。
   - **討論與對齊**：審查會議聚焦於責任 AI 觀點，討論並做出產品設計與開發決策。
   - **批准**：制定「對齊計畫」，跟踪計畫的執行。
6. **審查流程的發展**：
   - 審查流程隨著時間發展，並依照類似情況形成政策先例，這些先例可應用於其他審查中。 


## 案例: 名人識別API

1. **結果與產品簡介**：
   - 2019年，Google Cloud推出了「名人識別API」，該API專為媒體與娛樂產業設計，用於標記知名的演員和運動員。
   - 這是Google Cloud首款帶有人臉識別功能的企業產品。
2. **審查過程**：
   - Google早在2016年就決定不將人臉識別技術加入其Cloud Vision API中，儘管這是用戶的強烈需求。
   - 通過內部AI原則審查程序，評估了人臉識別技術的潛在偏見及社會影響，並選擇推出限制範圍的名人識別功能。
3. **風險與社會責任**：
   - 技術需避免偏見並保護隱私，不應用於侵犯國際規範的監控。
   - Google與外部專家和人權組織合作，確保產品的公平性，並根據專家建議制定安全措施，例如限制API的客戶範圍、允許名人選擇退出等。
4. **公平性測試與改進**：
   - 進行多次公平性測試，分析API在不同膚色和性別群體中的表現。
   - 發現資料集對於深色皮膚人群的標籤不準確，後來使用Fitzpatrick膚色分類進行重新標籤，減少了錯誤率。
   - 通過擴展訓練集，涵蓋名人在不同年齡階段的圖像，解決了部分錯誤識別問題。
5. **持續改進與產業影響**：
   - 這一經驗強調了負責任AI開發的重要性，並促使其他科技公司退出或限制人臉識別業務。
   - Google推出了Monk膚色量表，以進一步改善影像中的代表性分析。

