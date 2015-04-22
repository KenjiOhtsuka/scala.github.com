---
layout: tutorial
title: Annotations

disqus: true

tutorial: scala-tour
num: 3
language: ja
---

アノテーションはメタ情報を定義と関連付けます。

単純なアノテーション節は `@C` または `@C(a1, .., an)` の形をしています。 ここで `C` はクラス `C` のコンストラクタで、クラス `scala.Annotation` に一致します。 そして、コンストラクタに渡されるすべての引数 `a1, .., an` は定数式(数値、文字列、クラス、 Java の enum またはそれらの1次配列)でなければなりません。

アノテーション節は、それに続く最初の定義または宣言に適用されます。 1つ以上のアノテーション節が定義や選言の前に記述されることもありますが、その場合のアノテーション節の順番は定められていません。

アノテーション節の意味は _インプリメンテーションディペンデント_ です。 Java のプラットフォームでは、次の Scala のアノテーションは標準的な意味を持っています。

|           Scala           |           Java           |
|           ------          |          ------          |
|  [`scala.SerialVersionUID`](http://www.scala-lang.org/api/2.9.1/scala/SerialVersionUID.html)   |  [`serialVersionUID`](http://java.sun.com/j2se/1.5.0/docs/api/java/io/Serializable.html#navbar_bottom) (field)  |
|  [`scala.cloneable`](http://www.scala-lang.org/api/2.9.1/scala/cloneable.html)   |  [`java.lang.Cloneable`](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Cloneable.html) |
|  [`scala.deprecated`](http://www.scala-lang.org/api/2.9.1/scala/deprecated.html)   |  [`java.lang.Deprecated`](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Deprecated.html) |
|  [`scala.inline`](http://www.scala-lang.org/api/2.9.1/scala/inline.html) (2.6.0 以上)  |  該当なし |
|  [`scala.native`](http://www.scala-lang.org/api/2.9.1/scala/native.html) (2.6.0 以上)  |  [`native`](http://java.sun.com/docs/books/tutorial/java/nutsandbolts/_keywords.html) (キーワード) |
|  [`scala.remote`](http://www.scala-lang.org/api/2.9.1/scala/remote.html) |  [`java.rmi.Remote`](http://java.sun.com/j2se/1.5.0/docs/api/java/rmi/Remote.html) |
|  [`scala.serializable`](http://www.scala-lang.org/api/2.9.1/index.html#scala.annotation.serializable) |  [`java.io.Serializable`](http://java.sun.com/j2se/1.5.0/docs/api/java/io/Serializable.html) |
|  [`scala.throws`](http://www.scala-lang.org/api/2.9.1/scala/throws.html) |  [`throws`](http://java.sun.com/docs/books/tutorial/java/nutsandbolts/_keywords.html) (キーワード) |
|  [`scala.transient`](http://www.scala-lang.org/api/2.9.1/scala/transient.html) |  [`transient`](http://java.sun.com/docs/books/tutorial/java/nutsandbolts/_keywords.html) (キーワード) |
|  [`scala.unchecked`](http://www.scala-lang.org/api/2.9.1/scala/unchecked.html) (2.4.0 以上) |  該当なし |
|  [`scala.volatile`](http://www.scala-lang.org/api/2.9.1/scala/volatile.html) |  [`volatile`](http://java.sun.com/docs/books/tutorial/java/nutsandbolts/_keywords.html) (キーワード) |
|  [`scala.reflect.BeanProperty`](http://www.scala-lang.org/api/2.9.1/scala/reflect/BeanProperty.html) |  [`Design pattern`](http://docs.oracle.com/javase/tutorial/javabeans/writing/properties.html) |

次の例では、 Java の main プログラムの中で、発生した例外を捕捉するため `read` メソッドの定義に `throws` アノテーションを追加しています。

> Java のコンパイラは、メソッドやコンストラクタの実行によってどのチェック例外が発生しうるかを分析し、プログラムが[チェック例外](http://docs.oracle.com/javase/specs/jls/se5.0/html/exceptions.html)ハンドラを実装しているか確認します。 発生しうるそれぞれのチェック例外について、メソッドやコンストラクタの **throws** 節では例外クラスまたはそのスーパークラスを捕捉する必要があります。
> Scala にはチェック例外がないので、 Java のコードが Scala で発生した例外を捕捉できるように、メソッドに `throws` アノテーションを記述する必要があります。

    package examples
    import java.io._
    class Reader(fname: String) {
      private val in = new BufferedReader(new FileReader(fname)) @throws(classOf[IOException])
      def read() = in.read()
    }

次の Java プログラムは `main` メソッドの第1引数に渡された名前のファイルの中身を標準出力に書き出します。

    package test;
    import examples.Reader;  // Scala class !!
    public class AnnotaTest {
        public static void main(String[] args) {
            try {
                Reader in = new Reader(args[0]);
                int c;
                while ((c = in.read()) != -1) {
                    System.out.print((char) c);
                }
            } catch (java.io.IOException e) {
                System.out.println(e.getMessage());
            }
        }
    }

Reader クラスの `throws` アノテーションをコメントアウトすると、 Java のメインプログラムをコンパイルする際に次のエラーメッセージが出力されます。:

    Main.java:11: exception java.io.IOException is never thrown in body of
    corresponding try statement
            } catch (java.io.IOException e) {
              ^
    1 error

### Java アノテーション ###

**注意:** Java アノテーションに `-target:jvm-1.5` オプションを使用していることを確認してください。

Java 1.5 は[アノテーション](http://java.sun.com/j2se/1.5.0/docs/guide/language/annotations.html)の形でユーザ定義メタデータを導入しました。 アノテーションの重要な特徴は、名前と値のペアの設定に基づいて要素を初期化することです。 あるクラスのソースを追跡するためのアノテーションが必要な場合は、アノテーションを次のように定義します。

    @interface Source {
      public String URL();
      public String mail();
    }

そして定義したアノテーションを次のように適用します。

    @Source(URL = "http://coders.com/",
            mail = "support@coders.com")
    public class MyClass extends HisClass ...

Scala でのアノテーション適用は、 名前付き引数を用いて Java アノテーションをインスタンス化するため、コンストラクタを実行するように見えます。:

    @Source(URL = "http://coders.com/",
            mail = "support@coders.com")
    class MyScalaClass ...

もしアノテーションが(デフォルト値を持たない)ただ1つの要素を必要とするだけなら、この構文はとてもうんざりする書き方です。 そこで、規約によって、もし `value` として定められたものがあれば、アノテーションをコンストラクタに似た構文で Java へ適用できるようになっています。:

    @interface SourceURL {
        public String value();
        public String mail() default "";
    }

上のアノテーションは次のように適用します。

    @SourceURL("http://coders.com/")
    public class MyClass extends HisClass ...

このケースもまた、 Scala では同じように記述します。

    @SourceURL("http://coders.com/")
    class MyScalaClass ...

`mail` 要素はデフォルト値が指定されているため、特別に指定する必要はありません。 しかし、 Java で `mail` 要素を指定する必要が出てきたときは、 `value` 要素と `mail` 要素を別々に指定する必要があり、2通りの書き方を混在させることはできません。:

    @SourceURL(value = "http://coders.com/",
               mail = "support@coders.com")
    public class MyClass extends HisClass ...

一方 Scala は Java よりも柔軟に書くことができます。

    @SourceURL("http://coders.com/",
               mail = "support@coders.com")
        class MyScalaClass ...

拡張されたこの構文は .Net のアノテーションでも同じであり、アノテーションの機能を最大限引き出します。





