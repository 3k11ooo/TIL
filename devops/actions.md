# GitHub Actions

## PostgreSQLコンテナの作成

- envにPOSTGRES_hogeを設定すればいい感じコンテナとDBを作成してくれる
- ただし、スキーマは自分で作成しないといけないので注意が必要

    ``` YAML
    services:
      postgres:
        image: postgres:17
        env:
          POSTGRES_USER: user
          POSTGRES_PASSWORD: password
          POSTGRES_DB: testdb
        ports:
          - 5432:5432
    ```
  
## ワークフローの重複実行を制御する

- 同じワークフローが起動していたら古い方をキャンセルしてくれるらしい

    ```
     concurrency:
       group: ${{ github.workflow }}-${{ github.ref }}
       cancel-in-progress: true
    ```
