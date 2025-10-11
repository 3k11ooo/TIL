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

## Workflow チェックリスト

- なるべくサードパーティActionは使わずにMakefileなどにまとめる
    - ベースとなる実行可能バイナリをラップして提供されているActionsを利用する際は、実現したいワークフローがラップされたActionsに沿っているのかを検討し、できるならペースとなる実行可能バイナリを直接実行する、もしくは`make`経由で実行するのが良いらしい
    - ローカル環境でも実行できるのでGitHub Actionsへの依存が減って良さそう
    - `make`にまとめる手間はかかるのでラップされたActionsを把握する手間と天秤になるかも？
- 大掛かりなカスタマイズをするならActions自作でも良いかも
