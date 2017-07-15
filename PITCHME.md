
### スパイクするタイプのサービスを作って勉強になったこと

2017/7/21

---

### とりあえず自己紹介

#### 栗本 篤臣

* 山中を徹夜で二晩くらい走り続ける競技が大好きです。
* ランニング関係のサービス創るお仕事してます。
* 基本エンジニアです。

https://www.facebook.com/kurimoto.atsuo

---

#### ランニング関係のサービス？

##### [TATTA]([https://runnet.jp/smpapp/tatta/)

- マラソン大会にエントリーした人たちが当日までの練習を競い合うアプリ

※これはサーバレスじゃないただのCM


---

### サーバレスなランニング関係のサービス

* マラソン大会に参加してる家族や友人を応援するアプリ

https://runnet.jp/smpapp/ouennavi/

* マラソン大会でカメラマンが撮影した写真を、ゴールしたらすぐ完走証にして印刷するサービス

https://runphoto-plus.runnet.jp/

+++

* 応援や写真など、大会に関する情報をLINE botで通知するサービス

https://fujisan-marathon.jp/line/

---

### サービス特性

* 大会ある日はドカンとアクセス
* 大会無い日はほとんど凪いでる、、
* 大会ある日は休日なので、トラブル発生すると当日中の復旧が難しい。SLAのレベルめっさ高い

---

### EC2で作ってた時代

* 世界中から何百台とサーバ集める羽目になったり（アプリの作りの問題だけど）
* オートスケール全然間に合わなくて休日出勤して監視したり
* 改修する度に高コストな負荷試験したり

---

# つらい

---

# そうだ、サーバレスにしよう！

---

### 今日のトピックス

* API Gateway + Lambda + DynamoDB基本セット
* プロビジョニングと負荷試験
* 更なる低コスト化を求めて
* 非同期イベントを使いこなそう
* デプロイ周りどうしてる？
* コードの共通化

---

## API Gateway + Lambda + DynamoDB基本セット

+++

## API Gateway

+++

### 気に入ってるとこ

* APIとして容易に外部公開できる
* API別に使用量制限かけれる
* APIキー便利
* Swaggerを使って　As a codeで仕様書が書ける

+++

```yaml:sample.yaml
  /v1/some-resources:
    get:
      parameters:
        - name: "param1"
          in: "query"
          required: true
          type: "string"
      responses:
        200:
          description: "200 response"
          schema:
            $ref: '#/definitions/SomeResponse'
      security:
      - api_key: []
```

+++

### 気になってるとこ

* Lambda側にも実装が影響する（プロキシ統合の使用時に顕著）
* たまに「あれ？直接Lambda呼び出しした方が　TCO低かった？」って思う
* 意外と高い(弊社環境ではLambdaより少しお高め)

+++

## Lambda

+++

### 気に入ってるとこ

* 全部

+++

### もっとこうなったらいいのに

* デプロイメント制限も緩和申請したい
* 稼働インスタンスを自分で制御したい
* ていうか自作Dockerで動いて欲しい

+++

## DynamoDB

+++

### 気に入ってるとこ

+++

### 気になってるとこ

+++

---

# Tips

+++

* ステートフルなLambda（ぇ
* Lambdaでデプロイ制限に引っかかった場合

---

### Thank you!
