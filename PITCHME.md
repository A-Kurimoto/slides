## とりあえず自己紹介

### [栗本 篤臣](https://www.wantedly.com/projects/26782/staffings/106526)

* 基本エンジニアです。
* 山中を徹夜で二晩くらい走り続ける競技が大好きです。
* ランニング関係のサービス創るお仕事してます。

<img width="200" alt="スクリーンショット 2017-07-17 7.13.12.png" src="https://qiita-image-store.s3.amazonaws.com/0/80905/dc2a622a-51b9-0755-8e71-212310b6fdc2.png">

---

# ランニング関係のサービス？

---

## [TATTA]([https://runnet.jp/smpapp/tatta/)

- マラソン大会にエントリーした人たちが当日までの練習を競い合うアプリ

<img width="1005" alt="スクリーンショット 2017-07-17 7.04.38.png" src="https://qiita-image-store.s3.amazonaws.com/0/80905/a2341d40-2c21-a2ad-608b-6327b9319bd9.png">


※これはサーバレスじゃないただのCM

---

# サーバレスなランニング関係のサービス

---

## [ランフォト＋](https://runphoto-plus.runnet.jp/)

- マラソン大会でカメラマンが撮影した写真を、ゴールしたらすぐ完走証にして印刷するサービス

<img width="940" alt="スクリーンショット 2017-07-17 7.10.54.png" src="https://qiita-image-store.s3.amazonaws.com/0/80905/954ec51c-e4b4-59c8-685e-4219d04bec7d.png">

---

## [マラソンLINE bot](https://fujisan-marathon.jp/line/)

- 選手への応援、写真など、大会に関する情報をLINE botで通知するサービス

<img width="661" alt="スクリーンショット 2017-07-17 7.12.20.png" src="https://qiita-image-store.s3.amazonaws.com/0/80905/0f411242-c2ab-3116-739c-985c9785e4e2.png">

---

## [応援navi](https://runnet.jp/smpapp/ouennavi/)

- マラソン大会に参加してる家族や友人を応援するアプリ

<img width="1000" alt="スクリーンショット 2017-07-17 7.05.24.png" src="https://qiita-image-store.s3.amazonaws.com/0/80905/c01d4f0b-4b23-abac-3cbe-15e4c18447b6.png">

---

# 今日のトピックス

* Road to server-less
* 関連AWSサービス概要紹介

---

# Road to server-less

---

# 応援navi導入大会（2016年実績）

勝田全国マラソン
いわきサンシャインマラソン
北九州マラソン
高知龍馬マラソン
熊本城マラソン
東京マラソン
静岡マラソン
鹿児島マラソン
かつしかふれあいRUNフェスタ
マラソンフェスティバルナゴヤ・愛知（名古屋ウィメンズマラソン）
古河はなももマラソン
横浜マラソン
板橋Cityマラソン
練馬こぶしハーフマラソン
さが桜マラソン
仙台国際ハーフマラソン
柴又100K
サロマ湖100kmウルトラマラソン
札幌マラソン
東京30K秋大会・冬大会
水戸黄門漫遊マラソン
手賀沼エコマラソン
ぐんまマラソン
下関海響マラソン
富士山マラソン
沖縄100Kウルトラマラソン

http://runners.co.jp/results/

---

## サービス特性

* 大会ある日はドカンとアクセス、大会無い日はほとんど無風、、
* 大会毎にアクセス数が全く異なる
<img width="1238" alt="スクリーンショット 2017-07-20 7.53.44.png" src="https://qiita-image-store.s3.amazonaws.com/0/80905/a87d394b-b9e7-25b7-dd8b-115a4dcb06d4.png">

* 大会ある日は休日なので、トラブル発生すると当日中の復旧が難しい、けどSLAのレベルは非常に高い

---

## EC2時代

* 休日出勤して監視
* 改修毎にがっつり負荷試験
* 常時稼働でもないのにこの値段、、、

<img width="1177" alt="スクリーンショット 2017-07-20 7.28.16.png" src="https://qiita-image-store.s3.amazonaws.com/0/80905/306f928d-ec49-503f-14c0-acb766a0ea8f.png">

---

# つらい

---

# そうだ、サーバレスにしよう！

---

##  既存環境からの切り替え

* 元々の構成はEC2 + CloudSearch + DynamoDBのWebアプリ
* Jenkins + GitHub Flow + apexでデプロイ自動化
* 本番は東京、開発はバージニアで同一の環境を製作
* API GatewayとLambdaで処理をAPI化。静的ファイルはS3へ
* APIはAndroid, iOS版と共有

---

## 安定運用までの道程

