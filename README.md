# Agent Meter

Agent Meterは、Codex、Claude Code／GLMなどの利用状況をPC内で確認するWindows向けローカル監視アプリです。

このリポジトリは、利用者向けのバイナリ配布とドキュメントだけを公開するためのリポジトリです。開発用ソースコード、テストデータ、開発履歴は含みません。

## ダウンロード

1. [Releases](https://github.com/sachsen/llm_cost_manager_release/releases/latest)を開きます。
2. `agent-meter-windows-x64-<version>.zip`と、同名の`.sha256`ファイルをダウンロードします。
3. SHA256を照合してからZIPを展開します。

PowerShellでの確認例です。

```powershell
(Get-FileHash .\agent-meter-windows-x64-0.1.0.zip -Algorithm SHA256).Hash
Get-Content .\agent-meter-windows-x64-0.1.0.zip.sha256
```

2つのハッシュが一致しない場合は実行しないでください。

## 動作環境

- Windows 10／11 x64
- Microsoft Edge WebView2 Runtime
- 利用状況を取得したいCodexまたはClaude Code
- 契約枠の取得時は各提供元へのインターネット接続

管理者権限やインストーラーは不要です。現在の配布バイナリにはコード署名がありません。Windowsの警告が表示された場合は、入手元とSHA256を確認できない限り実行しないでください。

## インストール

1. ZIPを任意の専用フォルダーへ展開します。
2. `agent-meter-desktop.exe`と`agent-meter-hook-helper.exe`を同じフォルダーに置いたままにします。
3. `agent-meter-desktop.exe`を起動します。
4. Codexでは一度`/hooks`を開き、Agent Meterの`SessionStart`フックを確認して信頼します。

初回起動時、Agent MeterはCodexとClaude Codeのグローバル設定へ専用の`SessionStart`フックを既存項目とマージします。また、グローバル`AGENTS.md`／`CLAUDE.md`へ専用マーカー付きの案内を追記します。既存のフックや案内本文は保持され、変更前のファイルは一度だけバックアップされます。

以後はPC再起動後でも、CodexまたはClaude Codeのセッションを新規作成・再開するとAgent Meterがタスクトレイへ自動起動します。

## 使い方

- 画面は15秒ごとに更新されます。
- ウィンドウを閉じてもタスクトレイで監視を続けます。
- 再表示はトレイアイコンの「Agent Meterを開く」です。
- 完全終了はトレイアイコンの「終了」です。
- 「今すぐ取得」はCodexの契約枠を再取得します。
- 表示される割合は「使用済み」です。使用済み25%なら残りは約75%です。

エージェントまたはPowerShellからJSONで現在値を取得する場合は、展開フォルダーで次を実行します。

```powershell
.\agent-meter-hook-helper.exe --status
```

停止中の場合はバックグラウンド起動してから、ダッシュボードと診断を返します。

## 対応範囲

| 対象 | 主な表示内容 | 備考 |
|---|---|---|
| Codex | 契約枠、ローカルトークン統計 | 初回は過去履歴を集計するため値が大きくなる場合があります |
| Claude Code | モデル、コンテキスト、ローカルトークン統計 | 本文やツール内容は保存しません |
| GLM Coding Plan | 5時間枠、MCP月間枠、モデル別統計 | Claude CodeでZ.AI／BigModel接続を使用している場合に取得します |
| GitHub Copilot | 対応IDEの拡張導入検知 | 個人向けリアルタイム残量は取得しません |

提供元が公開していない値は、推測値を実測値として表示せず「未報告」と表示します。

## 更新

1. トレイメニューからAgent Meterを終了します。
2. 新しいZIPを別フォルダーへ展開します。
3. 新しい`agent-meter-desktop.exe`を起動します。

統計データはWindowsのアプリデータ側に保存されるため、通常は引き継がれます。Agent Meter専用フックの実行パスは新しいフォルダーへ更新されます。

## アンインストール

展開フォルダーで次を実行します。

```powershell
powershell -ExecutionPolicy Bypass -File .\uninstall-agent-meter.ps1
```

統計データを残す場合:

```powershell
powershell -ExecutionPolicy Bypass -File .\uninstall-agent-meter.ps1 -KeepData
```

実行前に変更対象だけを確認する場合:

```powershell
powershell -ExecutionPolicy Bypass -File .\uninstall-agent-meter.ps1 -WhatIf
```

アンインストーラーはAgent Meter専用のフックと案内だけを除去し、他の設定を保持します。処理後、展開したフォルダーを削除してください。

## プライバシー

プロンプト本文、回答本文、ツール引数・出力、APIキー、OAuthトークン、Cookieは保存しません。保存範囲と設定変更の詳細は[PRIVACY.md](PRIVACY.md)を参照してください。

## 詳細ガイド

ZIP内の`distribution-guide.html`をブラウザーで開いてください。トラブルシューティングを含む詳細な利用方法をオフラインで確認できます。

## 公開範囲とライセンス

この公開リポジトリには配布バイナリと利用者向け文書だけを掲載します。Agent Meterの配布物には[MIT License](LICENSE)を適用します。
