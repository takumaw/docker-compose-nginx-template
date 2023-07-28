Docker Compose Nginx Template
========================================

このプロジェクトについて
----------------------------------------

サービスフロントエンドの構成を格納したプロジェクトです。


プロジェクト構成
----------------------------------------

* `docker-compose.yml` ... docker compose 設定
* `config/` ... アプリケーションの設定ファイル類を格納
* `data/` ... アプリケーションの永続データを格納
  * ※rsync バックアップの対象
* `image/` ... イメージのビルドに必要な資材を格納
  * ※ビルドコンテキストはここ
  * `Dockerfile` ... イメージの Dockerfile
  * `*` ... その他イメージに焼きこむファイル
* `scripts/` ... 運用時に利用するスクリプトを格納


前提条件
----------------------------------------

* Docker >= 24.0
  * 作成時に試験に用いたバージョンに基づきます。
  * Docker Compose v3.8 を利用しているため、>= 19.03.0 であれば動くと思われます。


サービス運用手順
----------------------------------------

### サービスのインストール

サービスを `/srv/service-proxy` に配備します。

    $ cd /srv
    $ git clone ${REPOSITORY_URL} service-proxy

systemd unit を配備します。

    $ ln -s /srv/gitlab/systemd/service-proxy.service /etc/systemd/system/

### サービスのアップデート

`docker-compose.yml` などを適宜更新し、commit、push します。
その後、サーバ上で git pull します。

    $ cd /srv/gitlab
    $ git pull

### サービスの有効化

systemd unit を有効化します。

    $ systemctl enable service-proxy.service

### サービスの起動

systemd unit を起動します。

    $ systemctl start service-proxy.service

### サービスの停止

systemd unit を停止します。

    $ systemctl stop service-proxy.service

### 設定の検証

`nginx -t` コマンドを用いて nginx 設定ファイルの検証ができます。

    % docker compose run --rm service-proxy nginx -t
    nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
    nginx: configuration file /etc/nginx/nginx.conf test is successful
