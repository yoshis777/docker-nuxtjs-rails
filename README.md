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