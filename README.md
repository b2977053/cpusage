# Copilot Usage Analyzer

這是一個用於分析 GitHub Copilot 聊天記錄並估算成本的命令列工具。它會讀取本地的 Copilot Session Logs (.jsonl) 與 `~/.copilot/logs` 的 usage 記錄，優先使用 usage Token，若 usage 不可用則回退為 session-state 估算，並根據模型定價表計算預估費用。

## 特色

- **自動偵測路徑**：預設讀取使用者家目錄下的 `~/.copilot/logs` usage 記錄與 Copilot 記錄檔 (`~/.copilot/session-state`)，若 usage 不可用則回退為 session-state 估算。
- **支援多種模型計價**：包含 GPT-5, GPT-4, Claude 等模型的預估價格。
- **靈活統計**：支援按日、月、小時進行統計。
- **排名與過濾**：可根據成本排序，並限制顯示筆數。
- **JSON 輸出**：支援 JSON 格式輸出，方便與其他工具整合。
- **跨平臺支援**：支援 Windows, macOS, Linux。

## 安裝

```bash
npm install -g @willh/cpusage
```

或者直接使用 `npx` 執行：

```bash
npx @willh/cpusage
```

## 使用方法

### 基本使用

直接執行命令，程式會自動尋找預設路徑下的記錄檔，並按日期降冪排序顯示：

```bash
cpusage
```

---

### Windows 使用者快速指令

在 Windows 環境下，Copilot 的 session 記錄通常儲存在 VS Code 的資料夾中。你可以直接在 **PowerShell** 複製貼上對應版本的指令進行分析：

#### **VS Code (穩定版)**

```powershell
# 設定路徑並按月統計輸出 JSON
$env:SESSION_DIR="$env:APPDATA\Code\User\workspaceStorage"
cpusage --unit month --json

```

#### **VS Code (Insiders 版)**

```powershell
# 設定路徑並按月統計輸出 JSON
$env:SESSION_DIR="$env:APPDATA\Code - Insiders\User\workspaceStorage"
cpusage --unit month --json

```

---

### 命令列參數

| 參數 | 說明 |
| --- | --- |
| `-h`, `--help` | 顯示說明訊息 |
| `--rank` | 依預估成本降冪排序（預設顯示前 10 筆） |
| `--limit <n>` | 限制顯示筆數（預設：若使用 `--rank` 為 10，否則顯示全部） |
| `--unit <unit>` | 統計單位：`day` (預設), `month`, `hour` |
| `--verbose` | 顯示詳細分析資訊（記錄檔路徑與數量） |
| `--list-price` | 顯示目前的模型定價表 |
| `--json` | 以 JSON 格式輸出結果（不包含統計標頭） |

### 範例

**顯示成本最高的前 5 天：**
```bash
cpusage --rank --limit 5
```

**按月統計並輸出 JSON：**
```bash
cpusage --unit month --json
```

**查看目前的定價表：**
```bash
cpusage --list-price
```

### 指定記錄檔路徑

如果你的記錄檔存放在其他位置，可以透過設定 `SESSION_DIR` 環境變數來指定：

**Windows (PowerShell):**
```powershell
$env:SESSION_DIR="C:\path\to\your\logs"
cpusage
```

**Linux / macOS:**
```bash
SESSION_DIR=/path/to/your/logs cpusage
```

## 開發與建置

本專案使用 TypeScript 開發並使用 Bun 進行建置。

### 前置需求

- [Bun](https://bun.sh/) (最新版本)

### 安裝依賴

```bash
bun install
```

### 執行開發模式

```bash
bun run start
```

### 建置專案

編譯 TypeScript 原始碼至 `dist/` 目錄：

```bash
bun run build
```

## CI/CD

本專案包含 GitHub Actions Workflow，每次 Push 或 Pull Request 會自動執行建置測試。

## 授權

MIT
