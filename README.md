# PostgreSQL Documentation Reading

[オンライン読書会 Next](https://gist.github.com/5t111111/77feb47572c19f930c58ef34b99cbca0) のテーマとして扱う「PostgreSQLのドキュメントを読む」のためのリポジトリです。

読むのに必要となる PostgreSQL 環境をすぐに用意できるように Dockerfile を置いています。

## テーマ

PostgreSQL 10 のドキュメントを読む

https://www.postgresql.jp/document/10/html/

## 必要な PostgreSQL 実行環境

- PostgreSQL 10系

ドキュメントは 10.0 とあるが 10.x ならいいと思う

### 確認方法

```shell
$ psql --version
psql (PostgreSQL) 10.3
```

### ローカルインストール (Mac)

```shell
$ brew install postgresql
```

その後画面の指示に従ってサービスを登録すると PostgreSQL デーモンが自動起動するようになる

シンプルなローカルインストールで悩むことは少ないが、微妙な点として、

- 業務などのプロジェクトで使うバージョンが決まっている場合やりにくい
- PostgreSQL がバージョンアップした場合 `brew upgrade` でバージョンアップされちゃう

ことはあります。

### Docker を利用する

まず [Docker for Mac](https://www.docker.com/docker-mac) などをインストールして Docker の実行環境を用意する。

チュートリアルで、C のソースコードのコンパイルなどが必要になるため、`yochiyochirb/postgresql-documentation-reading` という、そういった前準備をした Docker イメージを用意しています。

PostgreSQL の Docker コンテナを起動するのは↓のコマンド1発です。

```shell
$ docker run --rm --name odknext1_postgres -p 15432:5432 \
    -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres \
    -v odknext1_postgres-data:/var/lib/postgresql/data \
    yochiyochirb/postgresql-documentation-reading
```

これで、

- コンテナ名: `odknext1_postgres`
- ホスト側ポート: 15432
- PostgreSQL バージョン: 10.3
- PostgreSQL ユーザー名: `postgres`
- `postgres` ユーザーパスワード: `postgres`
- 永続化ボリューム: `odknext1_postgres-data`

でコンテナが起動します。

※ `odknext1_postgres-data` はホストのローカルに永続化されるのでコンテナを落としてもデータは残る (`docker volume ls` で確認可能)

#### psql やその他クライアントからの接続方法

ホスト側のローカルに psql が入って入れば、

```shell
$ psql -h localhost -p 15432 -U postgres postgres
```

でコンテナで起動している PostgreSQL に接続できる。

他のクライアントでも `localhost:15432` に接続すれば OK。

#### Docker コンテナのプロセスを実行する

上記 PostgreSQL コンテナを起動した状態で、

```shell
$ docker exec -it odknext1_postgres psql -U postgres postgres
```

とする、起動中のコンテナで psql コマンドを実行できる。

もしくは、

```shell
$ docker exec -it odknext1_postgres bash
```

とすると `bash` を起動しシェル操作を行うことができる。

これだと `createdb` などのシェルから実行するコマンドも実行できるし、そこで `psql -U postgres postgres` とすれば psql も実行できる。
