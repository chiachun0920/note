# SQL
- 是一種用來與**關聯式資料庫系統對話**而使用的語言
- 由IBM研發
- 種類：
    - DDL (資料定義語言)
        - 用來宣告(建立)資料庫物件
        - 針對Table、View或Database，做Create(建立)、Drop(刪除)或Alter(更改)等動作
    - DML (資料處理語言)
        - 用來操作資料庫中的資料
        - 針對Table內的data，做插入(Insert)、更新(Update)或刪除(Delete)等動作。
    - DCL (資料控制語言)
        - 用來做資料庫的權限管控
        - Grant、Revoke、Alter Password
    - DQL (資料查詢語言)
        - 用來查詢資料庫中的資料
    - 資料管理指令
        - 用來從事資料庫的稽核與分析
    - 交易控制指令
        - 用來管理資料庫的交易動作

# SQL資料型態
- 字串
    - CHAR(N)：固定長度字元串，N為字元個數
    - VARCHAR(N)：變動長度字元串
    - BIT(N)：固定長度位元串，N為位元個數
    - BITVARING(N)：變動長度位元串
- 數值
    - INT,INTEGER：整數
    - DEC(m,n),DECIMAL(m,n),NUMERIC(m,n)：格式化數值
        - m為總位數或精確度
        - n為小數位數
    - SMALLINT：短整數
    - FLOAT：浮點數
    - REAL：單精度實數(32bits)
    - DOUBLE PRECISION：雙精度實數(64bits)
- 日期/時間
    - DATE：格式為YYYY-MM-DD
    - TIME：格式為HH：MM：SS
    - TIMESTAMP：
        - 時間戳記
        - 由DATE+TIME+六位以上小數秒數
        - 用以記錄交易進入系統的時間順序
    - INTERVAL：時間區間

# DDL
- 主要有CREATE,DROP,ALTER三個指令
- 針對資料庫(Database)、表格(Table)、View做操作

# 建立、刪除資料庫
- CREATE DATABASE：建立一個新的DB
```sql
CREATE DATABASE MediaDatabase;
```
- DROP DATABASE：刪除一個資料庫
```sql
DROP DATABASE MediaDatabase;
```
> 只令不區分大小寫

# 建立、刪除或修改表格(Table)
- CREATE TABLE：建立一個新的關連表格
    - 格式：
        ```sql
        CREATE TABLE <table_name>(
            <欄位名1><data_type>[Null/Not Null][DEFAULT <預設值>],
            <欄位名2><data_type>[Null/Not Null][DEFAULT <預設值>],
                                ...
            <欄位名n><data_type>[Null/Not Null][DEFAULT <預設值>],
            PRIMARY KEY(<欄位名>),
            UNIQUE(<欄位名>),
            FOREIGN KEY(<欄位名>) REFERENCES <表格名(欄位名)>[ON DELETE.../ON UPDATE...]
        );
        ```
    - 範例：
        ```sql
        CREATE TABLE Employee
        (
            Ssn CHAR(10) NOT NULL,
            Name CHAR(10) NOT NULL,
            Address VARCHAR(50),
            DeptId INT,
            ProjId INT,
            Salary NUMERIC(8,1) NOT NULL DAFAULT 18000,
            PRIMARY KEY(Ssn),
            UNIQUE(EmpId),
            FOREIGNER KEY(ProjId) REFERENCES Project(Pno),
            FOREIGNER KEY(DeptId) REFERENCES Department(Dno) ON DELETE Cascade
        );
        ```
    - ON DELETE/ON UPDATE處理動作
        - RESTRICT：發生違反完整性限制的操作時，DBMS不允許該操作進行
        - CASCADE：發生違反完整性限制的操作時，外來鍵內的資料連帶更新或刪除
        - SET NULL：發生違反完整性限制的操作時，外來鍵內的資料設為空值
- DROP TABLE：刪除一個關連表格
    - 格式：DROP TABLE <表格名>
    - 範例：DROP TABLE Employee;
    - 表格間有外來鍵存在時的建表、刪表順序
        - 建表順序
            - 先建立被參考表格，再建立參考表格
            - 要讓外來鍵有對象可以參考
        - 刪表順序
            - 先刪除參考表格，再刪除被參考表格
            - 要讓外來鍵有對像可以參考
- ALTER TABLE：更改一個關聯表格中的某個欄位的基本定義與限制
    - 增加(ADD)欄位、刪除(DROP)欄位
    - 修改(ALTER/CHANGE/MODIFY)欄位預設值、定義或名稱
    - 更改表格名稱(RENAME)
    - *更改表格類型(ENGINE)*
    - 增加欄位
        - 格式
        ```sql
           ALTER TABLE <表格名>
           ADD <新欄位名> <data_type> [NULL/NOT NULL][DEFAULT <預設值>];
        ``` 
        - 例如
        ```sql
            ALTER TABLE Employee
            ADD Sex CHAR(1)
        ```
    - 刪除欄位
        - 格式
        ```sql
           ALTER TABLE <表格名>
           DROP <欄位名> [RESTRICT/CASCADE]
        ``` 
        - 例如
        ```sql
            ALTER TABLE Employee
            DROP Sex CHAR(1)
        ```
    - 修改欄位
        - 新增、修改或刪除預設值
        - 更改欄位定義(不含修改欄位名稱)
        - 更改欄位定義(含修改欄位名稱)

# DML
- INSERT
    - 格式
    ```sql
        INSERT INTO <table_name>[(attribute1,attribute2...)]
        VALUES(...)
    ```
    - 範例(插入部分欄位資料)
    ```sql
        INSERT INTO Employee (Ssn,Name,Address)
        VALUES('L124400977','王曉明','台中'); 
    ```
    - 範例(插入全部欄位資料)
    ```sql
        INSERT INTO Employee
        VALUES('L124400977','王曉明','台中'); 
    ```
- DELETE
- UPDATE

# DQL
- 用以查詢資料庫的相關資料
- 語法
    ```sql
    SELECT <attribute_list>
    FROM <table_list>
    WHERE <condition>
    GROUP BY <grouping_attributes>
    HAVING <grouping_condition>
    ORDER BY <column_name> ASC/DESC
    ```
- 執行順序
    ```
        FROM->WHERE->GROUP BY->HAVING->SELECT->ORDER BY
    ```
- 欄位名稱重覆處理
    - 問題：位於不同表格中的同名欄位被放在一起使用的情況
    - 這些同名欄位的名稱在使用時，需加上表格名稱
- 聚合函數
    - COUNT(attribute_name)：計算屬性個數。
    - SUM(attribute_name)
    - AVG(attribute_name)
    - MAX(attribute_name)
    - MIN(attribute_name)
# 範例題組

1. 列出所有供應商名稱
```sql
    SELECT 供應商名稱
    FROM Supplier
```
2. 列出所有重量在20以上，且不為黑色的零件名稱、顏色、重量
```sql
SELECT 零件名稱,顏色,重量
FROM Component
WHERE 顏色 != '黑' AND
      重量 >= 20;
```
3. 依照數量由小到大列出供應商S1所參與之專案名稱、零件名稱以及數量
```sql
    SELECT 專案名稱,零件名稱,數量 
    FROM Project_supp_Component, Component, Project
    WHERE 供應商代號='S1' AND
          Project_supp_Component.零件代號 = Component.零件代號 AND
          Project_supp_Component.專案代號 = Component.專案代號
    ORDER BY 數量;
```
![](http://i.imgur.com/UILmh7Q.png)

    
