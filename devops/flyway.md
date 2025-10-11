# Flyway

## Flywayのソース設定方法

ソースの設定方法は3つ

- `filesystem`
    - rootパスからの任意のファイルパスを指定できる
    - 割と融通がきく
- `classpath`
    - 基本的には`src/main/resource`下になる
    - `test`プロファイル起動なら `src/test/resource`下となる
    - 記述がすっきりするので推奨
- `s3`
    - s3からソースを引っ張ってこれる
    - Javaの場合はbuild.gradleにS3の認証情報が必要
        - AWS SDK S3は必須
        - 設定が面倒
