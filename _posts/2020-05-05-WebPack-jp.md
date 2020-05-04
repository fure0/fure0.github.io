---
layout: post
categories: "JS"
title: "Webpack"
author: "TY_K"
---

![webpack](https://user-images.githubusercontent.com/20508342/80999679-063f6f00-8e80-11ea-89d7-f2c6dad4e808.png)

### なぜWebpackが登場するようになったのか?

* 規模のあるシステムでは多くのJavascriptが存在することになります。 この多くのJavascriptファイルを1つのファイルとして管理するには困難があります。

### だったら、いくつかに呼び出せば解決できるんじゃない?

* 複数のファイルをブラウザでロードすると、ネットワークコストがそれだけ使用され、反応速度が遅くなります。
* さらに、各ファイルの変数衝突の危険性も存在します。

> これを解決するためにWebpackが登場することになりました。

### Webpackとは?

* 現代のJavascriptApplicationのStatic Module Bundlerです。
* Webpackが実行されたら、Dependencies Graphを通じて必要な形態の一つまたは複数のBundleで生成します。

### Bundleとは?

* ソフトウェアおよび一部のハードウェアと一緒に動作するのに必要なすべてのものを含むパッケージ
* それぞれのモジュールに対する依存性関係を把握し、一つまたは複数のグループで見ることができます。

### では、なぜWebpackを使用しなければならないのか?

* 他のModule Bundlerも多く存在しますが、performanceに優れています。

### Browsify、Grunt、Gulpなどのツールはwebpackと何の違いがあるのか?

* 大きくて複雑で多様なリソースが入っているプロジェクトにはwebpackを使うと良いでしょう。
* Grunt、Gulpは、ただリソースに対するツールとして使われており、dependency graphに対する概念がありません。
* Browsifyは似たようなツールですが速度面でwebpackがもっと良いです。

### Webpack Core Concept

### Entry

* dependency graphを作るために必要なInput Sourceです。
* 直接·間接的に依存性を持つモジュールを理解します。
* 複数のエントリが存在することができます。
* Default value:./src/index.js

```javascript
module.exports = {
  entry: './path/to/my/entry/file.js'
};
```

### Output

* Webpackが生成したbundlesの結果物の位置を指定することができます。
* Default value:./dist/main.js

```javascript
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  }
};
```

### Loaders

* WebpackはJavascriptとJsonだけが理解できる短所があります。
* Loaderは他のTypeのファイルをWebpackが理解して処理可能なモジュールに変換させる作業を担当します。

```javascript
const path = require('path');

module.exports = {
  output: {
    filename: 'my-first-webpack.bundle.js'
  },
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  }
};
```

### Plugins

* Loaderが変換する間、Pluginはbundleoptimization、assetmanagementandinjectionofenvironmentのようなことを進めることができます。

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin'); //installed via npm
const webpack = require('webpack'); //to access built-in plugins

module.exports = {
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
};
```

### Mode

* 様々なファイルに指定して行うことができます。
* development、 production、 none
* Default value: production

```javascript
module.exports = {
  mode: 'production'
};
```

### Browser Compatibility

Webpackは、ES5を使用する全てのブラウザに対応します。 ただし、IE8の下記バージョンは対応しておりません。

[参照][Webpack]

[Webpack]: https://nesoy.github.io/articles/2019-02/Webpack "Webpack"