+++
title = "さっそくはじめる"
date = 2021-05-01T08:00:00+00:00
updated = 2021-05-01T08:00:00+00:00
draft = false
weight = 2
sort_by = "weight"
template = "docs/page.html"

[extra]
toc = true
top = false
flair =[]
+++

<img style="width:100%; max-width:640px" src="tour.png"/>
<br/>
<br/>
<br/>
数分でLoco上にブログアプリのバックエンドを作成しましょう。まず、`loco-cli`と`sea-orm-cli`をインストールします：

<!-- <snip id="quick-installation-command" inject_from="yaml" template="sh"> -->
```sh
cargo install loco-cli
cargo install sea-orm-cli # DBが必要な場合のみ
```
<!-- </snip> -->

次に、新しいアプリを作成します（「SaaSアプリ」を選択）。

 ```sh
 ❯ loco new
✔ ❯ App name? · myapp
✔ ❯ What would you like to build? · SaaS app (with DB and user auth)
✔ ❯ Select a DB Provider · Sqlite
✔ ❯ Select your background worker type · Async (in-process tokio async tasks)
✔ ❯ Select an asset serving configuration · Client (configures assets for frontend serving)

 🚂 Loco app generated successfully in:
 myapp/
 ```

すべてのデフォルトを選択すると、以下のようになります：

