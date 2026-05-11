# Architecture

## 系統架構

```
使用者貼上對話/信件
        ↓
  Claude Code (recapchat skill)
        ↓
  Notion MCP (mcp__claude_ai_Notion__)
   ├─ notion-search   (比對聯絡人/專案/任務)
   ├─ notion-fetch    (讀取現有內容與 Action Items)
   ├─ notion-update-page  (更新屬性/內文/打勾)
   └─ notion-create-pages (新建任務)
        ↓
  Notion 專案任務清單
```

## Notion 資料庫結構

```
Notion Workspace
├─ 聯絡人資料庫 (collection://c151df96-...)
│   └─ 每位廠商聯絡人（Email、公司、負責專案）
├─ A704
│   └─ A704_任務清單 (collection://2b72aaea-...-8166-...)
│       ├─ PNC（耳套材質與洩漏改善）
│       ├─ 可拆電池安規確認
│       ├─ 電池背膠與包覆設計確認
│       └─ ...
└─ A702
    └─ A702_任務清單 (collection://2a82aaea-...-8145-...)
        ├─ 耳罩蝕刻裝飾網翹起（RMA）
        └─ ...
```

## 模組說明

| 模組 | 職責 |
|------|------|
| recapchat skill | 主控 SOP 流程，解析對話/信件並執行 7 步驟 |
| Notion MCP | 讀寫 Notion workspace（搜尋、讀取、新建、更新）|
| 聯絡人資料庫 | 廠商/ASUS 內部人員對應表 |
| 專案任務清單 | 各專案（A702/A704 等）的任務、RMA、Bug 追蹤 |

## 關鍵設計決策

- **方案 A（直接對話）**：不需要 API server，直接透過 Claude 桌面版 MCP 整合，最低建置成本
- **任務寫入位置**：統一寫入 `{專案}_任務清單`，不使用溝通記錄資料庫
- **Action Item 追蹤**：每次處理資料時主動比對現有 checkbox，即時更新完成狀態
