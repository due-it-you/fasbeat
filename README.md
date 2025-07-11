# fasbeat
## ■サービス概要
楽曲の音声と背景素材を投じるだけで、各SNSに調整した動画生成・予約投稿を半自動的に行ってくれるサービスです。


## ■ このサービスへの思い・作りたい理由
自身がビートメイカーとして収益を上げていた経験の中で収益の大部分の経路として、各SNSでビートを投稿することで顧客を流入させてそこから気に入ってもらったら購入してもらうというルートが存在しています。
しかし、ビートは頻繁に新しいものを作り続けるため、その度に毎回違うSNSを開いての投稿の繰り返し作業が強いフラストレーションになっていました。
そこで、単に決まったテンプレート通りの投稿作業に時間を取られたくないという想いから、このサービスを通して課題を解決することに決めました。

## ■ ユーザー層について
ユーザー層は、hiphopのビートを販売しているビートメイカーを対象にしています。
SNSやYoutubeを活用して自身の楽曲を販売するのはhiphop以外にもありますが、専らその投稿のほとんどがhiphopというジャンルに偏重していることと、自身が実際にビートメイカーとして収益を上げていたこともあり一番よく知っている分野なので、ビートメイカーに特化して価値を提供するように決めました。

## ■サービスの利用イメージ
ユーザーは自身が制作したビートの楽曲ファイルと背景画像を用意してドラッグ&ドロップするだけで、あとはアプリ側が動画生成, 各SNSへの予約投稿まで行ってくれます。
これによって、ユーザーはビートの動画作成と投稿作業の時間が削減され、その分の時間を自身の楽曲制作や他のマーケティング活動に勤しむことが出来るようになります。


## ■ ユーザーの獲得について
実際に同じようなフラストレーションを感じている人がいるかredditで調べてみたところ、そういったことを言っているユーザー達が同じように存在したので、redditなどから認知を広げていこうと考えています。

## ■ サービスの差別化ポイント・推しポイント
動画編集機能だけのサービスや予約投稿だけのサービスは従来にも存在しますが、このサービスはユーザーがファイルを投げるだけでその後を投稿状態まで一気に持っていってくれるサービスとなっているため、極力まで対象ユーザーの労力を減らすという特徴を持っています。
考えずにファイルを投げてもらって、あとは最後に少しだけ調整したい部分があれば調整する程度の労力に減らします。

## ■ 機能候補
### MVPまでの機能
- ログイン機能
- 動画生成機能
- 投稿文章の生成機能（※1）
- 予約投稿機能

(※1) hiphopのビートを販売する時に「⚪︎⚪︎ Type Beat」という文言を入れて投稿することが多い。 例えば、「Eminem Type Beat (Eminemっぽい雰囲気のビート)」という形で投稿される。 こうすることで、「Eminemっぽい雰囲気のビート」が欲しいと思ったラッパーが「Eminem Type Beat」と検索をかけるので、購入サイトに流入する可能性が上がる。 これもどのType Beatを使うかでタグが異なってくるため、ユーザーが「Eminem」と打ち込んだら、それをもとに文章やタグがある程度生成されるようにしたいと考えています。

### 本リリースまでの機能
- Youtube用の動画生成機能
- Youtubeへの予約投稿機能（APIのunit数の上限解放申請が必要）


## ■ 機能の実装方針予定
- deviseを用いてログイン機能を実装
- 各SNSのAPIとOAuthを用いてSNSへの投稿機能を実装
- Sidekiqを用いて投稿の予約機能を実装

## 技術スタック
- フロントエンド: React(+ Vite) ver. 19.1 , TailwindCSS ver. 4.1
- バックエンド: Ruby , Rails API ver. 7.2
- データベース: PostgreSQL ver. 15
- フロントエンドテストツール: Vitest ver. 3.2.3
- JS静的解析ツール: ESLint
- 自動フォーマッター: Prettier
- CI/CD: Github Actions
- デプロイサーバー: Vercel(フロントエンド), Render(バックエンド)

### 使用予定のgem
- devise
- omniauth
- sidekiq
- streamio-ffmpeg
- rails-i18n
- bullet

## 動画生成について
動画生成処理については、ffmpegを用いて、Sidekiqによって非同期的に処理を行います。
生成した動画やサムネイルなどの情報はCarrierWaveでAWS S3にアップロードするようにします。

## 負荷の分散
動画生成についてはCPU負荷が高い処理であることから、ユーザーの増加とサーバーの状況を確認しながら、別のプロセスや専用サーバー, AWS Lamdbaへ処理を切り離すことで負荷を分散させる工夫が必要になってくる。