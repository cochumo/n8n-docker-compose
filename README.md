# n8n + Mattermost Docker Compose

このプロジェクトは、n8n（ワークフロー自動化ツール）とMattermost（チームコラボレーションプラットフォーム）をDocker Composeで簡単に構築できる環境を提供します。

## 🚀 機能

- **n8n**: ワークフロー自動化プラットフォーム（ポート: 5678）
- **Mattermost**: チームコラボレーションプラットフォーム（ポート: 8065）
- **PostgreSQL**: データベース（Mattermost用）
- 完全なDocker Compose環境
- ヘルスチェック機能
- データ永続化

## 📋 前提条件

- Docker
- Docker Compose
- 4GB以上のRAM（推奨）

## 🛠️ セットアップ

### 1. リポジトリのクローン

```bash
git clone <repository-url>
cd n8n-docker-compose
```

### 2. 環境変数の設定（オプション）

```bash
cp .sample.env .env
```

`.env`ファイルを編集して、必要に応じて設定を変更してください：

```bash
# n8n Environment Variables

# IMPORTANT: Replace this with a secure random key (e.g., openssl rand -hex 32)
N8N_ENCRYPTION_KEY=your-secure-encryption-key

# Optional: Set the timezone (e.g., Europe/Berlin, America/New_York)
TZ=Asia/Tokyo
```

### 3. サービスの起動

```bash
docker-compose up -d
```

### 4. サービスの確認

```bash
docker-compose ps
```

## 🌐 アクセス方法

### n8n
- **URL**: http://localhost:5678
- **初回アクセス**: 管理者アカウントを作成してください

### Mattermost
- **URL**: http://localhost:8065
- **初回アクセス**: 管理者アカウントを作成してください

## 📁 データ永続化

以下のデータは永続化されます：

- **PostgreSQL**: `pg_data` ボリューム
- **Mattermost**: 
  - `mm_config` - 設定ファイル
  - `mm_data` - アップロードファイル
  - `mm_logs` - ログファイル
  - `mm_plugins` - プラグイン
  - `mm_client` - クライアントプラグイン
- **n8n**: `n8n_data` ボリューム

## 🔧 管理コマンド

### サービスの停止
```bash
docker-compose down
```

### ログの確認
```bash
# 全サービスのログ
docker-compose logs

# 特定サービスのログ
docker-compose logs n8n
docker-compose logs mattermost
```

### サービスの再起動
```bash
docker-compose restart
```

### データベースのバックアップ
```bash
docker-compose exec postgres pg_dump -U mmuser mattermost > backup.sql
```

### データベースの復元
```bash
docker-compose exec -T postgres psql -U mmuser mattermost < backup.sql
```

## 🔒 セキュリティ

### 本番環境での使用

本番環境で使用する場合は、以下の設定を変更してください：

1. **パスワードの変更**: `docker-compose.yml`内の`POSTGRES_PASSWORD`を強力なパスワードに変更
2. **暗号化キーの設定**: `.env`ファイルで`N8N_ENCRYPTION_KEY`を設定
3. **HTTPSの設定**: リバースプロキシ（nginx等）を使用してHTTPSを有効化
4. **ファイアウォールの設定**: 必要に応じてポートを制限

### 環境変数の例

```yaml
# docker-compose.yml の postgres サービス
environment:
  POSTGRES_PASSWORD: your-secure-password

# .env ファイル
N8N_ENCRYPTION_KEY=your-32-character-encryption-key
TZ=Asia/Tokyo
```

## 🐛 トラブルシューティング

### よくある問題

1. **ポートが既に使用されている**
   ```bash
   # 使用中のポートを確認
   lsof -i :5678
   lsof -i :8065
   ```

2. **サービスが起動しない**
   ```bash
   # ログを確認
   docker-compose logs
   
   # ヘルスチェックの確認
   docker-compose ps
   ```

3. **データベース接続エラー**
   ```bash
   # PostgreSQLの状態確認
   docker-compose exec postgres pg_isready -U mmuser -d mattermost
   ```

### ログの確認

```bash
# リアルタイムログ
docker-compose logs -f

# 特定サービスのログ
docker-compose logs -f n8n
docker-compose logs -f mattermost
```

## 📚 参考リンク

- [n8n公式ドキュメント](https://docs.n8n.io/)
- [Mattermost公式ドキュメント](https://docs.mattermost.com/)
- [Docker Compose公式ドキュメント](https://docs.docker.com/compose/)

## 🤝 貢献

プルリクエストやイシューの報告を歓迎します。

## 📄 ライセンス

このプロジェクトはMITライセンスの下で公開されています。詳細は[LICENSE](LICENSE)ファイルを参照してください。

## ⚠️ 注意事項

- このセットアップは開発・テスト環境向けです
- 本番環境では適切なセキュリティ設定を行ってください
- 定期的なバックアップを推奨します
