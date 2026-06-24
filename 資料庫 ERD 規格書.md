# ECOCO Mall Database ERD Specification

Version：1.0

---

# 1. 資料庫架構

```text
Member
 │
 ├────< OrderMaster
 │          │
 │          ├────< OrderDetail
 │          │
 │          ├──── PaymentRecord
 │          │
 │          ├──── InvoiceRecord
 │          │
 │          └──── AllowanceRecord
 │
 └──── MemberAddress
```

---

# 2. Member

會員主檔

| 欄位         | 型態           |
| ---------- | ------------ |
| MemberID   | PK           |
| Name       | varchar(100) |
| Email      | varchar(255) |
| Phone      | varchar(20)  |
| Status     | varchar(20)  |
| CreateDate | datetime     |

---

# 3. Product

商品主檔

| 欄位          | 型態           |
| ----------- | ------------ |
| SKU         | PK           |
| ProductName | varchar(255) |
| CategoryID  | FK           |
| Price       | decimal      |
| Cost        | decimal      |
| StockQty    | int          |
| Status      | varchar(20)  |

---

# 4. OrderMaster

| 欄位             | 型態          |
| -------------- | ----------- |
| OrderNo        | PK          |
| MemberID       | FK          |
| OrderAmount    | decimal     |
| DiscountAmount | decimal     |
| PaidAmount     | decimal     |
| Status         | varchar(20) |
| CreateDate     | datetime    |

---

# 5. OrderDetail

| 欄位            | 型態      |
| ------------- | ------- |
| OrderDetailID | PK      |
| OrderNo       | FK      |
| SKU           | FK      |
| Qty           | int     |
| UnitPrice     | decimal |
| Amount        | decimal |

---

# 6. PaymentRecord

| 欄位            | 型態          |
| ------------- | ----------- |
| TradeNo       | PK          |
| OrderNo       | FK          |
| PaymentMethod | varchar(50) |
| TradeAmt      | decimal     |
| PaymentDate   | datetime    |

---

# 7. InvoiceRecord

| 欄位            | 型態          |
| ------------- | ----------- |
| InvoiceNo     | PK          |
| OrderNo       | FK          |
| InvoiceAmount | decimal     |
| InvoiceDate   | datetime    |
| CarrierNo     | varchar(50) |

---

# 8. AllowanceRecord

| 欄位           | 型態       |
| ------------ | -------- |
| AllowanceNo  | PK       |
| InvoiceNo    | FK       |
| RefundAmount | decimal  |
| CreateDate   | datetime |

```
```
