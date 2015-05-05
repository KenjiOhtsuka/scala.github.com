---
layout: tutorial
title: Extractor Objects

disqus: true

tutorial: scala-tour
num: 8
tutorial-next: generic-classes
tutorial-previous: sequence-comprehensions
---

Scala では、パターンをケースクラスとは独立して定義することができます。 そのためには、いわゆるエクストラクタを生成するための `unapply` という名前のメソッドを定義します。 たとえば、次のコードはエクストラクタオブジェクト `Twice` を定義しています。
In Scala, patterns can be defined independently of case classes. To this end, a method named unapply is defined to yield a so-called extractor. For instance, the following code defines an extractor object Twice.

    object Twice {
      def apply(x: Int): Int = x * 2
      def unapply(z: Int): Option[Int] = if (z%2 == 0) Some(z/2) else None
    }
    
    object TwiceTest extends App {
      val x = Twice(21)
      x match { case Twice(n) => Console.println(n) } // prints 21
    }

ここで関係する2つの構文上の規約があります。:
There are two syntactic conventions at work here:

パターン `case Twice(n)` は `Twice.unapply` を呼び出し、偶数との照合に使われます。 `unapply` の戻り値は引数が照合したか否かを表し、また更なる照合に使用可能なサブ値となります。 ここでサブ値は `z/2` です。
The pattern `case Twice(n)` will cause an invocation of `Twice.unapply`, which is used to match any even number; the return value of the `unapply` signals whether the argument has matched or not, and any sub-values that can be used for further matching. Here, the sub-value is `z/2`

`apply` メソッドはパターンマッチングには必要ではありません。 単にコンストラクタを真似るために使われます。 `val x = Twice(21)` は `val x = Twice.apply(21)` として展開されます。
The `apply` method is not necessary for pattern matching.  It is only used to mimick a constructor. `val x = Twice(21)` expands to `val x = Twice.apply(21)`.

`unapply` の戻り値は次のように選ばれるべきです。
The return type of an `unapply` should be chosen as follows:
* 単なるテストなら、 `Boolean` を返す。 例えば `case even()`
* If it is just a test, return a `Boolean`. For instance `case even()`
* 型 T のひとつのサブ値を返すなら、 `Option[T]` を返す。
* If it returns a single sub-value of type T, return an `Option[T]`
* 複数のサブ値 `T1,...,Tn` を返したいなら、それらを一つのオプショナルタプル `Optional(T1,...,Tn)` にまとめる。
* If you want to return several sub-values `T1,...,Tn`, group them in an optional tuple `Option[(T1,...,Tn)]`.

時々、サブ値の数が固定でシーケンスを返したいことがあります。 そのような場合は、 `unapplySeq` を使ってパターンを定義することができます。 最後のサブ値 `Tn` は `Seq[S]` となければなりません。 この機構は例えばパターン `case List(x1, ..., xn)` の中で使われます。
Sometimes, the number of sub-values is fixed and we would like to return a sequence. For this reason, you can also define patterns through `unapplySeq`. The last sub-value type `Tn` has to be `Seq[S]`. This mechanism is used for instance in pattern `case List(x1, ..., xn)`.

エクストラクタはコードをより保守しやすいものにします。 詳しくは Emir, Odesky, Williams による論文["Matching Object with Patterns"](http://lamp.epfl.ch/~emir/written/MatchingObjectsWithPatterns-TR.pdf) (2007年1月) (セクション4)  を読んでください。
