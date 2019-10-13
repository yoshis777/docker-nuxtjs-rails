# 概要
以下構成のDockerコンテナを作成する。手っ取り早くNuxtRailsを経験する用。
* フロント：Nuxt.js
* バック：Rails

手順は[Nuxt.js + Rails(API) on DockerのHello Worldするべ！](https://qiita.com/at-946/items/08de3c9d7611f62b1894)を参考にさせて頂きました。

# ブランチ
| ブランチ名 | 状態 |
---- | ----
| master | 最新の状態 |
| docker-build | 開発環境の最小構成。構築後のNuxt.jsやRailsファイルの中身はコミットしていない。docker-compose upにより上記構成のコンテナ群が作成される。 |

# 補足
* [スモールスタートではじめるSSR](https://tech.dely.jp/entry/min_ssr)
    * ※SSRで始める場合。SSRかSPAかはNuxt.jsアプリの構築時(npx create-nuxt-app)に選択できる
* [docker-compose `up` とか `build` とか `start` とかの違いを理解できていなかったのでまとめてみた。](https://qiita.com/tegnike/items/bcdcee0320e11a928d46)