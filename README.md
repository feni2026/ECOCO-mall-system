# ECOCO 商城管理系統

> 企業內部後台管理平台｜規劃設計階段

---

## 📌 專案簡介

ECOCO 商城管理系統是一套企業內部後台管理平台（B2B），提供完整的商城營運管理功能，涵蓋訂單管理、發票管理、退款處理、RBAC 權限控管與 KPI 數據儀表板，協助企業高效管理商城日常作業。

---

## 🏗️ 系統架構

```
ecoco-mall-system/
│
├─ README.md
│
├─ docs/                    # 系統規格文件
│  ├─ business/             # 商業需求文件
│  ├─ finance/              # 財務流程文件
│  ├─ operation/            # 營運操作文件
│  ├─ security/             # 資安規範文件
│  ├─ qa/                   # 品質保證文件
│  └─ report/               # 報表規格文件
│
├─ database/                # 資料庫設計
│  ├─ erd.md                # 實體關係圖
│  └─ schema.sql            # 資料庫結構 SQL
│
├─ frontend/                # 前端應用程式 (Next.js)
│
├─ backend/                 # 後端 API 服務 (Next.js API Routes)
│
└─ deployment/              # 部署設定檔
```

---

## 🛠️ 技術選型

| 層級 | 技術 |
|------|------|
| 前端框架 | [Next.js 14](https://nextjs.org/) (App Router) |
| UI 元件庫 | [Shadcn UI](https://ui.shadcn.com/) + [Tailwind CSS](https://tailwindcss.com/) |
| 資料庫 | [PostgreSQL](https://www.postgresql.org/) via [Supabase](https://supabase.com/) |
| ORM | [Prisma](https://www.prisma.io/) |
| 身份驗證 | [NextAuth.js](https://next-auth.js.org/) |
| 部署平台 | [Render](https://render.com/) (前後端) + [Supabase](https://supabase.com/) (資料庫) |
| 版本控制 | Git + GitHub |

---

## 🔑 核心功能模組

### 1. RBAC 權限管理系統
- 角色定義：超級管理員、財務人員、客服人員、一般檢視者
- 功能權限細粒度控管
- 使用者帳號管理與角色指派

### 2. 訂單管理模組
- 訂單列表查詢（多條件篩選）
- 訂單狀態追蹤與更新
- 訂單詳情檢視
- 訂單匯出（CSV / Excel）

### 3. 發票管理模組
- 電子發票開立與管理
- 發票狀態追蹤（開立、作廢、退票）
- 發票查詢與下載

### 4. 退款管理模組
- 退款申請審核流程
- 退款狀態追蹤
- 退款紀錄查詢

### 5. KPI 儀表板
- 銷售總覽（日 / 週 / 月 / 年）
- 訂單數量趨勢圖
- 退款率統計
- 發票開立統計

---

## 🗄️ 資料庫設計

詳見 [`database/erd.md`](./database/erd.md) 與 [`database/schema.sql`](./database/schema.sql)

主要資料表：

| 資料表 | 說明 |
|--------|------|
| `users` | 系統使用者 |
| `roles` | 角色定義 |
| `permissions` | 功能權限 |
| `role_permissions` | 角色權限對應 |
| `orders` | 訂單主檔 |
| `order_items` | 訂單明細 |
| `invoices` | 發票主檔 |
| `refunds` | 退款申請 |
| `products` | 商品主檔 |
| `customers` | 顧客資料 |

---

## 🚀 快速開始

### 環境需求

- Node.js >= 18.0.0
- npm >= 9.0.0
- PostgreSQL >= 15（或 Supabase 帳號）

### 本機開發

```bash
# 1. Clone 專案
git clone https://github.com/feni2026/ECOCO-mall-system.git
cd ECOCO-mall-system

# 2. 安裝前端依賴
cd frontend
npm install

# 3. 設定環境變數
cp .env.example .env.local
# 填入 Supabase 連線資訊、NextAuth Secret 等

# 4. 執行資料庫 Migration
npx prisma migrate dev

# 5. 啟動開發伺服器
npm run dev
```

開啟瀏覽器前往 `http://localhost:3000`

### 環境變數說明

```env
# Supabase
DATABASE_URL=postgresql://...
NEXT_PUBLIC_SUPABASE_URL=https://xxx.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=...
SUPABASE_SERVICE_ROLE_KEY=...

# NextAuth
NEXTAUTH_SECRET=your-secret-key
NEXTAUTH_URL=http://localhost:3000

# App
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

---

## ☁️ 部署

### 正式環境

| 服務 | 平台 | 說明 |
|------|------|------|
| 前端 + API | [Render](https://render.com/) | Web Service |
| 資料庫 | [Supabase](https://supabase.com/) | PostgreSQL |

### Render 部署步驟

1. 前往 [dashboard.render.com](https://dashboard.render.com/)
2. 新增 **Web Service** → 連結 GitHub Repo
3. 設定 Build Command：`cd frontend && npm install && npm run build`
4. 設定 Start Command：`cd frontend && npm run start`
5. 填入所有環境變數
6. 點擊 **Deploy**

---

## 📁 文件索引

| 文件 | 路徑 | 說明 |
|------|------|------|
| 商業需求 | `docs/business/` | 功能需求、使用者故事 |
| 財務流程 | `docs/finance/` | 發票、退款流程規範 |
| 營運操作 | `docs/operation/` | 系統操作手冊 |
| 資安規範 | `docs/security/` | RBAC 設計、資安政策 |
| 品質保證 | `docs/qa/` | 測試計畫、驗收標準 |
| 報表規格 | `docs/report/` | KPI 指標定義、報表格式 |
| ERD | `database/erd.md` | 資料庫實體關係圖 |
| Schema | `database/schema.sql` | 資料庫建表 SQL |

---

## 👥 開發團隊

| 角色 | 負責範疇 |
|------|----------|
| 專案負責人 | 需求規劃、系統設計 |
| 前端工程師 | Next.js / Shadcn UI |
| 後端工程師 | API Routes / Prisma |
| 資料庫設計 | PostgreSQL / Supabase |

---

## 📋 專案狀態

- [x] 需求規劃
- [x] 系統架構設計
- [x] 資料庫 Schema 設計
- [ ] 前端開發
- [ ] 後端 API 開發
- [ ] 整合測試
- [ ] 正式部署

---

## 📄 授權

本專案為企業內部私有專案，未經授權禁止對外散佈。

---

*最後更新：2026 年 6 月*
