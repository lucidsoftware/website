---
out: Hello.html
---

  [Basic-Def]: Basic-Def.html
  [Setup]: Setup.html

Hello, World
------------

このページは、君が[sbt をインストール][Setup]したことを前提にする。

### ソースコードの入ったプロジェクトディレクトリを作る

一つのソースファイルを含むディレクトリでも、一応有効な sbt プロジェクトとなりうる。試しに、`hello`
ディレクトリを作って、以下の内容の `hw.scala` というファイルを作成する:

```scala
object Hi {
  def main(args: Array[String]) = println("Hi!")
}
```

次に `hello` ディレクトリ内から sbt を起動して 
sbt のインタラクティブコンソールに `run` と打ち込む。
Linux か OS X を使っていならばコマンドは以下のようになる:


```
\$ mkdir hello
\$ cd hello
\$ echo 'object Hi { def main(args: Array[String]) = println("Hi!") }' > hw.scala
\$ sbt
...
> run
...
Hi!
```

この例では、sbt は純粋に convention（デフォルトの慣例）だけを使って動作している。
sbt は以下を自動的に検知する:

 - ベースディレクトリ内のソース
 - `src/main/scala` か `src/main/java` 内のソース
 - `src/test/scala` か `src/test/java` 内のテスト
 - `src/main/resources` か `src/test/resources` 内のデータファイル
 - `lib` 内の jar ファイル

デフォルトでは、sbt は sbt 自身が使っている Scala のバージョンを使ってプロジェクトをビルドする。

`sbt run` を用いてプロジェクトを実行したり、`sbt console` を用いて [Scala REPL](http://www.scala-lang.org/node/2097) に入ることができる。`sbt console` は君のプロジェクトにクラスパスを通すから、
君のプロジェクトのコードを使った Scala の例をライブで試すことができる。

### ビルド定義

ほとんどのプロジェクトは何らかの手動設定が必要だ。基本的なビルド設定は `build.sbt` というファイルに書かれ、
プロジェクトのベースディレクトリ (base directory) に置かれる。

例えば、君のプロジェクトが `hello` ディレクトリにあるなら、`hello/build.sbt` をこんな感じで書く:

```scala
lazy val root = (project in file(".")).
  settings(
    name := "hello",
    version := "1.0",
    scalaVersion := "$example_scala_version$"
  )
```

[.sbt ビルド定義][Basic-Def]で、`build.sbt` の書き方をもっと詳しく説明する。

君のプロジェクトを jar ファイルにパッケージ化する予定なら、最低でも `build.sbt` に name と version は書いておこう。

### sbt バージョンの設定

`hello/project/build.properties` というファイルを作ることで、特定のバージョンの sbt を強制することができる。
このファイルに、以下のように書く:

```
sbt.version=$app_version$
```

sbt はリリース間で 99% ソースの互換性を持たせてある。
だけど、sbt バージョンを `project/build.properties` に設定することで混乱を予防することできる。
