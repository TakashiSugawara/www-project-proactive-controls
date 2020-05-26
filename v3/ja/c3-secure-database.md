---

layout: col-document
tags: OWASP Top Ten Proactive Controls 2018, C2
document: OWASP Top Ten Proactive Controls 2018
order: 7

---

# C3: データベースへの安全なアクセス

## 概要

この節はリレーショナルデータベースとNoSQLの両方を含む、全てのデータストアへの安全なアクセスについての説明です。検討すべき領域は幾つかあります

1. 安全なクエリー
2. 安全な設定
3. 安全な認証
4. 安全な通信

### 安全なクエリー

SQLインジェクションは、信頼できないユーザー入力が、よくある単純な文字列結合など安全でないSQLクエリーに動的に追加される場合に発生します。SQLインジェクションはアプリケーションセキュリティリスクのなかでも最も危険なものの一つです。SQLインジェクションは簡単に悪用され、全データベースが盗まれたり、削除されたり、変更されたりしかねません。このアプリケーションはデータベースをホストするOSへ危険なコマンドを実行できる場合もあり、そうなれば攻撃者にとってネットワーク上での足ががりとなります。

SQLインジェクションの軽減には、信頼できないユーザー入力がSQLコマンドの一部として解釈されるのを防がなくてはなりません。その為の最善の方法は「クエリーパラメーター化」として知られているプログラミング技術です。この防御策はSQL、OQL、ストアドプロシジャーの構築で適用すべきです。

ASP、ColdFusion、C#、Delphi、.NET、Go、Java、Perl、PHP、PL/SQL、PostgreSQL、Python、R、Ruby、Schemeにおけるクエリーパラメータ化の例の良いリストが、[http://bobby-tables.com](http://bobby-tables.com/) や [OWASP Cheat Sheet on Query Parameterization](https://www.owasp.org/index.php/Query_Parameterization_Cheat_Sheet) で見つかるでしょう。

**クエリーパラメータ化の注意**

データベースクエリーの中には、パラメーター化できない場所があります。これらの場所はデータベースベンダー毎に異なります。データベスクエリーのパラメーターがパラメーター化クエリーにバインドできない場合には、完全一致や手動エスケープを慎重に行って下さい。また、大抵のパラメーター化クエリーはパフォーマンスに改善をもたらしますが、あるデータベース実装における特定のパラメーター化クエリーは悪影響となります。クエリーをテストしてパフォーマンスを確認して下さい。特に、広範囲のlike節やテキスト検索機能のように、複雑なクエリーは要注意です。

### 安全な設定

残念なことに、データベース管理システム(DBMS)は「デフォルトで安全」という設定で出荷されている訳ではありません。DBMSとホスティングプラットフォームから利用可能なセキュリティコントロールが有効になっており、適切に設定されていることを確認するために、注意して下さい。殆どの標準的なDBMSでは標準、ガイド、ベンチマークがよういされています。

安全な設定のための簡単ガイダンスが[Database Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Database_Security_Cheat_Sheet.html#database-configuration-and-hardening)にあります。

### 安全な認証

データベースへの全てのアクセスを適切に認証しましょう。DBMSへの認証を安全な方法で行いましょう。安全なチャンネルのみを通じて認証しましょう。資格情報は適切に保護され、使用可能な状態になければなりません。

安全な認証のための簡単ガイダンスが[Database Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Database_Security_Cheat_Sheet.html#authentication)にあります。

### 安全な通信

殆どのDBMSでは様々な通信方法(サービス、API, 他)をサポートしており、安全な(認証され暗号化された)ものと、安全でない(認証されず、暗号化されてもない)ものとあります。*全ての場所でデータを保護せよ*の対策に則り、安全な通信のみを用いる事がよいです。

安全な通信のための簡単ガイダンスが[Database Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Database_Security_Cheat_Sheet.html#connecting-to-the-database)にあります。

## 本対策で防げる脆弱性

* [OWASP Top 10 2017- A1: Injection](https://www.owasp.org/index.php/Top_10-2017_A1-Injection)
* [OWASP Mobile Top 10 2014-M1 Weak Server Side Controls](https://www.owasp.org/index.php/Mobile_Top_10_2014-M1)

## 参考文献

* [OWASP Cheat Sheet: Query Parameterization](https://www.owasp.org/index.php/Query_Parameterization_Cheat_Sheet)
* [OWASP Cheat Sheet: Database Security](https://cheatsheetseries.owasp.org/cheatsheets/Database_Security_Cheat_Sheet.html)
* [Bobby Tables: A guide to preventing SQL injection](http://bobby-tables.com/)
* [CIS Database Hardening Standards](https://www.cisecurity.org/cis-benchmarks/)
