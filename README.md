# StreamHammer

**StreamHammer は、軽量・高信頼の国産イベントログストアです。**  
Kafka や Redpanda のようなストリーム設計思想をベースにしつつ、  
DuckDB をコアに用いて、**小規模・低コストな構成でも使える**ストリームログ基盤を提供します。

## 特徴

- シンプルな HTTP ベースのイベント受信 API（JSON 形式）
- DuckDB による軽量かつ高速な永続化
- 時系列でのリプレイ・検索・統計が可能
- 単体バイナリ or Docker コンテナとして即デプロイ
- 小規模開発、PoC、IoT、教育、バックエンド連携用途に最適

---

## ユースケース例

- Webアプリのユーザーアクションログの蓄積
- IoT センサーイベントの時系列保存
- 非同期通知処理のログ中継基盤として
- バッチシステムとの連携用のイベント受信エンドポイント
- 障害再処理（replay）基盤として

---

## API 仕様（MVP）

### 1. イベント送信

POST /publish
Content-Type: application/json

{
“topic”: “user.signup”,
“data”: {
“user_id”: “abc123”,
“email”: “user@example.com”
}
}

### 2. イベント取得（再送／再処理）

GET /replay?topic=user.signup&since=2024-01-01T00:00:00Z

### 3. 統計取得

GET /stats?topic=user.signup

---

## 内部構成（MVP）

- `Go + Gin` を使用した API サーバー
- `DuckDB` によるファイルベースの高速ログストア
- 保存形式：timestamp, topic, payload(JSON)

---

## インストール・起動方法

```bash
git clone https://github.com/sakurafactory-oss/streamhammer.git
cd streamhammer
go build -o streamhammer cmd/main.go
./streamhammer
```
または Docker 起動（予定）：

docker run -p 8080:8080 sakurafactory/streamhammer



⸻

ライセンス

Apache License 2.0
※ 本プロジェクトは Kafka, Redpanda 等の商標・コードを一切流用していません。

⸻

コントリビューション
	•	Issue、機能提案、コード修正歓迎
	•	特に「使いどころ」の共有や、業務システムとの連携事例は大歓迎です

⸻

将来予定機能
	•	トピックごとのレートリミット
	•	ユーザー単位のオフセット管理
	•	Web UI ベースのイベントリプレイ画面
	•	プロトコル拡張（gRPC / Webhook / MQTT）

⸻

誕生の背景

Kafka は強力ですが、大規模・複雑な構成が求められます。
StreamHammer は「ミニマムなインフラでイベントを記録・可視化・再送できる」ことを目的に設計されました。
個人開発や中小企業、教育機関にもフィットする、国産の小さなメッセージログ基盤を目指します。
