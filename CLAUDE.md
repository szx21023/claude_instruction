# CLAUDE.md

本檔案為本專案的開發規範，Claude 每次協作時皆會自動載入並遵循。

## 專案概述

- 後端服務，技術棧：Python 3.11+ + FastAPI。
- (待補：一句話說明這個服務是做什麼的)

## 常用指令

- 安裝依賴（含開發工具）：`pip install -e ".[dev]"`
- 首次啟用 pre-commit：`pre-commit install`
- 啟動開發伺服器：`uvicorn app.main:app --reload`
- 跑測試：`pytest`
- lint + 格式檢查：`ruff check . && ruff format --check .`
- 自動修正：`ruff check --fix . && ruff format .`
- 手動對所有檔案跑 pre-commit：`pre-commit run --all-files`

## 程式風格

### 語言與工具鏈
- Python 3.11+；所有函式簽名必須有 type hints，不允許裸露的 `Any`
- 格式化與 lint 統一用 `ruff`（`ruff format` + `ruff check`），提交前必須零錯誤；ruff 規則設定見 `pyproject.toml` 的 `[tool.ruff]`
- lint/格式在 `git commit` 時由 pre-commit 自動執行並擋關（設定見 `.pre-commit-config.yaml`）；首次需執行 `pre-commit install`
- import 排序交給 ruff，不要手動調整；不使用相對 import，一律絕對 import
- 資料模型一律用 Pydantic v2（`BaseModel`），不要用裸 dict 在層與層之間傳資料

### 命名慣例
- 變數/函式：snake_case；類別：PascalCase；常數：UPPER_SNAKE
- 變數名稱用完整單字，不要用縮寫或單一字元（用 `ticket` 不要用 `t`、用 `index` 不要用 `i`）；例外：慣用的 loop 計數短名視情況可接受，但有語意時一律用完整單字
- 布林值用 is_/has_/should_ 開頭（如 is_active）
- Pydantic schema 命名：輸入用 `XxxCreate` / `XxxUpdate`，輸出用 `XxxRead` / `XxxResponse`
- 私有成員以單底線開頭 `_internal`

### FastAPI 慣例
- 路由函式只做「參數驗證 → 呼叫 service → 回傳」，商業邏輯一律放 service 層，不寫在 router 裡
- 依賴注入用 `Depends`，不要在函式內自行建立 DB session / client
- 所有 I/O（DB、外部 API）一律用 async；不要在 async 路由裡呼叫同步阻塞函式
- response_model 一定要明確指定，不要回傳未經 schema 過濾的 ORM 物件
- 路徑用複數名詞（`/users`、`/users/{id}`），不要動詞化路徑

### 錯誤處理
- 業務錯誤拋自訂 exception（見 app/exceptions.py），由統一的 exception handler 轉成 HTTP 回應
- 不要在 router 直接 raise `HTTPException` 散落各處；不要吞例外（禁止空的 except）
- 禁止 print；一律用 logging 模組，並帶上 request 相關 context

### 結構與複雜度
- 函式超過 50 行或巢狀超過 3 層就拆分
- 優先 early return，避免深層 else 巢狀
- 設定值只從 config（pydantic-settings）讀取，禁止散落的 os.getenv

### 註解
- 註解寫「為什麼」，不寫「做什麼」
- 公開函式與 service 方法用 docstring 說明參數、回傳、可能拋的例外
- 不要為顯而易見的程式碼加註解
