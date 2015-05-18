---
layout: tutorial
title: Case Classes

disqus: true

tutorial: scala-tour
num: 5
tutorial-previous: classes
tutorial-next: compound-types
---

Scala は _ケース クラス_ の表記をサポートしています。 ケースクラスはコンストラクタ引数をエクスポートし、[パターンマッチング](pattern-matching.html)を通して再帰的な分解メカニズムを提供する標準的なクラスです。

下は抽象クラスのスーパークラス `Term` と3つの具象クラス `Var`、`Fun`、`App` からなるクラス階層の例です。

    abstract class Term
    case class Var(name: String) extends Term
    case class Fun(arg: String, body: Term) extends Term
    case class App(f: Term, v: Term) extends Term

このクラス階層は[型付けされていないラムダ計算](http://www.ezresult.com/article/Lambda_calculus)の項を表現するのに使えます。 ケースクラスのインスタンス作成を楽にするために、 Scala は `new` プリミティブを使わなくてもよいことになっています。 単純に、クラス名を関数として使用できます。

次がその例になります。:

    Fun("x", Fun("y", App(Var("x"), Var("y"))))

ケースクラスのコンストラクタ引数はパブリックな値として扱われ、直接アクセスすることができます。

    val x = Var("x")
    println(x.name)

すべてのケースクラスについて、 Scala のコンパイラは構造的透過性を `equals` メソッドと `toString` メソッドを生成します。 たとえば:

    val x1 = Var("x")
    val x2 = Var("x")
    val y1 = Var("y")
    println("" + x1 + " == " + x2 + " => " + (x1 == x2))
    println("" + x1 + " == " + y1 + " => " + (x1 == y1))

は次のように出力されます。

    Var(x) == Var(x) => true
    Var(x) == Var(y) => false

パターンマッチングをデータ構造の分解に使う場合には、ケースクラスを定義するのが妥当です。 下のオブジェクトはラムダ計算を表示するプリティプリンタを定義しています。:

    object TermTest extends scala.App {
      def printTerm(term: Term) {
        term match {
          case Var(n) =>
            print(n)
          case Fun(x, b) =>
            print("^" + x + ".")
            printTerm(b)
          case App(f, v) =>
            print("(")
            printTerm(f)
            print(" ")
            printTerm(v)
            print(")")
        }
      }
      def isIdentityFun(term: Term): Boolean = term match {
        case Fun(x, Var(y)) if x == y => true
        case _ => false
      }
      val id = Fun("x", Var("x"))
      val t = Fun("x", Fun("y", App(Var("x"), Var("y"))))
      printTerm(t)
      println
      println(isIdentityFun(id))
      println(isIdentityFun(t))
    }

上の例で、 `printTerm` 関数は `match` キーワードで始まるパターンマッチング文として表現されており、 `case パターン => 処理` 節の並びから形成されています。
また上のプログラムは与えられた項が単純な恒等関数であることをチェックする `isIdentityFun` 関数も定義しています。 この例ではディープパターンとガードを使用しています。 与えられた値をもつパターン(ディープパターン)と照合した後で(キーワード `if` の後に定義された)ガードを評価します。 もし照合が `true` を返すなら照合は成功です。そうでなければ失敗であり次のパターンと照合されます。
