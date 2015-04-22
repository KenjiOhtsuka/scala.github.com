---
layout: tutorial
title: 抽象型

disqus: true

tutorial: scala-tour
num: 2
outof: 35
tutorial-next: annotations
tutorial-previous: tour-of-scala
---

Scala では、クラスは値(コンストラクタ引数)と型(クラスが[ジェネリック](generic-classes.html)なら)でパラメータ化されます。 規約により、オブジェクトのメンバとして値を持つだけでなく、値と一緒に型もオブジェクトのメンバにできます。 更に、どちらのメンバも具象・抽象のどちらにもできます。
例として、以下に値と抽象型の両方をメンバとしてもった[クラス](traits.html)の例 `Buffer` を示します。
 
    trait Buffer {
      type T
      val element: T
    }

*抽象型* はその正体が正確に知られていません。 上の例では `Buffer` というクラスのオブジェクトはどれも `T` という型のメンバを持っていますが、 `Buffer` の定義ではどの具象型がメンバの型 `T` と一致するかを明らかにしていません。 この型の定義は、値の定義のように、サブクラスでオーバライドできます。 そのような型境界(ある抽象型についての可能な具象インスタンス化について記述するもの)によって、抽象型についてより多くの情報を付け加えることができます。

下のプログラムでは、型 `T` は新しい抽象型 `U` を使用した `Seq[U]` の派生型という制約を加え、バッファ中にシーケンスのみを保存できる派生クラス `SeqBuffer` を記述しています。
 
    abstract class SeqBuffer extends Buffer {
      type U
      type T <: Seq[U]
      def length = element.length
    }
 
抽象型のメンバを持つトレイトや[クラス](classes.html)は、よく無名クラスのインスタンス化と同時に使用されます。 わかりやすくするために、32bit符号付き整数のリストを参照するシーケンスバッファを扱うプログラムを見てみましょう。
 
    abstract class IntSeqBuffer extends SeqBuffer {
      type U = Int
    }
    
    object AbstractTypeTest1 extends App {
      def newIntSeqBuf(elem1: Int, elem2: Int): IntSeqBuffer =
        new IntSeqBuffer {
             type T = List[U]
             val element = List(elem1, elem2)
           }
      val buf = newIntSeqBuf(7, 8)
      println("length = " + buf.length)
      println("content = " + buf.element)
    }
 
`newIntSeqBuf` メソッドの戻り値は `Buffer` トレイトの特化を参照していて、その中で型 `U` は `Int` と同じです。 `newIntSeqBuf` メソッドの中の無名クラスのインスタンス化でも似たような型エイリアスを定義しています。 型 `T` が `List[Int]` を参照する `IntSeqBuffer` クラスの新しいインスタンスを作成しています。

抽象型メンバをクラスの型パラメータにすることと、またその逆も可能であること覚えておいてください。 上のコードの型パラメータのみを使う例を示します。
 
    abstract class Buffer[+T] {
      val element: T
    }
    abstract class SeqBuffer[U, +T <: Seq[U]] extends Buffer[T] {
      def length = element.length
    }
    object AbstractTypeTest2 extends App {
      def newIntSeqBuf(e1: Int, e2: Int): SeqBuffer[Int, Seq[Int]] =
        new SeqBuffer[Int, List[Int]] {
          val element = List(e1, e2)
        }
      val buf = newIntSeqBuf(7, 8)
      println("length = " + buf.length)
      println("content = " + buf.element)
    }
 
ここでは[変位指定アノテーション](variances.html)を使う必要があります。 そうでないと `newIntSeqBuf` メソッドが返すオブジェクトの具象シーケンス実装型を隠すことができません。 また、型パラメータで抽象型を置き換えられない場合もあります。

