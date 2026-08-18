<p align="center">
  <a href="https://godotengine.org">
    <img src="misc/logo/logo_outlined.svg" width="400" alt="Godot Engine 圖示">
  </a>
</p>

# Godot 4.7.1 台灣繁中自訂版

以 [Godot Engine](https://github.com/godotengine/godot) `4.7.1-stable` 為基礎的個人化 Mono Editor，補強台灣繁體中文介面、插件翻譯支援與日常編輯流程。

> [!IMPORTANT]
>
> 這是非官方自訂版本，不代表 Godot Foundation 或 Godot 官方專案。一般使用、官方下載、文件與問題回報，請以 [Godot 官方網站](https://godotengine.org)及[官方倉庫](https://github.com/godotengine/godot)為準。

## 版本資訊

- 上游版本：Godot `4.7.1-stable`
- 編輯器類型：Mono / .NET Editor
- 介面語系：台灣繁體中文（`zh_TW`）
- 穩定自訂分支：`main`
- 功能開發分支：依內容建立 `agent/<description>` 分支
- 自訂倉庫：[UlinRei/Godot-zh-tw](https://github.com/UlinRei/Godot-zh-tw)

## 自訂內容

- 關閉編輯器時可選擇退出、返回專案管理員，或每次詢問；預設為返回專案管理員。
- EditorPlugin 使用獨立的 `editor_plugins` 翻譯網域，可依編輯器語言載入專案內的插件翻譯，而不改變遊戲語系。
- 補充台灣繁中介面翻譯，並保留產品名、協定名及程式識別字原文。
- Node 通用 Inspector 欄位採「中文（English）」格式，方便對照英文教學與 API 文件。
- `Ctrl+D` 的 `Duplicate` 顯示為「創建副本」，實際快捷鍵與建立副本功能維持不變。
- 補齊 .NET 編輯器設定、關閉行為、對話框與音訊匯流排等常用介面用語。

完整的實作位置、建置方式、部署方法與驗證清單請見 [GodotCustomBuild.md](GodotCustomBuild.md)。代理與協作規則請見 [AGENTS.md](AGENTS.md)。

## 取得與切換分支

```powershell
git clone https://github.com/UlinRei/Godot-zh-tw.git
cd Godot-zh-tw
```

`main` 是可直接建置的自訂版本；功能修改會先放在 `agent/<description>` 獨立分支，再整合回 `main`。此 Fork 的官方上游為 `https://github.com/godotengine/godot.git`，不會修改官方 Godot 倉庫或其分支。

## Windows Mono Editor 建置

請先安裝 Godot 官方文件列出的 Windows 編譯依賴、Python、SCons、Visual Studio C++ 與 .NET SDK，再於倉庫根目錄執行：

```powershell
python -m SCons platform=windows target=editor arch=x86_64 module_mono_enabled=yes dev_build=no -j12
```

主要輸出檔位於：

- `bin/godot.windows.editor.x86_64.mono.exe`
- `bin/godot.windows.editor.x86_64.mono.console.exe`

若修改 `modules/mono/editor/GodotTools/`，還需要重新產生 Mono glue 並建置託管組件；詳細命令請見 [GodotCustomBuild.md](GodotCustomBuild.md)。

其他平台及完整依賴說明請參閱 [Godot 官方編譯文件](https://docs.godotengine.org/en/latest/engine_details/development/compiling/)。本倉庫不提交 `bin` 內的建置產物。

## 插件翻譯驗證專案

[UlinRei/godot-card-game](https://github.com/UlinRei/godot-card-game) 收錄 AT Icons、GoTree、Godot State Charts、Phantom Camera 與 Rider Plugin 的台灣繁中翻譯，可用來驗證 `editor_plugins` 翻譯網域。

翻譯依照各插件功能與官方說明調整，不採逐字直譯。專案內的預覽圖片、網頁摘錄、Godot 快取與插件暫存檔不會同步到 GitHub。

## 與官方上游同步

```powershell
git fetch upstream
git switch main
git rebase upstream/master
```

升級 Godot 版本時，建議從目標正式標籤重新檢查並套用自訂修改，完成建置與介面驗證後再發布，不直接沿用舊版二進位檔。

## 授權

Godot Engine 及本倉庫中的修改沿用 [MIT License](LICENSE.txt)。Godot 名稱與圖示等商標權利仍歸其各自權利人所有；本自訂版本不代表官方認可或背書。
