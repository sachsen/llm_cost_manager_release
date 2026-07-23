# Agent Meter for Claude Code プライバシー情報

Claude Code専用版は、取得した統計を利用者のPC内へ保存するローカルファーストのモニターです。

## 保存する情報

- Claude Codeが報告した入力、出力、キャッシュなどのトークン数
- 使用モデル名、会話コンテキスト、取得状態、最終観測時刻
- 生の絶対パスではなく、ハッシュ化した作業場所と最終フォルダー名

## 保存しない情報

- プロンプト本文、回答本文、ツールの引数や出力
- APIキー、OAuthトークン、Cookie、Claude Codeの認証情報
- CodexまたはGLMのデータ

## 読み取り・通信範囲

- Claude CodeのローカルセッションJSONLから`usage`メタデータだけを読みます。
- Claude Codeのstatus line連携からモデル名やコンテキスト情報を受信します。
- モデル名が`GLM`で始まるセッションは保存せず、集計しません。
- `.codex`、Codexのセッション、Codex認証情報を読みません。
- Z.AI／BigModelの設定・認証値を読まず、それらのAPIへ通信しません。

collectorは`127.0.0.1:4319`だけで待ち受けます。内部取り込みAPIはAgent Meterが生成したBearerトークンで保護されます。統計DBと連携用内部ファイルはWindowsのAgent Meterアプリデータ内にある`claude-only`専用領域へ保存されます。

## 設定変更

初回起動時、Claude Codeのグローバル`SessionStart`フックとstatus lineへAgent Meter専用項目を既存内容とマージし、`CLAUDE.md`へ専用マーカー付き案内を追記します。変更前のファイルは一度だけバックアップします。Codexの設定や`AGENTS.md`は変更しません。

不具合報告へ認証ファイル、トークン、プロンプト、会話ログを添付しないでください。
