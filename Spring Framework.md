# Spring Framework

#Spring

  

2004年ver1.0始動

定型作業を取り除いて、ビジネスロジックの実装に集中するために開発された

出来ることを横並びに増やすため、様々な機能が存在する

例えるならば「フレームワークの集合体」

Spring Frameworkには「機能の使い分けが困難」といったデメリットがある→Spring Bootが登場

  

[公式ドキュメント](https://spring.pleiades.io/spring-framework/reference/overview.html "https://spring.pleiades.io/spring-framework/reference/overview.html")  

  

## 基本設計

  

共通インターフェースを用意し、開発者にはそのインターフェースを呼び出させる

実装部分はDIコンテナ側で切り替える

  

## コア機能

  

### DI：依存性注入

  

オブジェクトのインスタンス管理の仕組み

DIコンテナと呼ばれる基盤にコンポーネント=Bean（簡単に言うとクラス）を登録し、プログラムをではそれを抽象化したインターフェースを呼び出すことで、実態・実装=継承した処理を自由に切り替えることが出来る

これを行うことで、1つのインスタンスを再利用してくれる

また、実装を気にせずテストを行うことが出来る

注意点として、インスタンス変数（クラス変数）を積極的に利用しないこと

スコープの管理もしやすくなる

  

### AOP

  

共通処理を抽出・分離してまとめること。ログや認証処理など

  

### Validation

  

`@~` アノテーションで処理を簡単に記述できる

  

### Message  

  

メッセージ（文字列）を外部ファイルで定義して、利用できる

  

## データアクセス機能

  

### Transaction

  

DBアクセスによるデータ・処理の矛盾の発生に対して、整合性を保ってくれる

`@Transactional`⁠でトランザクション制御を行う

  

### DAO

  

DBへのアクセスを行うクラスのこと。技術ではなくデザインパターンの１つ

  

### ORM

  

データベースのテーブルとJavaのオブジェクト（クラス）をマッピングする技術。各DBに沿ったSQLを直接書かなくてもDBの操作を可能にしてくれる

（SELECT文を書くのではなく、SELECTメソッドを利用するイメージ）

ORMフレームワークは通常、複数のデータベースに対応しており、異なるデータベースへの移行が容易

  

### JDBC

  

Javaプログラム上でDBへのアクセスコードを実装・統一させる技術

標準API

インターフェースの集まり

低レベルでデータベース操作を行うため、SQLクエリや接続管理などを開発者が直接記述する必要がある

データベースの特定の機能や方言（SQL方言）に依存する場合があり、異なるデータベースへの移行が難しいことがある。

  

### JPA

  

Java標準のORM仕様

実装として、HibernateやEclipseLinkなどのフレームワークがある

  

### R2DBC

  

非同期のDBアクセスAPI

（JDBCは同期API）

Spring WebFluxなどのリアクティブなフレームワークと併用されることが多い

  

## Webフレームワーク

  

### Spring MVC

  

webアプリケーションを作成するためのフレームワーク

Spring Frameworkに最初から同梱されている

MVCの中でも**Front Controller(フロントコントローラ)パターン**に分類

![](Files/image%204.png)  

[引用元](https://qiita.com/t-shin0hara/items/687085ec34ae78ca2260 "https://qiita.com/t-shin0hara/items/687085ec34ae78ca2260")  

  

Servletは「プログラムの一部」であり、Tomcatはそれを実行する「環境」

  

Servlet

- JavaのWebアプリケーションでHTTPリクエストを処理するために使われる技術
- Javaのクラスとして実装され、サーバー上でリクエスト（GET、POSTなど）を処理し、レスポンスを生成するために使用される

  

Tomcat

- Java ServletやJSPなどを実行するためのサーバー、つまり「Servletコンテナ」
- Servletをホストし、クライアントからのHTTPリクエストをServletに転送し、そのレスポンスをクライアントに返す
- Tomcat自体はフル機能のアプリケーションサーバーではなく、主にServlet/JSPを扱う軽量なWebサーバー

  

### Spring WebFlux

  

従来のWebアプリではなく、リアルタイムに発生するストリームデータを非同期に処理するアプリを作成するためのフレームワーク（リアクティブスタックWebアプリケーション、Reactive Streams API）

同時接続数が多いアプリケーションの作成においてリソースを効率的に使用できる

1つのスレッドで複数のリクエストを処理する

  

![](Files/image%205.png)  

![](Files/image%206.png)  

[引用元](https://news.mynavi.jp/techplus/article/techp5260/ "https://news.mynavi.jp/techplus/article/techp5260/")  

  

| 特徴  | ブロッキング（Spring MVC） | ノンブロッキング（Spring WebFlux） |
| --- | --- | --- |
| リクエスト処理モデル | ブロッキングI/O | ノンブロッキングI/O |
| スレッド数 | リクエストごとにスレッドが必要 | 少数のスレッドで複数リクエストを処理 |
| リソース使用 | 高負荷時にスレッドリソースが増加 | 効率的なリソース使用 |
| パフォーマンス | 高負荷時にスレッドオーバーヘッド | 高トラフィックでも効率的 |
| プログラミングモデル | 同期処理 | 非同期処理、リアクティブプログラミング |

  

Spring WebFluxでは、Java上で非同期プログラミングを行うため、デフォルトの選択肢として**Reactor**というライブラリがある（他にもライブラリがあるが、Reactorがメジャーみたい）

| 特徴  | Reactor | RxJava | Akka Streams | CompletableFuture | Vert.x |
| --- | --- | --- | --- | --- | --- |
| データ型 | Mono, Flux | Observable, Single | Source, Flow | CompletableFuture | Future, Promise |
| 統合性 | Springに最適化 | Springとも統合可能 | 分散システム向け | Java標準 | 軽量フレームワーク |
| Backpressureサポート | あり  | あり  | あり  | なし  | あり  |
| 主な用途 | Webアプリケーション、非同期処理 | リアクティブプログラミング全般 | 分散システム、メッセージ駆動 | シンプルな非同期タスク | 非同期I/O |

  

## インテグレーション

  

### JMS

  

IBM MQなどで、非同期にメッセージを送受信できる

Springでは、IBM MQのようなメッセージ指向ミドルウェア（MOM）と呼ばれるメッセージングシステムにアクセスするためのAPIであるJMSを簡単に使用できる

DB→JDBC、MQ→JMS

  

### Email

  

メール送信できる

  

### Cache

  

メソッドの実行結果やインスタンスがキャッシュされ、２回目以降処理が高速化される

  

### Scheduling

  

処理のスケジューリング・定期実行

@Scheduledを付与することで定義

  

## テスト

  

### Mock

  

[単体テストで利用するOSSライブラリの使い方](https://terasolunaorg.github.io/guideline/5.4.1.RELEASE/ja/UnitTest/ImplementsOfUnitTest/UsageOfLibraryForTest.html "https://terasolunaorg.github.io/guideline/5.4.1.RELEASE/ja/UnitTest/ImplementsOfUnitTest/UsageOfLibraryForTest.html")  

  

mockとは、テストに必要な部品の値を擬似的に設定するもの

[スタブ、モック、フェイク、ダミーの違い](https://craftsman-software.com/posts/38 "https://craftsman-software.com/posts/38")  

`Mock`を使用することで、実際の外部依存にアクセスすることなく、テスト対象のコードが正しく動作するかを確認できる

- MockMvc：Spring MVCのテスト用ツール、コントローラをモックして**HTTPリクエストをテスト**できる、実際のサーバを起動しなくて良い
- Mockito：サービス層の依存関係をモックして、実際の**依存先にアクセスせずにロジックをテスト**できる

  

### TestContext

  

統合テストやユニットテストを行う際に、テスト実行のための基盤を提供するフレームワーク

テスト対象のコードがSpringのコンポーネント（例：サービス、リポジトリ）とやり取りする必要がある場合に、`TestContext`が自動的にSpringのアプリケーションコンテキストを初期化し、必要な依存関係を注入する

  

⁠その他のテスト機能・フレームワーク

- JUnit：`TestContext`を組み合わせることで、Springアプリケーションの統合テストが可能になる
- Mockito：Mockitoは実際のコンテキストを使用せず、モックで置き換える。一方、`TestContext`は実際のSpringコンテキストでテストを行う。
- Spring Boot Test：`TestContext`の機能を拡張して、さらに簡単にSpring Bootアプリケーションのテストを行えるようにしたもの

  

### Spring MVC Test

  

[Spring Web MVCアプリケーションのテスト方法を見ていきたい（＋モックでBeanのテスト）](https://kazuhira-r.hatenablog.com/entry/2022/04/16/195051 "https://kazuhira-r.hatenablog.com/entry/2022/04/16/195051")  

  

### WebTestClient

  

リアクティブWebアプリケーション向けのテストクライアント

従来の`MockMvc`に似た役割を果たしますが、リアクティブプログラミングに特化した設計がされている

サーバーを起動せずにHTTPリクエストをシミュレートし、Web層のテストを効率的に行うことができる