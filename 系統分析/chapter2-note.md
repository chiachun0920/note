# 資訊系統開發模式
或稱**軟體開發模式**，是資訊系統開發活動的一系列步驟及執行順序

使用系統化及邏輯化模式的優點
- 效率
- 品質
- 易於管理

# 階段模式
- 將軟體開發分為**8**個階段
    1. 作業規劃(完成怎樣的任務)
    2. 作業規格描述(要完成這樣的任務要先具有什麼工具)
    3. 程式規格描述(描述一下要完成這些工具的程式)
    4. 編碼(開工)
    5. 參數測試(丟一些參數進程式測試)
    6. 整合測試(一連串的單元測試)
    7. 上線測試(正式交由使用者作測試)
    8. 系統評估(評估整個任務的執行)
- 無論系統大或小，都必須**歷經8階段**
- 階段間並無回饋機制
- 無彈性

# 瀑布模式
- 為了改善階段模式的**無彈性**
- 將軟體開發分為一個週期**數**個階段
    - 小問題，可能三個階段(分析、設計、實施)
    - 大問題有可能十個階段(分析、設計、實施->再去細分)
- 每個階段定義要做哪些工作及交付哪些文件
- 個階段依序進行，且有回饋機制
- 每階段的產出都須經過**確認**、**驗證**或**測試**
- 前一階段必須完全成功才進行下一階段
- 每一階段的輸出都必須通過審核，一旦審核通過，輸出文件將會被凍結，避免任意修改。
- 適用情形：
    - 低風險專案
    - 需求清晰且變動性小
    - 領域內的知識容易取得
    - 技術成熟
- 缺點
    - 必須完整清楚描述需求
    - 所有需求在每個階段都要考量，系統開發須在一個周期內完成
    - 使用者參與不足，只有需求階段加入
    - 過於強調完整文件，需求變了，文件也要跟著改變
    - 程式撰寫在較後期階段才進行，故風險高
        - 風險包括
            - 資訊科技改變
            - 需求變更
            - 關鍵人才流失
            
# 漸增模式

> 該模式把需求分為**幾**個部分，將每個部份需求訂為一個開發週期，且每個周期可依序或平行開發。

- 瀑布模式之擴充
- 因應瀑布模式在軟體開發的各個階段都必須考量所有需求，因為瀑布模式只有一個周期進行
- 將需求分成幾個部分，每個部份依照瀑布模式開發
- 每個週期完成都會在系統上增加新功能、子系統
- 與瀑布模式之差異：
    - 系統被分為子系統，子系統可以依序開發
    - 系統開發由多個週期完成
    - 每個漸增完成需要跟使用者進行回饋
    - 每個週期均有使用者參與，故風險較低
- 適用情形
    - 需求清楚
    - 預算分期編列，避免一次性大量投入預算導致風險激增
    - 組織需要時間來學習新科技

# 雛型模式
- 適用需求不是那麼明確的情況
- 適用系統開發者對於該使用何種技術不是那麼熟悉的情況
- 先針對需求較清楚的部分開發，快速建立雛形，並利用此雛形與使用者重複的反饋
- 並不強調以**文件**來做為雙方溝通的工具，而是以**雛形**
```
參與人員      雙方         開發者          雙方           雙方
           
                            否
               ┌--------------------------------------------┐
               ↓                                            |  是
開發程序   需求擷取/分析 -> 雛型開發  -> 檢討雛形及需求 -> 符合約定 -> 結束

```
- 適用情況：
    - 客戶高度參與
    - 應用領域不熟悉
    - 高風險
- 缺點
    - 因缺少文件，所以難維護
    - 不適用大型專案設計
- 策略
    - 演進式雛型策略
        - 最初的雛型較為完善
        - 每一週期的開發都以上一個週期所產出的雛型來作為下次開發的基礎
    - 用後丟棄式雛型策略
        - 以快而粗糙的方式建立雛形
        - 雛型用過即丟

# 螺旋模型
- 導入風險分析
- 依照風險使用不同模式混合
- 每個周期結束要開會檢討系統績效，並決定下一周期的進行方式
- 週期
    1. 找出系統目標、可行之實施方案與限制
        - 系統目標：達到的績效，擁有的功能...
        - 實施方案：A計畫、B計畫或是重用、購買
        - 方案之限制：成本、時程...
    2. 依目標與限制評估方案、風險分析
        - 找出風險、解決風險
        - 解決風險可用雛形、*模擬*、*標竿*、*參考點檢查*、問券
    3. 發展、驗證階段
    4. 由剩下之相關風險決定下一個步驟
        - 考慮接下來要用何種模式開發
        - 可能初期使用雛型模式解決風險，後期使用瀑布模式

