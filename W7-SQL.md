# Week 7. SQL   

# 課程與連結
- [Week 7 SQL](https://cs50.harvard.edu/x/2024/weeks/7/)

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/1RCMYG8RUSE/0.jpg)](https://www.youtube.com/watch?v=1RCMYG8RUSE)

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/yMqldbY2AAg/0.jpg)](https://www.youtube.com/watch?v=yMqldbY2AAg)
# 目錄：
- [Flat-File Database](#flat-file-database)
- [Relational Databases](#relational-databases)
- [Shows & Schema](#shows-and-schema)
- [JOINs](#JOINs)
- [Indexes](#indexes)
- [Using SQL in Python](#using-SQL-in-Python)
- [Race Conditions](#race-conditions)
- [Using SQL in Python and SQL Injection Attacks](#Using-SQL-in-Python-and-SQL-injection-attacks)
- [Reference](#reference)


## Flat-File Database
是一種將資料儲存在純文字檔案中的資料庫，欄位之間用分隔字元（通常是逗號或製表符）分隔。

但由於其簡單的結構，平坦檔資料庫中表示的「表」僅支援有限的功能，例如記錄和欄排序。

常見的平坦資料庫檔案包含:
1. 逗號分隔值（Comma-Separated Values，CSV）
2. 制表符分隔值（Tab-Separated Values，TSV）
3. 任何分隔字元分隔值（Delimiter Separated Values，DSV）。

## Relational Databases
有別於平坦檔資料庫 (Flat-File Database)，關聯式資料庫 (Relational Databases) 是建立在關聯模型基礎上的資料庫，用於儲存並存取  
相關的資料點，並支援資料庫特定的語言(e.g. SQL, QUEL)。

常見的關聯式資料庫管理系統(Relational Databases management system, RDBMS) 包含但不限 Oracle、MySQL、PostgreSQL、Access、  
Microsoft SQL Server及 SQLite(本周課堂及作業使用) 皆遵守 ACID 的交易特性。

**什麼是 ACID ? 可參考以下影片說明**

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/GAe5oB742dw/0.jpg)](https://www.youtube.com/watch?v=GAe5oB742dw)

## Shows and Schema

![IMAGE ALT TEXT HERE](https://cs50.harvard.edu/x/2024/notes/7/cs50Week7Slide025.png)

上圖為個體關係圖 (entity-relationship diagram, ERD) 適合用來呈現(Shows) 資料庫及資料表(Tables)的描述(Schema)，包含資料庫  
所有存在的表格、表格與表格之間是否有關連(e.g. 一對多或多對多)。

此外更詳細的 ERD 圖可能還有包含資料型別(Data Types)、約束條件(Constraints) 等描述。

## JOINs

- **Databases empower us to organize information into tables efficiently.**
    - We don't always need to store every possible relevant piece of information in the same table, but can use relationships  
    across the tables to let us pull information from where we need it.

在關聯式資料庫的世界，可以將資料表想像成拼圖，只要資料表與資料表之間存在相同資料的欄位，即可使用 SQL 的 JOIN 語法或  
子查詢(Sub Query) 的方式，來實現關聯查詢(若不考慮效能，兩張表之間是否先建立主鍵及外鍵的關聯並非必要，惟實務上應盡量  
避免)，將不同資料表的資料，依查詢條件拼湊於同一張表。

## Indexes

幫資料表的欄位適當建立索引(Indexs)，能為資料庫的查詢(Query) 提供效能上的優化，背後的演算法可能為 B-tree, B+ tree 或 Hash Index

而使用索引提高查詢效能的代價，除了較高的空間複雜度，為了維持索引背後的樹狀結構，將加重資料庫新增、修改或刪除資料時的負擔。

另外幫兩張資料表設定主鍵(Primary Keys) 及外鍵(Foreign Keys) 的關聯時，除了說明這兩張資料表具有一對多的關係，主鍵本身即預設索引、  
自動遞增編號(Auto Increment) 及唯一性(Unique) 等約束的效果。

## Race Conditions

在面對高併發(高流量)的請求時，可能導致資料庫發生競爭危害(Race Conditions)，需要工程師思考如何避免資料庫的資料被錯誤更新。

好在現今資料庫管理系統大都相當成熟，透過日誌(Log) 保留交易(Transaction)前的狀態，當發生錯誤時會將結果全部退回(Rollback)，且  
經由序列化(Serialization) 的交易安排，確保資料更新的請求將被依序執行，完整更新資料的內容。

## Using SQL in Python and SQL Injection Attacks

Python 具有支援 SQL 語法的函式庫(Library)，可以經由 Python 來執行 SQL 增、刪、改、查等操作。

惟撰寫程式時仍要留心使用者有可能惡意輸入具串改程式碼的內容，達成注入攻擊(Injection Attacks)的效果。

避免方式除了針對使用者輸入的內容作額外處理，也可以使用 ORM/ODM 等套件工具來減少注入攻擊的發生。


## Reference
[什麼是平坦檔資料庫？](https://navicat.com/cht/company/aboutus/blog/1725-what-is-a-flat-file-database.html)