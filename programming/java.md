# Java

## JDBC

### コネクションプールにおける"普通" (ベストプラクティス？)

- 一般的には最小コネクション数と最大コネクション数を同数に設定する
    - コネクションの作成、破棄にかかるコストを抑制
    - 常に一定のコネクションを確保することでリソース管理を行いやすくする
- コネクションを維持ことによるメモリへの負荷は、事前に見積もりを取ることでデータベースサイズを変更し調整する

コネクション数を一定にすることでリソースの管理が行いやすくなることが最大のメリット<br>
デメリットは見積もりを取ることで補完するとこができる<br>

## SpringBoot

### Annotation 

- @AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
  - 組み込みDBを無効にする
  - ローカルのDBを使用したユニットテストを行う場合は必須の設定
  - 普通はH2やTestContainerなどを利用するので気にする必要はないと思う
  - ymlに記述する場合は以下のように設定する

    ``` YAML
    spring.test.database.replace: none 
    ```

### Migration

- `Spring Boot 1.4.1.RELEASE` to `Spring Boot 1.5.22.RELEASE` のアップグレードで踏んだエラー

  - `@NoArgsConstructor` と `@Data` の併用によるコンストラクタエラー

      ```text
      コンストラクタ SomeClass()はすでにクラス com.example.model.SomeClassで定義されています
      ```

      - 対応方法
          - `@Data` を `@Getter` と `@Setter` に変換
  - パッケージ `@SpringApplicationConfiguration` の削除によるコンパイルエラー
      - 対応方法
          - `@SpringBootTest` を利用する
          - [Qiitaの参考記事](https://qiita.com/andna0410/items/2e48c5c522236a7b11aa#%E3%83%86%E3%82%B9%E3%83%88%E3%82%B3%E3%83%BC%E3%83%89%E3%81%AE%E3%82%B3%E3%83%B3%E3%83%91%E3%82%A4%E3%83%AB%E3%82%A8%E3%83%A9%E3%83%BC)

## tips

### 定期的なアップグレードは大事

定期的なアップグレードを怠ると差分を追うのが大変になる<br>
10年分のリリースを遡るのはめっちゃ大変<br>
