---
author: laco
description: "JavaScriptのモジュール（ES Module）について紹介します。ECMAScriptモジュールの基本的な使い方やCommonJSモジュールとの違いについて紹介します。"
---

# ES Module {#module}

ES Moduleは[Todoアプリのユースケース](../../../use-case/todoapp/README.md)で実際に動かしながら学ぶため、ここでは構文の説明とモジュールのイメージを掴むのが目的です。
この章のサンプルコードを実際に動かすためにはローカルサーバーなどの準備が必要なので、先にユースケースの章を読んでから戻ってきてもかまいません。

JavaScriptにおけるモジュールは、保守性・名前空間・再利用性のために使われます。

 * 保守性: 依存性の高いコードの集合を一箇所にまとめ、それ以外のモジュールへの依存性を減らすことができます
 * 名前空間: モジュールごとに分かれたスコープがあり、グローバルの名前空間を汚染しません
 * 再利用性: 便利な変数や関数を複数の場所にコピーアンドペーストせず、モジュールとして再利用できます

ひとつのJavaScriptのモジュールはひとつのJavaScriptファイルに対応します。
モジュールは変数や関数などを外部にエクスポートできます。また、別のモジュールで宣言された変数や関数などをインポートできます。
この章では **ECMAScriptモジュール（ESモジュール、JSモジュールとも呼ばれる）** について見ていきます。
ECMAScriptモジュールは、JavaScriptファイルをモジュール化する言語標準の機能です。

## ESモジュール {#es-modules}

ESモジュールは、[export文][]によって変数や関数などをエクスポートできます。
また、[import文][]を使って別のモジュールからエクスポートされたものをインポートできます。
インポートとエクスポートはそれぞれに **名前付き** と **デフォルト** という2種類の方法があります。

まずは名前付きエクスポート／インポート文について見ていきましょう。

### 名前付きエクスポート／インポート {#named-export-import}

**名前付きエクスポート**は、モジュールごとに複数の変数や関数などをエクスポートできます。
次の例では、`foo`変数と`bar`関数をそれぞれ名前付きエクスポートしています。
`export`文のあとに続けて`{}`を書き、その中にエクスポートする変数を入れることで、宣言済みの変数を名前付きエクスポートできます。
また、`export`文を宣言の前につけると、宣言と同時に名前付きエクスポートできます。

[import, title="exportExample.js"](src/named-export-1.js)

**名前付きインポート**は、指定したモジュールから名前を指定して選択的にインポートできます。
次の例では `exportExample.js`から名前付きエクスポートされたオブジェクトの名前を指定して名前付きインポートしています。
`import`文のあとに続けて`{}`を書き、その中にインポートしたい名前付きエクスポートの名前を入れます。

[import, title="importExample.js"](src/named-import-1.js)

名前付きエクスポート／インポートには**エイリアス**の仕組みがあります。
エイリアスを使うと、宣言済みの変数を違う名前で名前付きエクスポートできます。
エイリアスをつけるには、次のように`as`のあとにエクスポートしたい名前を記述します。

[import, title="exportExample.js"](src/named-export-2.js)

また、名前付きインポートしたオブジェクトにも別名をつけることができます。
インポートでも同様に、`as`のあとにインポートしたい名前を記述します。

[import, title="importExample.js"](src/named-import-2.js)

### デフォルトエクスポート／インポート {#default-export-import}

次に、デフォルトエクスポート／インポートについて見ていきましょう。
**デフォルトエクスポート**は、モジュールごとにひとつしかエクスポートできない特殊なエクスポートです。
次の例は、すでに宣言されている変数をデフォルトエクスポートしています。
`export default`文で、後に続く式の評価結果をデフォルトエクスポートします。

[import, title="exportExample.js"](src/default-export-1.js)

また、`export`文を宣言の前につけると、宣言と同時にデフォルトエクスポートできます。
このとき関数やクラスは名前を省略できます。

<!-- exportがないため -->
<!-- doctest:disable -->
```js
// 宣言と同時に関数をデフォルトエクスポートする
export default function() {}
```
<!-- doctest:enable -->

ただし、変数宣言は宣言とデフォルトエクスポートを同時に行うことはできません。
なぜなら、変数宣言はカンマ区切りで複数の変数を定義できてしまうためです。
次の例は実行できない不正なコードです。