* データベースには`sqlite`を使用します。データベースプロバイダーについては、_models_セクションの[Sqlite vs Postgres](@/docs/the-app/models.md#sqlite-vs-postgres)で学べます。
* バックグラウンドワーカーには`async`を使用します。ワーカーの設定については、_workers_セクションの[async vs queue](@/docs/processing/workers.md#async-vs-queue)で学べます。
* アセットサービングの設定は`Client`です。これは、バックエンドがAPIとして機能することを意味します。

次に、`myapp`ディレクトリに移動し、`cargo loco start`を実行してアプリを起動します：

<div class="infobox">

`Client`アセットサービングオプションを設定している場合、サーバーを起動する前にフロントエンドをビルドする必要があります。これは、フロントエンドディレクトリに移動して（`cd frontend`）、`pnpm install`と`pnpm build`を実行することで行えます。
</div>

<!-- <snip id="starting-the-server-command-with-output" inject_from="yaml" template="sh"> -->
```sh
$ cargo loco start

                      ▄     ▀
                                ▀  ▄
                  ▄       ▀     ▄  ▄ ▄▀
                                    ▄ ▀▄▄
                        ▄     ▀    ▀  ▀▄▀█▄
                                          ▀█▄
▄▄▄▄▄▄▄  ▄▄▄▄▄▄▄▄▄   ▄▄▄▄▄▄▄▄▄▄▄ ▄▄▄▄▄▄▄▄▄ ▀▀█
██████  █████   ███ █████   ███ █████   ███ ▀█
██████  █████   ███ █████   ▀▀▀ █████   ███ ▄█▄
██████  █████   ███ █████       █████   ███ ████▄
██████  █████   ███ █████   ▄▄▄ █████   ███ █████
██████  █████   ███  ████   ███ █████   ███ ████▀
  ▀▀▀██▄ ▀▀▀▀▀▀▀▀▀▀  ▀▀▀▀▀▀▀▀▀▀  ▀▀▀▀▀▀▀▀▀▀ ██▀
      ▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀
                https://loco.rs

listening on port 5150
```
<!-- </snip> -->

<div class="infobox">

`cargo`を通して実行する必要はありませんが、開発中は強く推奨されます。`--release`でビルドすると、バイナリにはコードと`cargo`またはRustが含まれるため、不要になります。
</div>

## CRUD APIの追加

ユーザー認証が生成された基本的なSaaSアプリがあります。`post`を追加し、完全なCRUD APIを`scaffold`を使用して作成しましょう。

<div class="infobox">

`-k`フラグを使用して、`api`、`html`、または`htmx`のスキャフォールドを生成することができます。
</div>

```sh
$ cargo loco generate scaffold post title:string content:text --api

  :
  :
added: "src/controllers/post.rs"
injected: "src/controllers/mod.rs"
injected: "src/app.rs"
added: "tests/requests/post.rs"
injected: "tests/requests/mod.rs"
* Migration for `post` added! You can now apply it with `$ cargo loco db migrate`.
* A test for model `posts` was added. Run with `cargo test`.
* Controller `post` was added successfully.
* Tests for controller `post` was added successfully. Run `cargo test`.
```

データベースがマイグレーションされ、モデル、エンティティ、完全なCRUDコントローラーが自動的に生成されました。

再度アプリを起動します：
<!-- <snip id="starting-the-server-command-with-output" inject_from="yaml" template="sh"> -->
```sh
$ cargo loco start

                      ▄     ▀
                                ▀  ▄
                  ▄       ▀     ▄  ▄ ▄▀
                                    ▄ ▀▄▄
                        ▄     ▀    ▀  ▀▄▀█▄
                                          ▀█▄
▄▄▄▄▄▄▄  ▄▄▄▄▄▄▄▄▄   ▄▄▄▄▄▄▄▄▄▄▄ ▄▄▄▄▄▄▄▄▄ ▀▀█
██████  █████   ███ █████   ███ █████   ███ ▀█
██████  █████   ███ █████   ▀▀▀ █████   ███ ▄█▄
██████  █████   ███ █████       █████   ███ ████▄
██████  █████   ███ █████   ▄▄▄ █████   ███ █████
██████  █████   ███  ████   ███ █████   ███ ████▀
  ▀▀▀██▄ ▀▀▀▀▀▀▀▀▀▀  ▀▀▀▀▀▀▀▀▀▀  ▀▀▀▀▀▀▀▀▀▀ ██▀
      ▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀
                https://loco.rs

listening on port 5150
```
<!-- </snip> -->

<div class="infobox"> 

どの`-k`オプションを選択したかによって、スキャフォールドされたリソースを作成する手順が変わります。`api`フラグまたは`htmx`フラグを使用した場合は、以下の例を使用できます。しかし、`html`フラグの場合は、ブラウザで投稿作成手順を実行することをお勧めします。

`curl`を使用して`html`スキャフォールドをテストしたい場合、リクエストを`application/x-www-form-urlencoded`のContent-Typeで送信し、ボディを`title=Your+Title&content=Your+Content`とする必要があります。これは、必要に応じてコード内で`application/json`をContent-Typeとして許可するように変更できます。

</div>

次に、`curl`を使って`post`を追加してみましょう：

```sh
$ curl -X POST -H "Content-Type: application/json" -d '{
  "title": "Your Title",
  "content": "Your Content xxx"
}' localhost:5150/posts
```

投稿をリストするには：

```sh
$ curl localhost:5150/posts
```

ブログバックエンドを作成するためのコマンドは次の通りです：

1. `cargo install loco-cli`
2. `cargo install sea-orm-cli`
3. `loco new`
4. `cargo loco generate scaffold post title:string content:text --api`

完了です！`loco`での旅を楽しんでください 🚂

## SaaS認証の確認

生成されたアプリには、JWTに基づく完全に機能する認証スイートが含まれています。

### 新しいユーザーの登録

`/api/auth/register`エンドポイントは、データベースに新しいユーザーを作成し、アカウント確認のための`email_verification_token`を生成します。ユーザーには確認リンク付きの歓迎メールが送信されます。

```sh
$ curl --location 'localhost:5150/api/auth/register' \
     --header 'Content-Type: application/json' \
     --data-raw '{
         "name": "Loco user",
         "email": "user@loco.rs",
         "password": "12341234"
     }'
```

セキュリティ上の理由から、ユーザーがすでに登録されている場合は、新しいユーザーは作成されず、200ステータスが返されますが、ユーザーのメール詳細は公開されません。

### ログイン

新しいユーザーを登録した後、次のリクエストを使用してログインします：

```sh
$ curl --location 'localhost:5150/api/auth/login' \
     --header 'Content-Type: application/json' \
     --data-raw '{
         "email": "user@loco.rs",
         "password": "12341234"
     }'
```

レスポンスには、認証用のJWTトークン、ユーザーID、名前、確認状況が含まれます。

```sh
{
    "token": "...",
    "pid": "2b20f998-b11e-4aeb-96d7-beca7671abda",
    "name": "Loco user",
    "claims": null
    "is_verified": false
}
```

クライアント側のアプリでは、このJWTトークンを保存し、次のリクエストを行う際に_Bearerトークン_として使用します（以下を参照）。

### 現在のユーザーを取得

このエンドポイントは認証ミドルウェアによって保護されています。以前に取得したトークンを使用して、_Bearerトークン_技術を使ってリクエストを行います（`TOKEN`を取得したJWTトークンに置き換えます）：

```sh
$ curl --location --request GET 'localhost:5150/api/user/current' \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer TOKEN'
```

これが最初の認証されたリクエストになります！

`controllers/auth.rs`のソースコードを確認して、独自のコントローラーで認証ミドルウェアを使用する方法を学びましょう。
