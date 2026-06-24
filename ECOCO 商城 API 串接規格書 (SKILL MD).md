# ECOCO 商城 API 串接規格書

## (綠界金流 + 易發票)

Version: 1.0

---

# 1. 文件目的

定義 ECOCO 商城與下列系統之間的資料交換規格：

1. 商城系統
2. 綠界金流 ECPay
3. 易發票系統
4. 財務對帳系統

作為系統開發、測試及驗收依據。

---

# 2. 系統架構圖

```text
┌─────────────┐
│   消費者     │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ ECOCO商城   │
└──────┬──────┘
       │建立訂單
       ▼
┌─────────────┐
│ 綠界金流    │
└──────┬──────┘
       │付款完成通知
       ▼
┌─────────────┐
│ ECOCO商城   │
└──────┬──────┘
       │開立發票
       ▼
┌─────────────┐
│ 易發票API   │
└──────┬──────┘
       │回傳發票資訊
       ▼
┌─────────────┐
│ 財務對帳系統 │
└─────────────┘
```

---

# 3. 訂單生命週期

## 狀態流程

```text
建立訂單
 ↓
待付款
 ↓
付款成功
 ↓
已開立發票
 ↓
已出貨
 ↓
完成
```

---

## 異常流程

```text
付款失敗
 ↓
訂單取消
```

```text
已付款
 ↓
退款
 ↓
發票作廢/折讓
 ↓
訂單結束
```

---

# 4. 資料表設計

## OrderMaster

訂單主檔

| 欄位名稱           | 型態            | 說明   |
| -------------- | ------------- | ---- |
| OrderNo        | varchar(30)   | 訂單編號 |
| MemberID       | varchar(30)   | 會員編號 |
| OrderAmount    | decimal(10,2) | 原始金額 |
| DiscountAmount | decimal(10,2) | 折扣   |
| PaidAmount     | decimal(10,2) | 付款金額 |
| Status         | varchar(20)   | 訂單狀態 |
| CreateTime     | datetime      | 建立時間 |

---

## OrderDetail

訂單明細

| 欄位名稱        | 型態            |
| ----------- | ------------- |
| OrderNo     | varchar(30)   |
| SKU         | varchar(30)   |
| ProductName | varchar(100)  |
| Qty         | int           |
| UnitPrice   | decimal(10,2) |
| Amount      | decimal(10,2) |

---

## PaymentRecord

付款紀錄

| 欄位名稱            | 型態            |
| --------------- | ------------- |
| TradeNo         | varchar(50)   |
| MerchantTradeNo | varchar(50)   |
| PaymentType     | varchar(30)   |
| TradeAmt        | decimal(10,2) |
| PaymentDate     | datetime      |

---

## InvoiceRecord

發票紀錄

| 欄位名稱          | 型態            |
| ------------- | ------------- |
| InvoiceNo     | varchar(20)   |
| OrderNo       | varchar(30)   |
| InvoiceDate   | datetime      |
| InvoiceAmount | decimal(10,2) |
| CarrierType   | varchar(20)   |
| CarrierNo     | varchar(50)   |

---

## AllowanceRecord

折讓單紀錄

| 欄位名稱         | 型態            |
| ------------ | ------------- |
| AllowanceNo  | varchar(30)   |
| InvoiceNo    | varchar(20)   |
| RefundAmount | decimal(10,2) |
| CreateDate   | datetime      |

---

# 5. 綠界串接規格

## 建立交易

商城送出：

```json
{
  "MerchantTradeNo":"EC202600001",
  "TradeDesc":"ECOCO商城訂單",
  "TotalAmount":"500",
  "ItemName":"ECOCO環保袋"
}
```

---

## 付款成功回傳

綠界 POST ReturnURL

```json
{
  "MerchantTradeNo":"EC202600001",
  "TradeNo":"240623000001",
  "TradeAmt":"500",
  "PaymentDate":"2026/06/23 12:00:00",
  "PaymentType":"Credit_CreditCard"
}
```

---

# 6. 易發票串接規格

## 開立發票

Trigger：

付款成功後自動執行

---

送出資料

```json
{
  "OrderNo":"EC202600001",
  "CustomerName":"王小明",
  "Email":"test@test.com",
  "Amount":"500"
}
```

---

回傳資料

```json
{
  "InvoiceNo":"AB12345678",
  "InvoiceDate":"2026-06-23",
  "RandomNumber":"1234"
}
```

---

# 7. 載具規格

## 手機條碼

格式

```text
/ABC1234
```

---

## 自然人憑證

格式

```text
AA12345678901234
```

---

## 愛心碼

格式

```text
16888
```

---

# 8. 發票開立邏輯

## 個人戶

條件：

* 無統編

開立：

B2C電子發票

---

## 公司戶

條件：

* 有統編

開立：

B2B電子發票

---

# 9. 退款邏輯

## 全額退款

```text
退款完成
 ↓
發票作廢
 ↓
訂單關閉
```

---

## 部分退款

```text
退款完成
 ↓
建立折讓單
 ↓
保留原發票
```

---

# 10. Webhook 設計

## 綠界付款通知

Endpoint

```text
POST /api/payment/callback
```

功能：

* 更新付款狀態
* 建立付款紀錄
* 呼叫發票API

---

## 發票通知

Endpoint

```text
POST /api/invoice/callback
```

功能：

* 更新發票資訊
* 寫入InvoiceRecord

---

# 11. Sequence Diagram

```text
會員
 │
 │下單
 ▼
商城
 │
 │建立交易
 ▼
綠界
 │
 │付款成功
 ▼
商城
 │
 │建立發票
 ▼
易發票
 │
 │回傳發票號碼
 ▼
商城
 │
 │寄送Email
 ▼
會員
```

---

# 12. 財務對帳規格

## 日對帳

核對：

1. 商城訂單
2. 綠界收款
3. 發票開立

公式：

```text
付款成功金額
=
已開立發票金額
```

---

## 月對帳

公式：

```text
商城銷售額
=
綠界實收
=
發票總額
－折讓
－作廢
```

---

# 13. 驗收標準

## 金流

* [ ] 信用卡付款成功
* [ ] ATM付款成功
* [ ] 超商付款成功

## 發票

* [ ] B2C發票
* [ ] B2B發票
* [ ] 手機載具
* [ ] 愛心碼

## 退款

* [ ] 全額退款
* [ ] 部分退款
* [ ] 發票作廢
* [ ] 折讓單建立

## 對帳

* [ ] 訂單金額一致
* [ ] 金流金額一致
* [ ] 發票金額一致

```
```