[import, exportExample.js](src/default-export-variables-invalid.js)

**デフォルトインポート**は、指定したモジュールのデフォルトエクスポートに名前をつけてインポートします。
次の例では `exportExample.js`のデフォルトエクスポートに`myModule`という名前をつけています。
`import`文のあとに任意の名前をつけることで、デフォルトエクスポートをインポートできます。

[import, title="importExample.js"](src/default-import-1.js)

実はデフォルトエクスポートは、`default`という固有の名前による名前付きエクスポートと同じものです。
そのため、名前付きエクスポートで`as default`とエイリアスをつけることでデフォルトエクスポートすることもできます。

[import, title="exportExample.js"](src/default-export-2.js)

同様に、名前付きインポートにおいても`default`という名前がデフォルトインポートに対応しています。
次のように、名前付きインポートで`default`を指定するとデフォルトインポートできます。
ただし、`default`は予約語なので、この方法では必ず`as`構文を使ってエイリアスをつける必要があります。

[import, title="importExample.js"](src/default-import-1.js)

また、名前付きインポートとデフォルトインポートの構文は同時に記述できます。
次のように2つの構文をカンマでつなげます。

[import, importExample.js](src/default-import-3.js)

ESモジュールでは、エクスポートされていないものはインポートできません。
なぜならESモジュールはJavaScriptのパース段階で依存関係が解決され、インポートする対象が存在しない場合はパースエラーとなるためです。
デフォルトインポートは、指定したモジュールがデフォルトエクスポートをしている必要があります。
同様に名前付きインポートは、指定したモジュールが指定した名前付きエクスポートをしている必要があります。

### その他の構文 {#other-syntax}

ES Moduleには名前付きとデフォルト以外にもいくつかの構文があります。

#### 再エクスポート {#re-export}

再エクスポートとは、別のモジュールからエクスポートされたものを、改めて自分自身からエクスポートしなおすことです。
複数のモジュールからエクスポートされたものをまとめたモジュールを作るときなどに使われます。

再エクスポートするは次のように`export`文のあとに`from`を続けて、別のモジュール名を指定します。

[import, exportExample.js](src/re-export-invalid.js)

#### すべてをインポート {#namespace-import}

`import * as`構文は、すべての名前付きエクスポートをまとめてインポートします。
この方法では、モジュールごとの **名前空間** となるオブジェクトを宣言します。
エクスポートされた変数や関数などにアクセスするには、その名前空間オブジェクトのプロパティを使います。

[import, importExample.js](src/namespace-import.js)

#### 副作用のためのインポート {#import-for-side-effect}

モジュールの中には、グローバルのコードを実行するだけで何もエクスポートしないものがあります。
たとえば次のような、グローバル変数を操作するためのモジュールなどです。

[import, sideEffects.js](src/sideEffects.js)

このようなモジュールをインポートするには、副作用のためのインポート構文を使います。
この構文では、モジュールのグローバルコードを実行するだけで何もインポートしません。

[import, importExample.js](src/import-side-effects.js)

## ESモジュールを実行する {#run-es-modules}

作成したESモジュールを実行するためには、起点となるJavaScriptファイルをESモジュールとしてWebブラウザに読み込ませる必要があります。
Webブラウザは`script`タグによってJavaScriptファイルを読み込み、実行します。
次のように`script`タグに`type="module"`属性を付与すると、WebブラウザはJavaScriptファイルをESモジュールとして読み込みます。

```html
<!-- myModule.jsをECMAScriptモジュールとして読み込む -->
<script type="module" src="./myModule.js"></script>
<!-- インラインでも同じ -->
<script type="module">
import { foo } from "./myModule.js";
</script>
```

`type="module"`属性が付与されない場合は通常のスクリプトとして扱われ、ECMAScriptモジュールの機能は使えません。
スクリプトとして読み込まれたJavaScriptで`import`文や`export`文を使用すると、シンタックスエラーが発生します。

また、インポートされるモジュールの取得はネットワーク経由で解決されます。
そのため、モジュール名はJavaScriptファイルの絶対URLあるいは相対URLを指定します。
詳しくは[Todoアプリのユースケース](../todoapp/README.md)を参照してください。

[export文]: https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/export
[import文]: https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/import