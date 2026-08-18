# AGENTS.md

## 使用者介面語言

- 所有代理回報、權限理由與新增註解使用台灣繁體中文（zh-TW）。
- Godot、.NET、SSH、Rider、Fleet、Visual Studio、VS Code、VSCodium、MonoDevelop、GDScript、Vulkan 等專有名詞保留原文。
- 翻譯採台灣用語，例如 `Audio Buses` 使用「音訊匯流排」，避免使用「總線」等非台灣慣用詞。

## 倉庫定位

- 此資料夾是 Godot `4.7.1-stable` 自訂引擎原始碼，不是 FAC 遊戲內容。
- 上游 Git 倉庫識別為 `https://github.com/godotengine/godot.git`；目前本機實體路徑為 `O:\Github Repositories\GodotSource-4.7.1`。
- 不得只依賴磁碟代號定位。換裝置時依序使用：目前專案上層的 `GodotSource-4.7.1`、Git remote 為 `https://github.com/godotengine/godot.git` 且分支／標籤符合 `4.7.1` 的倉庫、最後才採用本文件記錄的實體路徑。
- 目前插件翻譯驗證專案的實體路徑為 `O:\Github Repositories\godot-card-game`，可攜式位置為引擎倉庫同層的 `godot-card-game`；該資料夾目前沒有 `.git`，不可虛構 GitHub remote。
- 引擎修改、建置方式、翻譯及部署紀錄只寫入本資料夾，不得放入 `FAC-Pre/Docs/`。
- FAC 專案只能保留引擎版本、執行檔路徑及本文件位置等必要環境引用。

## 已採用的自訂功能

- Editor Settings 鍵：`interface/editor/behavior/window_close_action`。
- 選項：`退出`、`返回專案列表`、`每次詢問`。
- 預設值：`返回專案列表`。
- `X`／`Alt+F4` 的三種分支都必須沿用 Godot 原有的未儲存內容、執行中遊戲與外掛確認流程。
- 實作入口：`editor/editor_node.cpp` 的 `NOTIFICATION_WM_CLOSE_REQUEST`。
- 設定註冊：`editor/settings/editor_settings.cpp`。
- EditorPlugin 使用獨立的 `editor_plugins` 翻譯網域；專案翻譯會載入此網域，語系自動跟隨 Editor Settings 的編輯器語言，不改動遊戲主翻譯網域。
- 繁中介面的 `Ctrl+D`／`Duplicate` 顯示為「創建副本」；快捷鍵與建立副本的既有行為不變。
- Node 通用檢視器欄位採「中文（English）」格式，保留英文原文以方便對照 Godot 英文教學與 API 文件。

## 繁中翻譯範圍

- 翻譯來源：`editor/translations/editor/zh_Hant.po` 與 `editor/translations/properties/zh_Hant.po`。
- `.NET` 動態設定位於 `modules/mono/editor/GodotTools/GodotTools/GodotSharpEditor.cs`；非專有列舉值需在建立 hint 時呼叫 `.TTR()`，不能只把字串加入 PO。
- 目前已補關閉動作、`.NET` 設定、`.NET` 非專有選項、`Audio Buses` 及其相關顏色欄位。
- 不為追求中文化而翻譯產品名、協定名、API 名稱、檔案格式或程式識別字。

## 建置與部署

1. 執行 SCons 建置 Mono editor。
2. 若修改 GodotTools，使用新 editor 重新產生 `modules/mono/glue`。
3. 執行 `modules/mono/build_scripts/build_assemblies.py --godot-output-dir ./bin`。
4. 部署 editor、console 及完整 `bin/GodotSharp`；不可只替換執行檔。
5. 以部署後 console 驗證 `--version`，並以 `--headless --editor --path <project>` 驗證 Mono Editor 可載入專案。

完整命令與本機路徑以 `GodotCustomBuild.md` 為準。

## 修改規則

- 優先保持修改範圍小且可由上游版本重新套用。
- 不直接修改 FAC 遊戲邏輯來繞過引擎問題。
- 不刪除或重設未知工作樹修改。
- 未經使用者要求不得提交、推送或發布二進位。
- 完成後回報修改檔案、C++／C#／翻譯建置結果、部署狀態及仍需人工確認的介面行為。
