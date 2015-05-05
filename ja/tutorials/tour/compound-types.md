---
layout: tutorial
title: Compound Types

disqus: true

tutorial: scala-tour
num: 6
tutorial-next: sequence-comprehensions
tutorial-previous: case-classes
---

時に、オブジェクトが他の複数の型のサブタイプであることの表現が必要になります。 Scala ではオブジェクト型の論理積である *複合型* を使って表現することができます。
Sometimes it is necessary to express that the type of an object is a subtype of several other types. In Scala this can be expressed with the help of *compound types*, which are intersections of object types.

2つのトレイト `Cloneable` と `Resetable` があるとします。:

    trait Cloneable extends java.lang.Cloneable {
      override def clone(): Cloneable = { 
        super.clone().asInstanceOf[Cloneable]
      }
    }
    trait Resetable {
      def reset: Unit
    }

ここで、オブジェクトを引数にとり、元のオブジェクトをリセットする `cloneAndReset` 関数の定義をしてみます。:
Now suppose we want to write a function `cloneAndReset` which takes an object, clones it and resets the original object:

    def cloneAndReset(obj: ?): Cloneable = {
      val cloned = obj.clone()
      obj.reset
      cloned
    }

引数 `obj` の型は何でしょうか。 もし `Cloneable` だとすればオブジェクトはクローンできますがリセットできません。 もし `Resetable` だとすればリセットはできますがクローンできません。 そのような状況での型キャストを避けるために、 `obj` 型を `Cloneable` でもあり `Resetable` でもあると指定することができます。 この複合型は Scala では `Cloneable with Resetable` と書かれます。
The question arises what the type of the parameter `obj` is. If it's `Cloneable` then the object can be `clone`d, but not `reset`; if it's `Resetable` we can `reset` it, but there is no `clone` operation. To avoid type casts in such a situation, we can specify the type of `obj` to be both `Cloneable` and `Resetable`. This compound type is written like this in Scala: `Cloneable with Resetable`.

先ほどの関数を複合型を使って書き直すと下のようになります。:

    def cloneAndReset(obj: Cloneable with Resetable): Cloneable = {
      //...
    }

複合型はいくつかのオブジェクト型から生成され、それらのオブジェクト型はひとつのリファインメントを持ちます。 リファインメントは存在するオブジェクトメンバのシグネチャを制限するのに使えます。
Compound types can consist of several object types and they may have a single refinement which can be used to narrow the signature of existing object members.
一般的な書き方は次のようになります。: `A with B with C ... { refinement }`

リファインメントの使用例は[抽象型](abstract-types.html)のページに記載があります。
An example for the use of refinements is given on the page about [abstract types](abstract-types.html). 
