# 資料庫系統的開發流程
- 需求收集與分析 
- 概念設計
    - 常用工具：ER Model
- 選用適合的資料庫系統
- 邏輯設計
    - 概念設計產物轉換成真實資料庫系統的資料模式(關聯表格)
    - 執行正規化
    - 定義完整性限制
- 實體設計
    - 設計資料庫的內部儲存結構、檔案組織、索引...
    - 評估時間與空間的效率
- 系統實作

# ER Model
- 以圖形化的方式表示儲存於資料庫內的各資料項目之間的關係
- 產出的結果又可稱為ER Diagram
- 可做為分析師與使用者溝通之工具
- 包括
    - 個體Entity -> 以方塊圖形表示
    - 屬性Attribute -> 橢圓形表示
    - 關係Relationship -> 菱形表示

# 個體
- ER Model中最基本的物件
- 真實世界獨立存在的事物
- 例如：學生、課程
- 個體中每筆紀錄都稱為一個**實例**
- Instance(Entity)與Entity(Entity Type)成對出現

# 屬性
- 用來描述個體的性質，而這些性質稱為屬性
- 有五種不同的形態
    - 一般屬性：又稱單元屬性、簡單屬性，具不可分割之特性![單元屬性](http://i.imgur.com/8RETy6h.png "單元屬性示意圖")
    - 複合屬性
        - 屬性中的值是可以分成更小的部分
        - 例如：姓名可以分割為姓跟名兩個一般屬性![複合屬性](http://i.imgur.com/FBDs07N.png "複合屬性示意圖")
    - 多值屬性
        - 該類屬性中，可包含一個以上的值
        - 以電話的屬性來說，一個人可能會有好幾個電話號碼![多值屬姓](http://i.imgur.com/oiMwwDb.png "多值屬性示意圖")
    - 衍生屬性
        - 這個屬性的值可以完全由其他屬性推導出來
        - 可以不用實際存在於資料庫中
        - 例如：學生的年齡屬性，可以由生日計算出來![衍生屬性](http://i.imgur.com/TmxpTqU.png "衍生屬性示意圖")

# 鍵值或鍵值屬性
- 鍵值是用來識別個體中每個不同實例的屬性
- 唯一性、識別性
- 主鍵或候選鍵擔任
- ![鍵值](http://i.imgur.com/qmpZnzt.png "鍵值示意圖")

# 關係
- 不同個體間的一種結合或關係集合
- 他表達了不同個體之間所隱含的關聯性
- 關係階度
    - 關係中參與連接的個體數目
    - ![關係階度](http://i.imgur.com/4BoKFsh.png "關係階度示意圖")

- 基數率
    - 不同個體之間在參與某一個關係時，個體間的實例相互參與隻數量比
    - 一對一關係
        - (1個)經理 -> 管理 -> (1個)部門 => 1:1關係
        - (1個)DVD -> 擁有 -> (1個)DVDID => 1:1關係
    - 一對多關係
        - (N個)員工 -> 工作在 -> (1個)部門 => N:1關係
        - (1個)DVD -> 擁有 -> (多個)影片檔 => 1:N關係
    - 多對多關係
        - (N個)員工 -> 進行 -> (M個)專案 => M:N關係
        - (N個)DVD -> 擁有 -> (M個)影片類型 => M:N關係

- 參與限制
    - 指某個體中的所有實例，是否一定需要依靠參與某關係，並和另一個個體產生關聯 
    - 全部參與限制：個體中的所有實例，都需要參與此關係
        - 員工 -> 參與<工作在關係> -> 部門
        - 每位員工都需要有部門
    - 部分參與限制：個體中的所有實例，不一定都需要參與此關係
        - 員工 -> 參與<管理關係> -> 部門
        - 部分員工管理部門
- 關係型態的屬性
    - ![關係型態的屬性](http://i.imgur.com/UPyxLEG.png)
- 弱個體
    - 個體依存在條件分成兩類
        - 一般個體
        - 弱個體![兩種個體](http://i.imgur.com/Gw8anWz.png)
            - 必須倚賴其他個體才能存在，不然就沒有意義
            - ![](http://i.imgur.com/UF6LPL6.png)
            - 辨認關係：弱個體與依附的個體之間的關係
                - 弱個體一定**完全參與**識別關係
                - 部分鍵：唯一可識別弱個體的屬性
                    -![部分鍵](http://i.imgur.com/GXx4IF9.png)

# 設計ER步驟
1. 根據客戶需求定義個體與關係
    - 將需求製作成文字形式
    - 轉換原則：
        - 名詞 -> 實例
        - 名詞的集合 -> 個體
        - 動詞 -> 關係
        - 形容詞 -> 個體屬性
        - 副詞 -> 關係之屬性
        - ![對應](http://i.imgur.com/AEb77Ix.png)
2. 定義這些關係的基數
3. 定義這些關係和個體之屬性
4. 定義鍵值屬性

# EER Model
- 為了彌補ER Model無法描述繼承關係...等
- 為ER Model的強化版
- 組成
    - ER Model
    - 類別概念：超類別與子類別
    - 一般化與特殊化

# 類別
- 為一組個體的集合
- 該集合中的每個個體可能具有不同的類型
- Employee可在分成Secretary, Engineer, Manager, Technician
- 超類別
    - 依上述，Employee為Secretary的超類別
    - 依上述，Secretary為Employee的子類別
- 繼承
    - 子類別可以繼承其超類別所擁有的所有屬性，並具有子類別特有的屬性
    - 超類別/子類別的關係又稱為Is-a關係

# 一般化和特殊化
- 特殊化是用來強調各個個體間的**差異性**
    - 定義某個體中子類別的程序
    - 此個體為該特殊化後的超類別
    - 將車子特殊化為卡車與汽車![](http://i.imgur.com/WBvkzYt.png)
    - 分離限制
        - 所有子類別中個個體實例不可重複
        - 一個實例只能屬於一個子類別
        - 以符號d表示![](http://i.imgur.com/WBvkzYt.png)
    - 重疊限制
        - 個體實例可能重複屬於多個特殊化的子類別
        - 以符號O表示![](http://i.imgur.com/JSP3ByD.png)
    - 完整性限制
        - 全部特殊化：
            - 特殊化時，每個實例都會出現在子類別中
            - 以雙線段表示
            - ![](http://i.imgur.com/TcGi0MQ.png)
        - 部分特殊化
            - 特殊化時，每個實例不一定都會出現在子類別中
            - 以單線段表示
            - ![](http://i.imgur.com/Xf6oi69.png)
- 一般化是用來強調各個個體間的**共同性**
    - 將數個不同個體抽離出相同的屬性，再一般化為超類別
    - 多個子類別可一般化為一個超類別
    


