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

## ジョブの並列実行

- GitHub Actionsは同じジョブを複数のプロファイルで実行することができる
- マルチモジュールプロジェクトでテストを並列で実行することもできる
  - これが結構便利
  - ただしGitHubActionsの料金はワークフローの実行時間の総和なので注意

```yaml
jobs:
  test:
    strategy:
      fail-fast: false # いずれかのジョブが失敗しても他のジョブを続行する
      matrix:
        # テスト対象のモジュールをリストで書く
        module: [ 'module-a', 'module-b', 'module-c' ]

    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up JDK 17
        uses: actions/setup-java@v5
        with:
          java-version: '21'
          cache: 'gradle'

      - name: Setup Gradle
        run: gradle/actions/setup-gradle@v5

      - name: Run test for ${{ matrix.module }}
        # Gradleでは、マルチモジュールのタスクは ':モジュール名:タスク名' の形式で指定する
        run: ./gradlew :${{ matrix.module }}:test --no-daemon
```