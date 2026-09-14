# claude_instruction

本專案是一個 **`CLAUDE.md` 範本（template）**，供其他所有專案沿用。

它本身不含應用程式碼，目的是提供一套統一的開發規範與工具設定，讓每個新專案都能以相同的標準起步，並讓 Claude 在協作時自動載入並遵循這些規則。

## 內容

| 檔案 | 說明 |
|------|------|
| `CLAUDE.md` | 核心：FastAPI 後端服務的開發規範（架構分層、命名慣例、程式風格、錯誤處理等），Claude 每次協作會自動載入 |
| `pyproject.toml` | 相依套件、`ruff` lint/格式設定、`pytest` 設定 |
| `.pre-commit-config.yaml` | git commit 時自動執行的 hooks（`ruff` lint/format + 基本檢查） |

## 如何在新專案中使用

1. 複製 `CLAUDE.md`、`pyproject.toml`、`.pre-commit-config.yaml` 到新專案根目錄。
2. 依新專案調整 `pyproject.toml` 的 `name`、`description`，並補上 `CLAUDE.md` 的專案概述（`待補` 處）。
3. 安裝依賴與啟用 pre-commit：
   ```bash
   pip install -e ".[dev]"
   pre-commit install
   ```
4. 依 `CLAUDE.md` 的目錄約定建立 `app/` 結構後即可開始開發。

## 常用指令

- 安裝依賴（含開發工具）：`pip install -e ".[dev]"`
- 首次啟用 pre-commit：`pre-commit install`
- 啟動開發伺服器：`uvicorn app.main:app --reload`
- 跑測試：`pytest`
- lint + 格式檢查：`ruff check . && ruff format --check .`
- 自動修正：`ruff check --fix . && ruff format .`

> 詳細規範請見 [`CLAUDE.md`](./CLAUDE.md)。
