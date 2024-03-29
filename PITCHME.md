## Node.js 超入門

### ECL 輪読発表
#### 蒔田圭輔


---

## OUTLINE

- ### A-1 値と変数の基本
- ### 2-1 ソースコードの基本
- ### 2-2 HTMLファイルを使おう

---

## A-1 値と変数の基本

+++

### JavaScriptとNode.jsの関係
- JavaScript(以下js)は、webブラウザ上で動作するスクリプト言語
- Node.js(以下node)はサーバサイドで動作するjsの環境
　
\* 本節では、jsの基本的な仕様について述べる

+++?code=a1.html

### A-1 値と変数の基本

#### Webブラウザでjsを利用してみよう


+++

![output](img/fig_a1.png)

ブラウザでの出力画面

+++

### プログラムを書くにあたっって気をつけること

- 文は半角が基本
- 大文字と小文字は別扱い |
- スペースは無視可能 |
- インデントは大切 |
- 文は改行またはセミコロン |
- 補足等はコメントアウト |

+++?code=hello.rb

#### [蛇足] Rubyでは

+++

## jsを書く場所
- htmlに記述する場合
- 別ファイルに記述する場合  

+++?code=a1.html

#### htmlに記述する場合

 < script > </ script >の間に記入する

+++?code=hello.js

#### 別ファイルに記述する場合

+++?code=hello.html

#### scriptタグ内でソースファイルを指定

+++

![output1](img/fig_a2.png)

+++

### いろいろな値
- 数値 'number' 型 |
    - 100, 3.14
- 文字列 'string' 型 |
    - 'Hello', "world"
- 真偽値　'boolean' 型 |
    - true, false
- null型 |
- undefined型 |

#### typeof hoge で型が分かる

+++

### 変数

#### 変数の宣言

``` javascript
var hoge0;

hoge1 = 1;

let hoge2;

const hoge3;
```
- 代入するなら 変数 = 値 |
- 変数の値取得は 代入先 = 変数名 |

+++

### 計算をする
- 加算: +
- 減算: -
- 乗算: *
- 除算: /

+++ ?code=a3.html

+++ ?code=a3.js

+++

![output2](img/fig_a3.png)

+++

### まとめ

- JavaScriptはhtml上で動作するスクリプト言語
- htmlに直書きするか、jsファイルを作成してhtmlで読み込む |
- 値を保存し、管理する"変数"
- 主要なデータ型3つは、number, string, boolean |
- 変数計算をして、その値をhtml上に出力することができる

---

## 2-1 ソースコードの基本

+++

#### 第一回にて実行したソースを分解してみる

``` js
require('http').createServer(
    (rq,rs)=>{rs.end('Hello Node.js!');}).listen(3000);
```

+++?code=2-01.js

#### こんなかんじにできる

+++

### Nodeで実行してみる

``` shell
$ node app.js
```

+++

#### localhost3000番にアクセス

![output3](img/fig_201.png)

+++

### :3000って?

- ポート番号と言い、サーバへアクセスする上、同一サーバ上でサービスの選択をする事ができる |

### どうして3000番なの?

- nodeでは標準的に3000番を利用するようになっている |

+++?code=2-01.js

@[1](httpモジュールを取得して代入)
@[3](サーバオブジェクトの生成)
@[4,5,6](クライアントからアクセスされたときの動作を無名関数で定義)
@[8](ポート番号3000番によるアクセスを待機)

+++

### アクセスに対してhtmlを返すことを考える

いままではただのテキストだったのを、HTMLの出力にしたい

+++?code=2-02.js

+++
![output4](img/fig_202.png)

+++

### そもそも...
- HTTP通信でサーバからhtmlを取得している |
- HTTP通信をする上で、現状だと不足してるものが... |
- ヒント: 先程つくったhtml部はレスポンスボディ部になる

+++

### ヘッダをつける必要がある!!

+++?code=2-03.js

@[5](ヘッダを設定する)
@[5](送信するコンテンツはhtmlであることを定義)

+++

![output5](img/fig-212.png)


+++?code=2-03.js
### ヘッダ情報の他に追加したもの

@[6](使用している言語が日本語で有ることを定義)
@[7](使用する文字コードがUTF-8であることを定義)

+++

### console.logについて

- console.logはいわゆる標準出力でクライアントには最初は見えない |
- デバッグなどで動作確認するために利用しよう |

+++ 

### まとめ

- nodeでサーバオブジェクトを作るときに実行する文は
    - require('http'); |
- サーバを、クライアントからの3030番アクセス待ちにする為の関数は
    -  server.listen(3030); |
- レスポンスを返す際にHTML出力をしたい場合、response.endの他の手段は
    - response.write |

--- 

## 2-2 HTMLファイルを使おう 

+++

### さっきまでは

- jsソース内に書いてあったHTMLを返していた
- js内で書くのってめんどう...
- htmlを別ファイルで読み込めるようにしよう |

+++?code=2-04.html

### 今回のhtmlソース


+++?code=2-05.js

### 別ファイルを読み込んでそれを返すプログラム

@[2](ファイルを扱うFileSystemオブジェクトの作成)
@[6](読み込みたいファイルと文字コードを指定)
@[9](dataの中に読み込んだ情報が入っている)


+++ 

### 今度は...
#### 関数に分解したい

- 整理しやすい |
- 再利用性があがるetc... |

+++?code=2-06.js

@[4](後述のgetFromClient関数でレスポンス処理を分解している)
@[11,12,13,14,15,16,17,18,19,20,21](切り離して記述した処理)
@[12](関数の定義は funtion でおこなう)

+++?code=2-07.js
コールバック関数単位で分解してみる

@[3,4](分解した際に渡せなくなる値用に宣言)
@[13,14,15,16,17,18](createServerのコールバック関数部)
@[22,23,24,25,26](readfileのコールバック関数部)

+++

### 非同期処理について

#### ざっくり
- 順番に処理をしていく: 同期処理 |
- とにかく命令を優先(処理の進捗は二の次) 非同期処理 |

- keyword: async, await

+++

### jsでできる他のこと
#### テキストの操作 |

+++?code=2-08.html

### ちょっと修正

+++?code=2-09.js

@[17,18,19](読み込んだdata, htmlファイルから、対応する文字列を置換する)
@[18,19](/ hoge /g は、正規表現)


+++ 

### まとめ
- ファイルを扱うときに必要なオブジェクト
    - require('fs'); |
- 関数を定義する際に最初に書くモノ
    - function |
- 同期処理と非同期処理、命令を出すこと優先なのは
    - 非同期処理 |

+++

### まとめ(つづき)
- テキスト(文字列)の置換をするときに必要な関数
    - replace(); |
- 文字列のパターンマッチングにつかう/hoge/g は?
    - 正規表現 |
