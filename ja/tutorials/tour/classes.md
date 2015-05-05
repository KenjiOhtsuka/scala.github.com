---
layout: tutorial
title: Classes

disqus: true

tutorial: scala-tour
num: 4
tutorial-next: case-classes
tutorial-previous: compound-types
---

Scala のクラスは実行時、様々なオブジェクトにインスタンス化できる静的なテンプレートです。
ここでは `Point` クラスの定義を見てみましょう。:

    class Point(xc: Int, yc: Int) {
      var x: Int = xc
      var y: Int = yc
      def move(dx: Int, dy: Int) {
        x = x + dx
        y = y + dy
      }
      override def toString(): String = "(" + x + ", " + y + ")";
    }

`Point` クラスは2変数 `x`、 `y` と 2つのメソッド `move`、 `toString` を定義しています。 `move` は2つの整数を引数にとり、値を返しません(暗黙の返却型 `Unit` は Java ライクな言語でいう `void` と同じです)。 一方、 `toString` は引数をとらず、 `String` の値を返します。 `toString` はあらかじめ定義された `toString` メソッドをオーバライドしているため `override` フラグでタグ付けする必要があります。

Scala のクラスはコンストラクタ引数によって特徴付けされます。 上のコードでは2つのコンストラクタ引数 `xc`、`yc` を定義しています。 2つのパラメータともにクラス内のどこからでも参照できます。 上の例では変数 `x` と `y` を初期化するのに使われています。

クラスは下の例のように、 `new` プリミティブでインスタンス化されます。:

    object Classes {
      def main(args: Array[String]) {
        val pt = new Point(1, 2)
        println(pt)
        pt.move(10, 10)
        println(pt)
      }
    }

上のプログラムは実行可能なアプリケーション `Classes` を `main` メソッドを持つトップレベルのシングルトンオブジェクトとして定義しています。 `main` メソッドは新しい `Point` クラスのインスタンスを生成し、変数 `pt` に格納します。 _`val` 宣言で定義された値は `var` 宣言で定義されたものとは違い、更新できないことに注意してください(上の `Point` クラス参照)。 いわゆる定数です。_

以下が上のプログラムの出力です。:

    (1, 2)
    (11, 12)
