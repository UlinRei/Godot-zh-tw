# Godot 自訂編輯器建置

## 定位

這是以 Godot `4.7.2-stable` 為基礎的個人化 Mono Editor。遊戲專案不應依賴這些自訂功能，仍須能以官方相容版本開啟與執行。

## 自訂功能

### 關閉行為

Editor Settings 新增 `interface/editor/behavior/window_close_action`。主視窗收到 `X` 或 `Alt+F4` 時可選擇：

- `退出`：關閉編輯器程序。
- `返回專案列表`：啟動專案管理員後關閉目前編輯器；這是自訂版預設值。
- `每次詢問`：每次詢問本次要退出或返回專案列表。

三種分支都沿用 Godot 原有的未儲存場景、外部資源、執行中遊戲及外掛確認流程。實作入口是 `editor/editor_node.cpp` 的 `NOTIFICATION_WM_CLOSE_REQUEST`；設定註冊於 `editor/settings/editor_settings.cpp`。

### EditorPlugin 翻譯

EditorPlugin 使用獨立的 `editor_plugins` 翻譯網域。引擎會將專案翻譯載入此網域，並以 Editor Settings 的介面語言作為語系，因此插件可直接使用 `tr()` 與場景自動翻譯，而不改變遊戲主翻譯網域或遊戲語系預覽。

翻譯網域套用至 EditorPlugin 本體，以及經由插件 API 加入的 InspectorPlugin、DebuggerPlugin、GizmoPlugin、工具列、側邊欄、Dock 與底部面板控制項。

### 台灣繁中介面

- `Ctrl+D`／`Duplicate` 顯示為「創建副本」，快捷鍵與建立副本行為不變。
- Inspector 的分組標籤採「中文（English）」格式，方便對照英文教學與 API 文件。
- Inspector 的屬性名稱樣式預設為 `Localized`，因此翻譯會顯示在欄位與分組標籤；滑鼠懸浮內容保留為屬性的功能說明。
- 原生屬性的懸浮說明同時顯示文件的台灣繁中譯文與英文原文；Node 通用分組會連到該組主要屬性的功能說明，不再顯示另一種名稱樣式。
- 屬性文件 tooltip 會依目前文件翻譯網域重新組合，不沿用語言載入前或切換語言前的舊快取結果。
- Control 的 `Offset Transform` 分組、子屬性及 8 個相關屬性 tooltip 已補齊台灣繁中翻譯。
- 補齊 `.NET` 編輯器設定、非專有選項、關閉行為及對話框翻譯。
- `Audio Buses` 使用「音訊匯流排」等台灣慣用詞。
- Visual Studio、VS Code、VSCodium、MonoDevelop、Rider、Fleet、SSH、GDScript、Vulkan 等產品名、協定名與程式識別字維持原文。

## 來源與定位

- 自訂倉庫：`https://github.com/UlinRei/Godot-zh-tw.git`
- 上游倉庫：`https://github.com/godotengine/godot.git`
- 上游標籤：`4.7.2-stable`
- 上游提交：`ed1daf0bf001b61586d9930840f2f1394092c079`
- 穩定自訂分支：`main`
- 功能開發分支：依內容建立 `agent/<description>` 分支
- 自訂功能起始提交：`d774cfcfea`
- 目前原始碼：`O:\Github Repositories\GodotSource`
- 可攜式定位：目前專案上層的 `GodotSource` 或 `Godot-zh-tw`，或 `origin` 指向上述自訂倉庫的工作目錄
- 插件翻譯驗證專案：同層的 `godot-card-game`；倉庫為 `https://github.com/UlinRei/godot-card-game.git`
- 部署目錄：`G:\Desktop\Godot`

本機實體路徑只用於記錄目前環境。換裝置時應優先使用相對位置與 Git remote 識別倉庫。

## 建置與部署

目前建置保留 Mono、Vulkan、D3D12、GLES3、ANGLE 與 AccessKit 支援。

```powershell
$pythonExe = 'C:\Users\Ulin\AppData\Local\Programs\Python\Python313\python.exe'
& $pythonExe -m SCons platform=windows target=editor arch=x86_64 module_mono_enabled=yes dev_build=no -j12
& '.\bin\godot.windows.editor.x86_64.mono.console.exe' --headless --generate-mono-glue '.\modules\mono\glue'
& $pythonExe '.\modules\mono\build_scripts\build_assemblies.py' --godot-output-dir '.\bin'
```

Python、SCons、Visual Studio C++、.NET SDK 及 Godot Windows 建置依賴必須先存在。D3D12、ANGLE 與 AccessKit 依賴使用 `misc/scripts/` 的官方安裝腳本取得。

修改 `modules/mono/editor/GodotTools/` 後，必須重新產生 Mono glue 並執行 `build_assemblies.py`。部署時需同步 editor、console 與完整 `bin/GodotSharp`，不可只替換單一執行檔。

完成引擎修改並成功建置後，直接同步至下列既定部署目錄供介面測試；此測試部署不等同提交、推送或發布二進位，不需再次詢問。需要回退時由 Git 與原始碼重建，不另存部署二進位副本。

目前部署檔名：

- `Godot_mono_win64.exe`
- `Godot_mono_win64_console.exe`

## 驗證清單

- SCons Mono Editor 建置成功。
- Console `--version` 回報目前自訂提交版本。
- 部署來源與目標執行檔的 SHA-256 相同。
- 部署後 Console 可用 `--headless --editor --path <project> --quit` 載入 Mono 專案。
- `editor_plugins` 網域的動態 `tr()` 與插件場景翻譯會依 Editor Settings 語言解析。
- 自動送出主視窗關閉要求時，新程序帶有 `--project-manager`。
- 發布前人工確認三種關閉行為、`Ctrl+D` 顯示文字及 Node Inspector 中英對照。

## 維護限制

- 升級 Godot 時，從目標正式標籤重新套用修改並重編，不直接沿用舊二進位。
- 自訂內容只影響編輯器行為與介面，不得讓遊戲內容或執行期程式依賴它。
- 不提交或發布 `bin` 建置產物；發布的是可重建的原始碼與文件。
