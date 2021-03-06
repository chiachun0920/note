# 交易
- 將一個或多個針對資料庫從事資料存取的動作，包裝成的**單一任務**
- 為資料庫處理的邏輯單位
- 例如：ATM提款，線上訂票
- 交易一定要整個完成，確保任務的正確性

# 交易管理
- 因應在商業資料庫中，經常遇到多筆交易同時對一筆記錄做存取
- 確保交易的正確性
- 機制：
    - 並行控制：讓多筆交易能在同一時間存取同一資料，而不會互相干擾
    - 失敗回復、復原：若資料庫在執行某交易的動作中發生失敗，必須要能夠重新回到一個已知的正確狀態
- 目標：
    - 確保交易可以**並行處理**
    - 確保交易的**正確性**及**可靠性**
    - 提高*異質性交易*的效率
    - 提高系統的可用率
    - 降低系統成本

# 交易管理的四大特性：ACID
- 單元性
    - 交易是一個不可在分割的完整個體，不是全部執行，就是全部不執行
        - 全部執行：指的是交易正確且正常完成，並透過(確認)Commit命令將交易結果存入永久性資料庫中
        - 全部不執行：若在交易的進行中，發生故障、毀損等行為，必須透過Rollback(退回)指令，將交易回復到執行前的時間點
    - 確保單元性是**回復**的責任
- 一致性
    - 當交易執行完全正確，會讓資料庫從一個一致性狀態轉變到另一個一致性狀態，並這筆交易具有一致性。
        - 一致狀態：指資料庫的資料，符合當初設下的各種相關限制，且具有正確結果
    - 確保一致性是DBMS工程師的責任
- 孤立性
    - 某交易執行時所用的資料或中間結果，不應該被其他交易同時讀取或寫入，應要確實等到交易結果Commit後才能進行存取
    - 交易執行時，有可能會被其他交易查覺
    - 確保交易是**並行控制**的責任
- 永久性
    - 一旦交易全部執行且Commit後，則對資料庫所做的變更**永遠有效**，即使系統當機或毀損
    - 一般是以備份、硬碟映射、系統日誌來達成
    - 永久性是回復的責任

# 交易中的動作
- BEGIN_TRANSACTION：交易執行開始
- END_TRANSCATIONL：交易執行結束，可能是Commit或Rollback
- READ或WRITE：對某資料項的讀寫動作
- COMMIT：交易全部完成，且更改的資料已被確認進入資料庫
- ROLLBACK(或ABORT)：交易未完成，需將交易對資料庫的所有改變做回復動作，退回到交易前的原點

> 在資料庫系統中如果遭預正常/不正常的關閉，DBMS會將進行的交易Commit或Rollback

# 系統日誌
- 為了能從故障中回復，提供回復所需的資訊
- 記錄了所有交易中可能會影響資料庫的操作(例如:Write)
- 儲存於永久性儲存媒體(如：硬碟)，可預防非毀滅性故障
- 運作：
    1. 將影響資料庫的操作存在主記憶體(包含READ/WRITE)
    2. 主記憶體滿或發生Commit就將會影響資料庫的資訊寫入硬碟的系統日誌(WRITE)
    3. 系統日誌滿了或到達檢查點，將所有**異動資訊**寫進資料庫(開始更新資料庫)
    ![](http://i.imgur.com/lt5cWm3.png)
- 若交易進行中發生錯誤，可以檢查系統日誌，來做回復的動作
    - Redo：回溯整個日誌，重新執行交易的某些動作
    - Undo：回復某些操作，就當作沒有發生動作
- Rollback vs Undo
    - Rollback是回復整個交易；Undo是回復交易中的某些動作

# 確認點
- 當某交易對資料庫的存取動作都已成功執行，交易的動作結果也寫入系統日誌，此交易即達到確認點
- 到達確認點之後，交易被稱為**以確認的**，交易結果會永久紀錄在資料庫，接著寫入commit(T)到系統日誌
- 以**單元性**來看，當系統發生故障，若交易已經開始，但未達確認點，則交易必須被退回，以確保交易全部不執行
- 運作：
    ![](http://i.imgur.com/ZZqCgrw.png)

# 系統日誌強迫寫入
- 正常而言，主記憶體會保留一個區塊，做為交易進行時記載資料異動操作的日誌記錄，並在交易到達確認點，將其寫入到硬碟的系統日誌
- 在交易尚未到達確認點，因為某些原因，將主記憶體的資料寫入到硬碟系統日誌，稱為**系統日誌強迫寫入**
- 這些原因包括：
    - 主記憶體區塊滿了
    - 到達**檢查點**

# 檢查點
- 由DBMS定期發動的**系統日誌強迫寫入**，並將一個檢查點寫入到系統日誌中，同時將系統日誌中，於檢查點之前所有已確認的交易所產生的異動結果，<span style="color:red">真正寫入到資料庫<span>
- 檢查點的寫入，代表此點之前所有已確認的交易，在系統發生毀損錯誤時，不需要被重新執行
- 運作：
    1. 暫停所有交易動作
    2. 將主記憶體中，所有已確認的交易寫進系統日誌
    3. 將主記憶體中尚未確認的交易強制寫進系統日誌，並寫入一個check point到系統日誌
    4. 繼續交易
    ![](http://i.imgur.com/Cn6Puew.png)

# 交易排程
- 多筆交易以交錯方式並行執行時，所構成的執行順序
- 若N筆交易構成一個排程S，則每一個交易的操作，在排程S中出現的順序，必須與該操作在本身交易的順序相同

# 序列排程(或循序排程)
- 如果排程S裡的交易依序執行，並無交錯進行的現項，稱為序列排程
- 優點：確保資料庫的正確性
- 缺點：浪費時間和系統資源且缺乏彈性

# 可序列化排程
- 若排程與相同N筆交易的排程所構成之序列排程等價，則此排程被稱為可序列化排程
- 提供交易的並行性
- 紓解序列排程的缺點
- 保證交易之正確性

# 等價種類：
- 結果等價：兩個排程最後產生相同的資料庫狀態
    > 結果等價有可能在偶然發生，所以排程的等價不採用

- 衝突等價：若兩排程中，多個交易間發生衝突的順序相同，稱兩排程為衝突等價
    ![](http://i.imgur.com/GBHKudT.png)
    - 範例：
    ```
    序列排程Sa: r1(x), r1(y), w1(x), c1, r2(y), w2(y), r2(x), w2(x), c2
    Sa的衝突有：r1(x)->w2(x) , r1(y)->w2(y) , w1(x)->r2(x) , w1(x)->w2(x)

    排程Sb: r1(x), r1(y), w1(x), r2(y), w2(y), c1, r2(x), w2(x), c2
    Sb的衝突有：r1(x)->w2(x) , r1(y)->w2(y) , w1(x)->r2(x) , w1(x)->w2(x)

    排程Sc: r1(x), r1(y), r2(x), w1(x), r2(y), w2(y), c1, w2(x), c2
    Sc的衝突有：r1(x)->w2(x), r1(y)->w2(y), r2(x)->w1(x), w1(x)->w2(x)

    Sa與Sb衝突等價與Sc不衝突等價
    ```

# 衝突可序列化排程
- 假設有一排程與相同N筆交易的某個序列排程衝突等價，稱此排程是可序列化的
- 校驗步驟：