# PISUCONマニュアル
## ポータルサイト

http://isucon-portal.to-hutohu.trap.show

上記リンクを開いてください。計測ツールで測定したスコアはこのポータルに送られ、集計結果を見ることができます。

## Getting Started
### 1. インスタンスを作成する
下のリンクを開き、インスタンスを作成するボタンを押してください。

[インスタンス作成](http://isucon-portal.to-hutohu.trap.show/#/team-info)

IPアドレスやパスワードが表示されます。

しばらく待ってリロードしても作成されない場合は、@to-hutohuまで連絡してください

### 2. 起動したインスタンスに `isucon` ユーザで SSH ログインする

例:

```
ssh isucon@xx.xx.xx.xx
```
パスワードは上のページで表示されているパスワードです。

### 4. ソースコードのダウンロード
このリポジトリをサーバー上でクローンしてください。

### 5. アプリケーションに必要なソフトのインストール
* 各言語の実行環境
* Mysql
* Memcached

上記の3つがアプリケーションの実行に最低限必要なソフトです。

Nginx等その他に必要なものがあれば、適宜インストールしてください。

### 6. データのインポート

https://github.com/catatsuy/private-isu/releases/download/img/dump.sql.bz2

上のファイルをダウンロードし、サーバー上の適当な位置に配置します。

`bzcat dump.sql.bz2 | mysql -uroot`

上のコマンドを実行し、データのインポートを行います。

(ユーザー名・パスワードを変更した場合は適宜変更してください。)

### 7. アプリケーションの実行
アプリケーションを実行してみてください。

環境変数等の設定はコードを見て行ってください。

### 8. アプリケーションの動作を確認

Team Infoに表示されているIPアドレスにブラウザでアクセスし、動作を確認してください。以下の画面が表示されるはずです。

例として、「アカウント名」は `mary`、 「パスワード」は `marymary` を入力することでログインが行えます。

ブラウザでアクセスできない場合、主催者に確認してください。

### 9. 負荷走行を実行
ポータルのTeamInfoでベンチマークを行うボタンを押して、ベンチマークを開始してください。

キューはページ上部に表示されています。

この操作後、ポータルにて、あなたのチームのスコアが反映されているか確認して下さい。


### スコアについて

基本スコアは以下のルールで算出されます。

```
成功レスポンス数(GET) x 1 + 成功レスポンス数(POST) x 2 + 成功レスポンス数(画像投稿) x 5 - (サーバエラー(error)レスポンス数 x 10 + リクエスト失敗(exception)数 x 20 + 遅延POSTレスポンス数 x 100)
```

ただし、基本スコアと計測ツールの出すスコアが異なっている場合は、計測ツールの出すスコアが優先されます。

#### 減点対象

以下の事項に抵触すると減点対象となります。

  * 存在するべきファイルへのアクセスが失敗する
  * リクエスト失敗（通信エラー等）が発生する
  * サーバエラー(Status 5xx)・クライアントエラー(Status 4xx)をアプリケーションが返す
  * 他、計測ツールのチェッカが検出したケース

#### 注意事項

  * リダイレクトはリダイレクト先が正しいレスポンスを返せた場合に、1回レスポンスが成功したと判断します
  * POSTの失敗は大幅な減点対象です

### 制約事項

以下の事項に抵触すると点数が無効となります。

  * GET /initialize へのレスポンスが10秒以内に終わらない
  * 存在するべきDOM要素がレスポンスHTMLに存在しない


## 禁止事項

以下の事項は特別に禁止する。

  * 他のチームへの妨害と主催者がみなす全ての行為

## ソフトウェア事項

コンテストにあたり、参加者は与えられたソフトウェア、もしくは自分で競技時間内に実装したソフトウェアを用いる。

高速化対象のソフトウェアとして主催者からRuby, PHPによるWebアプリケーションが与えられる。ただし各々の性能が一致することを主催者は保証しない。どれをベースに用いてもよいし、独自で実装したものを用いてもよい。

競技における高速化対象のアプリケーションとして与えられたアプリケーションから、以下の機能は変更しないこと。

  * アクセス先のURI（ポート、およびHTTPリクエストパス）
  * レスポンス(HTML)のDOM構造
  * JavaScript/CSSファイルの内容
  * 画像および動画等のメディアファイルの内容

各サーバにおけるソフトウェアの入れ替え、設定の変更、アプリケーションコードの変更および入れ替えなどは一切禁止しない。起動したインスタンス以外の外部リソースを利用する行為 (他のインスタンスに処理を委譲するなど) は禁止する。

許可される事項には、例として以下のような作業が含まれる。

  * DBスキーマの変更やインデックスの作成・削除
  * キャッシュ機構の追加、jobqueue機構の追加による遅延書き込み
  * 他の言語による再実装

ただし以下の事項に留意すること。

  * コンテスト進行用のメンテナンスコマンドが正常に動作するよう互換性を保つこと
  * 各サーバの設定およびデータ構造は任意のタイミングでのサーバ再起動に耐えること
  * サーバ再起動後にすべてのアプリケーションコードが正常動作する状態を維持すること
  * ベンチマーク実行時にアプリケーションに書き込まれたデータは再起動後にも取得できること

# 採点

採点は採点条件（後述）をクリアした参加者の間で、性能値（後述）の高さを競うものとする。

採点条件として、以下の各チェックの検査を通過するものとする。

  * 負荷走行中、POSTしたデータが、POSTへのHTTPレスポンスを返してから即座に関連するURI GETのレスポンスデータに反映されていること
  * レスポンスHTMLのDOM構造が変化していないこと
  * ブラウザから対象アプリケーションにアクセスした結果、ページ上の表示および各種動作が正常であること

性能値として、以下の指標を用いる。

  * 計測ツールの実行時間は1分間とする
    * 細かい閾値ならびに配点についての詳細は当日のマニュアルに記載する
  * 計測時間内のHTTPリクエスト成功数をベースとする
    * リクエストの種類毎に配点を変更する
    * エラーの数により減点する