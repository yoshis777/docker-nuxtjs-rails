# 概要
以下構成のDockerコンテナを作成する。手っ取り早くNuxtRailsを経験する用。
* フロント：Nuxt.js
* バック：Rails

手順は[Nuxt.js + Rails(API) on DockerのHello Worldするべ！](https://qiita.com/at-946/items/08de3c9d7611f62b1894)を参考にさせて頂きました。

# ブランチ
| ブランチ名 | 状態 |
---- | ----
| master | 最新の状態。動くやつ。 |
| builded-nuxt-rails | nuxt.jsとRails6(APIモード)のアプリ作成後の展開したもの。DB設定済。 |
| docker-build | 開発環境の最小構成。構築後のNuxt.jsやRailsファイルの中身はコミットしていない。docker-compose upにより上記構成のコンテナ群が作成される。 |

# herokuにデプロイ
* 2つのアプリ登録（2つの実サーバーを用いる）により実現
### Rails apiサーバ（back）側
* デプロイまで
```bash
$ heroku login
$ heroku apps:create nuxtjs-rails-back
$ git remote rename heroku back
$ git subtree push --prefix back back master #back分のデプロイ
$ heroku config:get DATABASE_URL #この時点でDBのURL確認（しなくてもよい）
$ heroku run -a nuxtjs-rails-back rails db:schema:load

```
* （必要なら）JWTトークン認証を用いている場合
```bash
$ heroku config:set -a nuxtjs-rails-back JWT_SECRET = $(heroku run -a nuxtjs-rails-back rails secret)
```
* テスト用にユーザ作成
```bash
$ heroku run -a nuxtjs-rails-back rails console
$ User.create! name:%{user_name}
$ exit
$ curl https://nuxtjs-rails-back.herokuapp.com/users/1
# => {"id":1,"name":"user_name","created_at":"2019-10-15T01:45:18.453Z","updated_at":"2019-10-15T01:45:18.453Z"}
```
### Nuxtjs(front)側
* 本番環境用にリクエストを受け付ける用に設定
```bash
$ heroku config:set -a nuxtjs-rails-front NODE_ENV=production HOST=0.0.0.0 NPM_CONFIG_PRODUCTION=false
```
* package.json追記（その後、git push）
```javascript
"scripts" : {
  "heroku-postbuild" : "npm run build"
}
```
* 連携用にAPI(バック側)のURLを設定
```bash
$ heroku config:set -a nuxtjs-rails-front API_URL=https://nuxtjs-rails-back.herokuapp.com/api
```
* デプロイ
```bash
$ git subtree push --prefix front front master
```

# ハマった点
#### axiosでhttp://back:3000で403エラーとなる
* [原因]Railsでlocalhostと0.0.0.0/0(IP指定の全て)のみの許可設定となっているため
* [解決]以下でホスト名指定でのアクセス許可の追記
```ruby
[development.rb]
Rails.application.config.hosts << 'back'
```

# 参考補足
* [スモールスタートではじめるSSR](https://tech.dely.jp/entry/min_ssr)
    * ※SSRで始める場合。SSRかSPAかはNuxt.jsアプリの構築時(npx create-nuxt-app)に選択できる
* [docker-compose `up` とか `build` とか `start` とかの違いを理解できていなかったのでまとめてみた。](https://qiita.com/tegnike/items/bcdcee0320e11a928d46)
* [Deploying your Nuxt+Rails API app to production with Heroku](https://medium.com/@fishpercolator/deploying-your-nuxt-rails-api-app-to-production-with-heroku-54efd09eec22)