![](http://i.imgur.com/tb2T5fY.png)

# 同步模式
- 構想來自於製造業的同步工程
    - 同步工程目的
        - 縮短開發時間
        - 提高競爭力
- 多個團隊同時開發 -> 活動同步
    - 一個階段內的工作平行
    - 多個階段內的工作平行
    - 多個專案的工作平行
- 不同團隊資訊互相交流 -> 資訊同步
    - 向前傳遞
    - 向後傳遞
    - 建立資訊交換網路
- 整合性管理系統
    - 因應同步模式的管理比一般的開發模式複雜
    - 需要控管人力、資源及成本
- 將每一版本的工作分成數個功能組，分配給數個團隊平行開發，當一版本的功能組都完成後，便交由獨立團隊進行整合與測試
- 開發與測試團隊獨立分開
- 優點：
    - 開發時程縮短
    - 提升競爭力
- 缺點：
    - 資訊溝通過於頻繁
    - 管理複雜度高
    - 人力成本高
- 適用：
    - 商業化套裝軟體

# Rational 統一流程模式 (RUP)
- 結合螺旋模式的概念
- 以反覆與漸增的原理開發
    - 反覆：反覆用相同一系列操作或步驟進行軟體開發
    - 漸增：每次都在軟體產品上增加新功能、模組、元件或子系統
- 每次反覆後須產生一個可運作的系統版本
- 每個反覆週期要評估風險，盡早發現問題
- 不需要假設使用者一開始需求明確
- 動態面(水平軸)：
    - 依專案周期時程表表達系統開發之週期、階段、反覆與里程碑
    - 分成初始、詳述、建構與轉換四階段，並構成一個週期，週期反覆進行
    - 初始：
        - 建立系統需求與範圍
        - 建立準則
        - 評估風險(成本、時程與技術)
    - 詳述：
        - 處理主要的技術工作(設計、實施、測試)
        - 以實際程式編輯來探討主要的技術風險
    - 建構：
        - 建構可運作的版本
        - 內部版本α：系統可用且滿足使用者需求
        - 系統版本β：具備完整功能，包括安裝與文件支援與訓練教材
    - 轉換：
        - 依使用者回饋調整系統
        - 將系統移交客戶使用
- 靜態面(垂直軸)：
    - 依工作流程之邏輯順序表達系統開發之活動、領域範圍、工作產出與角色
    - 軟體工程
        - 企業塑模
            - 為了瞭解系統所要部署的**目標組織**的未來結構
            - 了解目標組織的問題與改善方案
            - 確保開發者與使用者有共同共識
            - 導出系統需求與企業模型
        - 需求
            - 建立與維護客戶對系統能做什麼的認同
            - 提供系統開發者能理解的系統需求
        - 分析與設計
            - 將需求轉換成如何實作系統之規格
        - 實作
            - 以子系統之組織階層定義程式碼
            - 個別進行單元測試，最後整合成可執行之系統
            - 實作僅限於單元測試，系統測試與整合測試是測試階段之工作
        - 測試
            - 發現與紀錄軟體的瑕疵
            - 驗證需求
        - 部署
            - 測試軟體在最終作業環境之運作
            - 包裝軟體以便交付、配送、安裝
    - 管理支援工作
        - 組態管理與變更管理
            - 追蹤與維護專案在演化的完整性
            - 維護開發之產出、資產
            - 管理版本
                - 存放位址、如何存取、為何被修改、目前之狀態與負責人員
        - 專案管理
            - 提供管理專案之架構
            - 規劃整個專案生命週期的反覆與一個特定的反覆
            - 監督專案
            - 衡量進展
        - 環境
            - 建立開發環境，包括工具，用來支援軟體開發
            - 包括選擇工具
- 面積：
    - 所對應工作之工作量或比率

# 敏捷軟體開發理念
- 個體與互動勝餘流程與工具
- 可運作的軟體賸餘全面性文件
- 與客戶的協同合作賸餘契約談判
- 因應變化勝於遵循計畫

# 動態系統開發方法
- 快速應用開發法(Rapid Application Development)之延伸
- 反覆與漸增的開發進行
- 強調使用者在開發過程中的參與
- 隨著需求改變而反覆調整
- 適用
    - 時程緊湊
    - 預算有限
- 過程
    - 專案前
        - 提出專案建議
        - 決定備案
    - 專案生命週期
        - 可行性研究：評估專案的型態、組織和成員是否適用動態系統開發
        - 企業研究：分析該企業之需求及技術能力等等
        - 反覆功能建模
            - 反覆的開發階段
            - 依照功能模型，產出**功能雛形**系統
        - 反覆設計與建置：
            - 考量功能性及非功能性需求
            - 精煉功能建模階段之雛形，釋出**設計雛形**系統供使用者測試
        - 實施：將系統建置在使用者的環境
    - 專案後
        - 衡量系統績效
        - 提出改善之處

# Scrum 軟體開發
- 強調反覆地短期發展、檢討與調整
- 主張以若干個固定時間長度的Sprint(衝刺)進行開發工作，並在每個衝刺時展示完成的工作
- 將一項任務分為多個子工作(一個衝刺時間可完成的大小，例如：1-4周)
- 優點：
    - 有效控制資源
    - 開發團隊具有高度的自主權及緊密的溝通合作
    - 持續檢視成果，開發團隊的活動變得透明、有效率且容易控制
- 階段：
    - 衝刺準備
        - Scrum團隊建立
            - 產品負責人：接觸客戶、定義需求、承擔成敗
            - Scrum專家
            - 開發團隊
        - 需求擷取
        - 需求轉換：將使用者需求轉換為**Story**，Story的集合稱作**產品清單**，並依照故事重要性設優先序
            - Story為Scrum的基本分析單位
            - 須以使用者觀點出發
            - 身為[角色]，我可以做[什麼行為]，以達到[什麼目的]
    - 衝刺進行
        - 衝刺規劃
            - 產品負責人召開衝刺會議，從產品清單挑選該衝刺的需求
            - 將一個故事分為幾個工作項目，並估算工時(小時)
            - 確認任務目標，標註優先序，分配成員
            - Scrum專家寫成衝刺資訊文件
        - 執行
            - 依照衝刺清單進行開發
            - 包含：設計、編碼、整合、測試
            - 每天有站立會議(昨天完成什麼、今天預計完成、遇到的問題)
        - 成果檢視
            - 在衝刺檢視會議上，將成果展示給產品負責人
            - 例如：展示網站的其中一個功能(可執行)
        - 評估
            - 產品負責人召開反省會議
            - 檢討與改善
            - Scrum專家撰寫衝刺報告
            - Scrum成員召開產品清單精進會議，反覆檢討修改
