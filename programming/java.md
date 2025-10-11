# Java

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

## tips

### 定期的なアップグレードは大事

定期的なアップグレードを怠ると差分を追うのが大変になる<br>
10年分のリリースを遡るのはめっちゃ大変<br>
