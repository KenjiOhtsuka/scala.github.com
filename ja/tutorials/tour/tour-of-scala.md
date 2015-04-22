---
layout: tutorial
title: イントロダクション

disqus: true

tutorial: scala-tour
num: 1
tutorial-next: 抽象型
---

Scala は近代的なマルチパラダイムプログラミング言語で、よく知られたプログラミングパターンを簡潔に、エレガントに、そしてタイプセーフな形で表現できるように設計されています。 Scala は柔軟にオブジェクト指向言語と関数型言語の機能を統合しています。

## Scala はオブジェクト指向 ##
Scala は[すべての値がオブジェクト](unified-types.html)という意味で、純粋なオブジェクト指向の言語です。 オブジェクトの型と振る舞いは[クラス](classes.html)及びトレイト(traits.html)として記述されます。 クラスはサブクラスの作成と、多重継承のクリーンな置き換えとしての柔軟な[ミックスインクラス合成](mixin-class-composition.html)機構によって格調されます。

## Scala は関数型 ##
Scala は[すべての関数が値](unified-types.html)という意味で、関数型言語でもあります。 Scala は無名関数の定義のために[ライトシンタックス](anonymous-function-syntax.html)を提供しており、無名関数は[高階関数](higher-order-functions.html)や[関数のネスト](nested-functions.html)、や[カリー化](currying.html)もサポートしています。 Scala の[ケースクラス](case-classes.html)とビルトインでサポートされたパターンマッチングモデルの代数的データ型は多くの関数型プログラミング言語で採用されています。

さらに、 Scala のパターンマッチングの概念は[右無視シーケンスパターン](regular-expression-patterns.html)を加えることで[XMLのデータ処理](xml-processing.html)へ自然に拡張されます。 パターンマッチングにおける[シーケンス内包表記](sequence-comprehensions.html)はクエリの設計において有用です。 これらの機能により、 Scala は Webサービスのようなアプリケーション開発にとって理想的なものとなっています。

## Scala は静的型付け言語 ##
Scala は、安全で一貫した抽象化がなされることを静的に強制する、表現力豊かな型システムを備えています。 特に Scala の型システムは次の項目をサポートしています。
* [ジェネリッククラス](generic-classes.html)
* [変位指定アノテーション](variances.html)
* [上限](upper-type-bounds.html)と[下限](lower-type-bounds.html)の型境界
* [内部クラス](inner-classes.html) and [抽象型](abstract-types.html) as object members
* [複合型](compound-types.html)
* [明示的に型付けされた自己参照](explicitly-typed-self-references.html)
* [ビュー](views.html)
* [多相的メソッド](polymorphic-methods.html)

[ローカル型推論機構](local-type-inference.html)のおかげで、ユーザは冗長な型情報の注釈を付ける必要がありません。 これらの機能が組み合わさって、抽象化の安全な再利用とソフトウェアのタイプセーフ拡張のための強力な基盤が提供されます。

## Scala は拡張可能 ##

実際問題として、多くの場合、ドメイン固有言語の開発ではドメイン固有の言語拡張が多くの場合に必要になります。 Scala では独特な言語機構の組み合わせにより、新しい言語要素をライブラリの形で簡単に追加できるようになっています。:
* すべてのメソッドは[中置または後置演算子](operators.html)として使えます。
* [期待される型に応じてクロージャが自動的に生成されます](automatic-closures.html) (ターゲットによる型付け)。

これらの機能を組み合わせることで、拡張した構文やマクロのようなメタプログラミング機能を使うことなく、新しい構文を簡単に定義できます。

Scala は Java や .Net と相互運用できる
Scala は人気の Java 2 実行環境 (JRE) とうまく相互運用できるように設計されています。 特に、主流のオブジェクト指向Javaプログラミング言語との相互作用はできるだけスムーズになっています。 Scala は Java と同じコンパイルモデル (分割コンパイル、動的クラスローディング) を備えており、既存の高品質な多数のライブラリも利用可能です。 .Net フレームワーク (CLR) もサポートしています。

次のページへお進みください。
