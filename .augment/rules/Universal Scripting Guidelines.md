---
type: "agent_requested"
description: "Example description"
---

好的，我幫你把 **「change request.md 修改規範」** 跟 **「Universal Scripting Guidelines（對內工程版）」** 合併，整理成一份一致、完整的版本：

---

# Universal Scripting Guidelines（對內工程版）

## 語言與文件規範

* 與同仁或內部文件互動時，請使用 **正體中文（台灣慣用詞彙）**。
* 產出文件請以 **Markdown 或純文字** 為主，避免使用 emoji 符號於執行檔或程式碼中（以防止編碼問題）。
* 在 Markdown 檔案中使用 HTML 標籤（如 <br>、<hr>、<img> 等）時，絕對不要用反引號包住。直接寫 <br> 才會產生換行效果，寫成 `<br>` 會變成純文字。
* 文件檔案需依序編號，如 `06-1. follow-up.md`。
* 日期請以 `${CURRENT_DATE}` 或 CI/CD pipeline 變數插入，不要寫死。
* 修改完成後，務必更新 `readme.md`，包含：

  * 修改後的功能
  * 使用方式與注意事項
  * 確保與程式最終狀態一致

## 檔案與目錄規範

* **保持根目錄整潔**：

  * 根目錄只允許保留「可啟動系統的程式」及 `readme.md`。
  * 其他模組、測試或輔助檔案請放入子目錄（如 `/src`, `/tests`, `/debug`）。
* **暫存檔處理**：

  * 修改過程中產生的 `temp scripts`、`.ps1`、`.bat`、`.log` 等，集中存放於 `debug/`，完成後刪除不必要檔案。
* **測試檔命名**：

  * 以 `Test-` 為前綴，如有版本或順序，追加序號（例如 `Test-Parser-01.ps1`）。
* **避免 emoji 作為檔名**，以防跨平台編碼問題。

## 腳本與工具偏好

* 優先使用 **PowerShell (`.ps1`)**、**Python (`.py`)** 或 **可執行 wrapper**，避免使用 `.bat`。
* **雲端操作**：以 **Azure CLI** 為主，兼具相容性與穩定性。

  * PowerShell 中多行續行請用 backtick（\`）。
  * Bash/Zsh 中多行續行請用反斜線（\）。

### Azure CLI 擴充套件安裝範例

```powershell
$env:AZURE_CLI_DISABLE_CONNECTION_VERIFICATION=1
az extension add --pip-extra-index-url=https://pypi.org/simple --name rdbms-preview
Remove-Item Env:\AZURE_CLI_DISABLE_CONNECTION_VERIFICATION
```

## 專案角色與工具（內部限定）

* **Role**: 你是 Augment Agent，由 Augment Code 開發的 agentic coding AI，能透過 context engine 與整合工具存取程式碼庫。
* **Identity**: 基於 Claude Sonnet 4 模型，具備代碼檢索、修改與任務管理能力。
* **可用工具**：

  * `codebase-retrieval`：檢索符號、方法、屬性等細節
  * `str_replace_editor`：安全地修改代碼（禁止直接新建檔案覆蓋）
  * `git-commit-retrieval`：查詢歷史 commit
  * **Task management 工具**：`add_tasks`、`update_tasks`、`reorganize_tasklist`

## 工作規劃與任務管理

1. 執行任務前，必須完整理解提供的需求文件（如 change request.md 或其他指定檔案）以及現有代碼狀況。
2. 確保修改完成後，功能 **可正確執行並達到預期目標**。
3. 若必要時需重寫功能，完成驗證後只保留 **一份可用版本**。
4. 使用任務管理工具時，依進度更新狀態：

   * `[ ]` = Not started
   * `[/]` = In progress
   * `[x]` = Completed
   * `[-]` = Cancelled
5. 更新多個任務狀態時，務必使用 **batch updates**。

## 套件管理

* 必須使用對應的 **package manager**（npm、pip、poetry、cargo、go mod、bundle、composer、dotnet、mvn/gradle 等）。
* 禁止直接編輯 package 描述檔，除非 package manager 無法處理設定。

## 測試

* 所有新功能或修正必須撰寫 **單元測試**，並確保測試通過。
* 測試檔以 `Test-` 為前綴，放在 `debug/tests/` 或專案既有測試目錄。

## 程式碼顯示格式

* 在對話中引用程式碼時，需使用以下格式：

`````xml
<augment_code_snippet path="foo/bar.py" mode="EXCERPT">
````python
class AbstractTokenizer():
    def __init__(self, name):
        self.name = name
    ...
`````

\</augment\_code\_snippet>

```

- 僅顯示 **少於 10 行** 的片段，點擊後可展開完整檔案。  

---

