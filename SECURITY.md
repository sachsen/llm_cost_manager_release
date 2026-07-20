# Security Policy

## 不具合を報告する前に

公開IssueへAPIキー、OAuthトークン、Cookie、認証ファイル、プロンプト、会話ログを貼り付けないでください。

通常の不具合は、OS、Agent Meterのバージョン、診断欄の状態名とエラーコード、秘密情報を除去した再現手順を添えてIssueへ報告してください。

## セキュリティ上の問題

認証情報の露出や任意コード実行など、公開前の調整が必要な問題は公開Issueへ詳細を書かないでください。GitHubのPrivate vulnerability reportingが利用できる場合は、リポジトリのSecurityタブから非公開で報告してください。

## 配布物の確認

ReleaseのZIPと`.sha256`ファイルを同時に取得し、実行前にSHA256が一致することを確認してください。現在のWindowsバイナリにはコード署名がありません。
