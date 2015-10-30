# js勉強会 (4?)フロントエンドのテスト

## アジェンダ
* フロントエンドのテストの種類
* node.jsのテストとの違い
* 何故ブラウザテストが必要か？
* Try it!
* karma.conf.jsの中身

## フロントエンドのテストの種類
フロントエンドで行うテストは大きく分けて2種類ある。今回は単体/結合テストを行う。

### 単体/結合テスト
* JavaScriptをモジュール単位でテストする
* ブラウザ用テストランナー(testem or Karma)

### E2Eテスト
* 実際のユーザー動作、DOM Eventをエミュレートしてテストする
* ブラウザ自動化ツール(Selenium)

## node.jsのテストとの違い
フロントエンドの単体/結合テストでは、node.jsのテストで用いたテストフレームワーク/アサーションライブラリに加え、ブラウザ用テストランナー(testem/karma)が必要。これはnode.jsのテストと、ブラウザテストのスタックの違いがある。

### node.jsのテストスタック
前回までの勉強会で習った通り、node.jsのテストにはテストフレームワーク(mocha)と、アサーションライブラリ(power-assert)が必要。それら全てがnode.js上で動く仕組みになっている。

![](https://github.com/KeitaMoromizato/js-study-front-test/wiki/images/0_stack/1.png)

### ブラウザテストのスタック
一方ブラウザテストの場合、テストコード・テストターゲットがブラウザ上で動く。それらをブラウザ上で動かす＋結果をコンソール(node.js)上に表示する役割を果たすのがテストランナー(testem/karma)である。
テストランナーを起動すると以下のような処理が実行される。
1. ローカルサーバーが起動する
2. ブラウザが自動的に起動し、(1)で起動したサーバーにアクセス。テストコード・テストターゲットを取得
3. ブラウザ上でテストが実行され、その結果がsocket.ioを使用してローカルサーバー(karma)上に表示される

![](https://github.com/KeitaMoromizato/js-study-front-test/wiki/images/0_stack/2.png)

## 何故ブラウザテストが必要か？
node.jsとブラウザではJavaScriptの実行エンジンが違う。同じJavaScriptと言えど、node環境で動いていた物がブラウザで動くとは限らない。

例
* node v0.12ではES6の一部の機能(Set/Map)が使えるが、IE11/12では使えない
* うっかりnodeのAPI (process.nextTickとか)を使ってしまう

また、DOM操作もブラウザ上でしか出来ないので、フロントエンドのコードの信頼性を確認するためにはブラウザテストが必須。

## Try it!

本レポジトリをCloneします。

```
$ git clnoe https://github.com/KeitaMoromizato/js-study-front-test
$ npm install
```

以下のコマンドでテストを実行。ブラウザが起動し、テスト結果がコンソールに出たらOKです。

```
$ npm test
```

テストコマンドは`package.json`内で定義されています。今回はテストランナーにKarmaを使っています。 karmaがグローバルインストールされていれば `karma start`でも同じことですが、テストは基本的に誰の環境でも同じように動くことが求められているため、ローカルインストールされたKarmaを使用しています。

```
  "scripts": {
    "test": "./node_modules/karma/bin/karma start"
  },
```

テストの設定ファイルは全て`karma.conf.js`内に書かれています。

## karma.conf.jsの中身

