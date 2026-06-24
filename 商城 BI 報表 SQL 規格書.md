# ECOCO Mall BI Report Specification

Version：1.0

---

# 1. 每日銷售報表

目的：

統計每日銷售額

SQL：

```sql
SELECT
    OrderDate,
    COUNT(*) AS OrderCount,
    SUM(PaidAmount) AS Revenue
FROM OrderMaster
WHERE Status='Completed'
GROUP BY OrderDate;
```

---

# 2. 商品銷售排行

```sql
SELECT
    ProductName,
    SUM(Qty) AS QtySold,
    SUM(Amount) AS Revenue
FROM OrderDetail
GROUP BY ProductName
ORDER BY Revenue DESC;
```

---

# 3. 客單價分析

```sql
SELECT
    SUM(PaidAmount) / COUNT(OrderNo)
AS AverageOrderValue
FROM OrderMaster;
```

---

# 4. 退款率分析

公式：

退款率

=

退款訂單數

÷

完成訂單數

---

# 5. 發票開立率

公式：

開票成功率

=

發票筆數

÷

付款成功筆數

目標：

100%

```
```