* CloudWatchでアラート設定
* DynamoDBにAutoScale設定（昔はDynamicDynamoDB）
* リクエストの実データから各リソースにかかる負荷の最大瞬間風速を予測
* スパイクに備えてマラソン大会の知名度や前年度実績に合わせてCloudWatchのルールで各種リソースを増強するツール作成
* 念のため[Goad](https://github.com/goadapp/goad)で負荷試験（Goad自体はforkしてカスタマイズ）
* Lambda制限緩和申請

---

## 結果

* 運用は事前増強ツールの設定のみ！
* ３サービスと開発環境合わせてこのお値段！

<img width="1189" alt="スクリーンショット 2017-07-21 1.13.29.png" src="https://qiita-image-store.s3.amazonaws.com/0/80905/aa7831a4-6318-ab7a-65fc-9e63c9674ac6.png">

---

## 更なる低コスト化を求めて

* EC2で動作していた管理コンソールもS3+API Gateway+Lambdaに
* CloudSearchが要件のシンプルさに比べて重厚なので、無理やりDynamoDB化（ただしデータ件数は増える）

| partition key | sort key |
|:-----------|:------------|
| {Group ID} | {Individual ID} |
| {Group ID} | 検索語句A: {Individual ID} |
| {Group ID} | 検索語句B: {Individual ID} |

* 多分これであと月額$300下げれる＾＾

---

## その他サーバレスにして良かったこと

* ステートレスの強制が高難易度のバグを産まない
* 自然と疎結合なアーキテクチャになり、他サービスへの拡張が容易になった（非同期イベントトリガー）

---

## モノリシックでは起きなかった課題

* 複数のリポジトリが共通のリソースを利用している状況でどのように構成を可視化すべきか
* 複数のリポジトリで同じようなコードを書かざるを得ないことへの対処
* 「要件とデータ量によって採用するサービス、構成を設計する」というスキルセットが重要に

---

## これからサーバレスを始める方に
* サーバレスアーキテクチャはとても素敵
* ただしEC2-lessにしてLambdaを使えば良い、という訳でもない
* 様々なマネージドサービスを状況に応じて適切に組み合わせ、活用するスキルが重要
* 「◯◯専用サービス」があり、要件に一致していると使いたくなるが、無駄にアーキテクチャを複雑化させてしまうかもしれない
* 「それ本当にCloudSearch使う必要あった？」「Kinesis要る？」「SQS挟まなくても対応できるんじゃない？」
* **コードと同じく、できるだけシンプルに**

---

# Any Questions?

---

# 関連AWSサービス概要紹介

---

* Cognito
* API Gateway
* Lambda
* AWS Batch
* DynamoDB
* S3

---

## Cognito

### 使ったところ

* クライアントからAPI Gateway、S3直アクセス等の認証に
* SNSログインからのAWS Credential発行
* 管理コンソールにUser Pool

---

## Cognito

### 使ってて迷ったこと

* Cognito Syncの適切な使い所
本人だけ知っていればよく（バックエンドは知る必要がない）、端末を跨いで共有したい情報の保存のみ。
便利そうでいて、DynamoDB使ってれば不要な感も、、

* 別サービス間での共通認証基盤にできるか
サービスの種類・性格によるが、SNSとの紐付けを複数できない（FacebookとTwitterはできるが、Facebookアプリ2つは無理）のがネック。

---

## API Gateway

### どんなものか

* マネージドなwebサーバ
* API別に使用量制限かけれる
* APIとして容易に外部公開できる
* Swaggerを使ってas a codeで仕様書が書ける

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

---

## API Gateway

### 気になってるとこ

* Lambda側にも実装が影響する（プロキシ統合の使用時に顕著）
* たまに「あれ？直接Lambda呼び出した方がTCO低かった？」って思う（2-Tier Architecture vs 3-Tier Architecture）
* 意外と高い(弊社環境ではLambdaより少しお高め)

---

## Lambda

### 素晴らしいところ

* 全部！（特にシンプルさ）

---

## Lambda

### もっとこうなったらいいのに

* CloudFrontみたいにグローバル単位で動いて冗長性を担保できないか（たまに発生するAWS大規模障害が怖い）
* コンテナの起動時間がもっと短くなれば、、（リスク承知でコンテナ上にキャッシュ持たせてる）
* 関数間の共通ライブラリできないものか（リポジトリに共通のlibディレクトリ作ってシンボリックリンク貼って対応中。でもリポジトリ跨ると対応できない）
* デプロイメント制限も緩和申請したい
* 稼働インスタンスを自分で制御したい
* ていうかDocker動いて欲しい（AWS Batch使え）

---

## AWS Batch

* Lambda上では動かない処理（インスタンス由来、デプロイメント制限由来等）や、タイムアウトしてしまう処理にはこれ
* キューされたリクエストを自作のDocker　imageで動かせる
* バックエンドでEC2をマネージドで利用

---

## DynamoDB

### 気に入ってるとこ

* いくらでも増強できる
* 速い
* SQLなど要らぬ
* 最近リリースされたAutoScale最高
* Stream便利だ、、、
* 普段凪いでるサービスだと格安

---

## DynamoDB

### 大事なこと

* 設計に対するキャッチアップコストを引き受ける気持ち
* プロビジョニングまで全て頭に入れながら開発できる見通す目
* 「RDBだったら一瞬なのに、、」という気持ちと戦う気概
* 諦めも肝心

---

## S3

### 素敵な点

* トリガーの応用力
* サーバレスWebアプリには必須
* CloudFront、Route53、ACMでSSLもばっちり
* 要件によってはDB代わりにも（遅いけど安いし）

---

## S3

### 気をつけた方が良い点

* 結果整合性（i.e. そのうち更新されるよ！）

---

# Any Questions?

---

# Thank you!
