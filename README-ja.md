# consultation-record

consultation-record は、開発チームの組織課題を分析・記録するための POS (problem oriented system: 問題志向型システム) アプリケーションです。

観察された事実、問題、仮説、介入、フォローアップ結果を構造化して残し、組織課題に対する診断と思考の過程を後から見直せるようにすることを目的としています。

English README: [README.md](README.md)

## 現在の状態

このリポジトリは初期設計段階です。依存関係、初期ドメイン設計、PostgreSQL向けのテーブル定義案は追加済みですが、最初のアプリケーション縦切りはまだ実装していません。

## 想定スタック

- Java 25
- Spring Boot 4
- Thymeleaf
- Spring Security
- PostgreSQL 18
- TestcontainersによるPostgreSQL 18テスト
- Gradle

永続化マッピング方式はまだ検討中です。

- MyBatis: 明示的なSQL、時系列・分析・レポート系クエリに向いています。
- JPA: 初期のCRUD中心の縦切りや、素直な親子関係を持つ集約に向いています。

## コアドメイン

アプリケーションは次の概念を中心に設計します。

- Team: 分析対象となる開発チームまたは組織単位
- Consultation Case: あるチームに対する相談・分析の作業単位
- Problem: ケース内で扱う問題志向の記録
- Observation: 問題に紐づく事実ベースの観察記録
- Assessment: 問題に対する仮説や解釈
- Intervention: 問題を改善するための行動や実験
- Follow-up: 介入後の結果確認と次アクション

## ドキュメント

- [初期設計](docs/initial-design.md)
- [データベース設計メモ](docs/database-schema.md)
- [PostgreSQL DDL案](docs/database-schema.sql)

## データベース

現在のスキーマは PostgreSQL 18 を対象にしています。

初期テーブル:

- `app_user`
- `team`
- `consultation_case`
- `problem`
- `observation`
- `assessment`
- `intervention`
- `follow_up`

DDL案では、identity主キー、`CHECK`制約つきの文字列ステータス、監査タイムスタンプ、外部キーごとの削除ルールを使っています。

## テスト実行

Gradle wrapperを使います。

```sh
./gradlew test
```

テストはTestcontainersを使うため、Dockerが必要です。

macOS + Rancher Desktop のこのローカル環境では、次の環境変数が必要になる場合があります。

```sh
DOCKER_HOST=unix:///Users/ssobue/.rd/docker.sock \
TESTCONTAINERS_DOCKER_SOCKET_OVERRIDE=/var/run/docker.sock \
TESTCONTAINERS_RYUK_DISABLED=true \
./gradlew test
```

## 最初の実装マイルストーン

最初の有用な縦切りは次の範囲を想定しています。

1. チーム登録
2. 相談ケース登録
3. 問題登録
4. 問題詳細画面
5. 観察記録登録

この範囲で、ルーティング、セキュリティ、DBアクセス、バリデーション、テンプレート、コアドメインの言葉を確認します。その後、Assessment、Intervention、Follow-upへ広げます。
