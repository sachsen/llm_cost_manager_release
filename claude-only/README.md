# Agent Meter for Claude Code

Claude Codeの利用状況だけをPC内で確認する、Windows向けのClaude Code専用版です。

この版にはCodexとGLMの表示・収集・連携機能がありません。Codexの設定やローカル履歴を読み書きせず、Z.AI／BigModelのAPIへ通信しません。Claude CodeをGLM互換モデルで使用したセッションも集計対象外です。

## ダウンロード

1. [Releases](https://github.com/sachsen/llm_cost_manager_release/releases/latest)を開きます。
2. `agent-meter-claude-only-windows-x64-<version>.zip`と同名の`.sha256`をダウンロードします。
3. SHA256を照合し、ZIPを専用フォルダーへ展開します。

```powershell
(Get-FileHash .\agent-meter-claude-only-windows-x64-0.1.0.zip -Algorithm SHA256).Hash
Get-Content .\agent-meter-claude-only-windows-x64-0.1.0.zip.sha256
```

ハッシュが一致しない場合は実行しないでください。Windows 10／11 x64とMicrosoft Edge WebView2 Runtimeが必要です。管理者権限やインストーラーは不要です。現在の配布バイナリにはコード署名がありません。

## インストール

1. フル版Agent Meterを使用中なら、トレイメニューから終了します。
2. ZIPを任意の専用フォルダーへ展開します。
3. `agent-meter-desktop.exe`と`agent-meter-hook-helper.exe`を同じフォルダーに置いたまま、`agent-meter-desktop.exe`を起動します。
4. Claude Codeで新しいセッションを開始します。

初回起動時に変更するのはClaude Codeのグローバル設定だけです。既存の`SessionStart`フックとstatus lineを保持しながらAgent Meter連携を追加し、`CLAUDE.md`へ専用マーカー付き案内を追記します。変更前のファイルは一度だけバックアップします。`.codex`、Codexのフック、`AGENTS.md`には触れません。

以後はPC再起動後でも、Claude Codeのセッションを新規作成・再開するとタスクトレイへ自動起動します。フル版と専用版は同じClaude Code連携設定を使うため、同時利用せず、どちらか一方を選んでください。

## 使い方

- 画面は15秒ごとに更新されます。
- Claude CodeのローカルセッションJSONLから、`usage`メタデータだけを約60秒間隔で集計します。
- Claude Code status lineから、モデル名や会話コンテキストを受信します。
- ウィンドウを閉じてもトレイで監視を続けます。完全終了はトレイメニューの「終了」です。
- エージェントまたはPowerShellから現在値をJSONで取得するには、展開フォルダーで次を実行します。

```powershell
.\agent-meter-hook-helper.exe --status
```

専用版は`127.0.0.1:4319`だけで待ち受け、専用のデータ領域へ保存します。

## この版にないもの

| 対象 | 専用版の動作 |
|---|---|
| Codex | API通信、ローカル履歴読取、フック設定、画面表示のすべてなし |
| GLM | Z.AI／BigModel通信、認証値読取、履歴集計、画面表示のすべてなし |
| GitHub Copilot | 検出・表示なし |
| Claude Code上のGLM互換モデル | モデル名が`GLM`で始まるセッションを無視 |

## 更新とアンインストール

更新時はトレイから終了し、新しいZIPを別フォルダーへ展開して起動します。

アンインストール前の確認:

```powershell
powershell -ExecutionPolicy Bypass -File .\uninstall-agent-meter.ps1 -WhatIf
```

データを含めてアンインストール:

```powershell
powershell -ExecutionPolicy Bypass -File .\uninstall-agent-meter.ps1
```

統計を残す場合は`-KeepData`を付けます。スクリプトはClaude CodeのAgent Meter専用項目と専用データだけを対象にし、Codex設定には触れません。

## プライバシー

プロンプト本文、回答本文、ツール引数・出力、APIキー、OAuthトークン、Cookieは保存しません。詳細は[PRIVACY.md](PRIVACY.md)と、ZIP内の`distribution-guide.html`を参照してください。
