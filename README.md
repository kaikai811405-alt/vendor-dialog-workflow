# bridge-notion-flow

廠商對話整理自動化工作流——將 Teams / WeChat / Outlook 信件內容，透過 Claude + Notion MCP 自動整理並寫入 Notion 專案任務清單。

## 專案簡介

**目標**：PM 貼上廠商對話或信件，Claude 自動執行 7 步驟 SOP，輸出摘要卡確認後寫入 Notion。

**執行方式（方案 A）**：直接在 Claude Code 對話框貼上內容 → Claude 執行 → 確認 → 寫入 Notion。

## 7 步驟 SOP

1. **比對聯絡人** — 查詢 Notion 聯絡人資料庫（優先 Email，其次姓名）
2. **判斷專案** — 比對 Notion 專案頁面（A704、A701、A702 等）
3. **判斷任務** — 在該專案內比對對應任務清單
4. **比對現有 Action Items** — 確認是否有待辦事項已被本次資料證實完成
5. **整理內容** — 摘要、重點決策、Action Items、SN 編號
6. **輸出摘要卡** — 一次列出所有結果等確認（含⚡已完成項目提示）
7. **確認後寫入 Notion** — 更新執行紀錄與待辦事項，完成項目打勾並記錄時間

## 核心原則

- 所有步驟連續執行，最後才輸出摘要卡讓使用者一次確認
- 遇到聯絡人無法比對、無法判斷專案歸屬、缺少必要資訊時立即停下詢問
- **RMA / Bug / Event / Task 均寫入對應專案任務清單**（A702_任務清單、A704_任務清單），不寫入溝通記錄資料庫
- Action Item 有完成跡象時，主動告知使用者確認後打勾並記錄完成時間

## Notion 資料庫對應

| 類型 | 寫入位置 |
|------|----------|
| RMA | `{專案}_任務清單`，類別：1_RMA |
| Bug | `{專案}_任務清單`，類別：0_Bug |
| Event / 一般任務 | `{專案}_任務清單`，類別：2_Event |
| ~~溝通記錄~~ | ~~不使用~~ |

## 安裝

```bash
pip install -r requirements.txt
```

## MCP 整合

- **Notion MCP** — 透過 Claude 桌面版整合（`mcp__claude_ai_Notion__` 系列工具）
- 聯絡人資料庫：`collection://c151df96-324a-49a0-a8ab-e9175b11103c`
- A704 任務清單：`collection://2b72aaea-4ff1-8166-829c-000b3c67d84d`
- A702 任務清單：`collection://2a82aaea-4ff1-8145-8239-000b3db6a958`
