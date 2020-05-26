---

layout: col-document
tags: OWASP Top Ten Proactive Controls 2018, C4
document: OWASP Top Ten Proactive Controls 2018
order: 8

---

# C4: データのエンコードとエスケープ

## 概要

**エンコーディング**とエスケープは、インジェクション攻撃を阻止するための防御技術です。エンコーディング(通常「外部エンコーディング」と呼ばれるもの)は、特殊な文字を出力先インタプリターにとって危険でないが、同等となる別の形に変換します。例えば、HTMLページにおいて``<``文字を``&lt;``という文字列への変換です。**エスケープ**は、文字や文字列の前に特殊な文字を追加することによって誤解を防ぎます。例えば、``"``(ダブルクォート)文字の前に``\``を追加することで、文字列の終了ではなくテキストとして理解させる事です。

出力エンコーディングは、コンテンツが出力先インタープリターに渡される**直前**に適用するのが最善です。この防御策がリクエストの早い段階で適用されてしまうと、エンコーディングやエスケープがプログラムの他の箇所でのコンテンツの利用を妨げるかもしれません。例えば、データベースへの保存前にHTMLコンテンツをエスケープした際に、UIが自動的にそれを再びエスケープしてしまえば、コンテンツは二重にエスケープされ適切に表示されないでしょう。

### コンテキスト出力エンコーディング

コンテキスト出力エンコーディングは、XSSを防ぐ為に必須の重要なセキュリティプログラミング技術です。この防御策はユーザーインターフェースの構築時に、信頼できないユーザー入力を動的にHTMLに追加する直前に、出力に対して行われます。エンコーディングの種類はデータが表示や保存される位置(コンテキスト)に依存します。安全なユーザーインターフェースの構築のために用いられるエンコーディングの種類には、HTML要素エンコーディング、HTML属性エンコーディング、JavaScriptエンコーディング、URLエンコーディング等があります。

#### Javaでのコンテキスト出力エンコーディング例

コンテキスト出力エンドーディングを提供するOWASP Javaエンコーダーの例が、[OWASP Java Encoder Project Examples](https://www.owasp.org/index.php/OWASP_Java_Encoder_Project#tab=Use_the_Java_Encoder_Project)にあります。


#### .NETでのコンテキスト出力エンコーディング

デフォルトでは無効ですが、.NET 4.5以降のフレームワークの一部として、アンチXSSライブラリがあります。web.confの設定を用いる事で、アプリケーションの全体に渡ってアンチXSSエンコーダーをデフォルトとして明示して利用することができます。適用する場合、ドキュメントでのデータ位置に応じてアンチXSSライブラリの正しい関数を用いてコンテキスト出力エンコードする事が重要です。

#### PHPでのコンテキスト出力エンコーディング

**Zend Framework 2**

Zend Framework 2 (ZF2)では、``Zend\Escaper``を出力のエンコードに用います。コンテキスト出力エンコーディングの例は[Context-specific escaping with zend-escaper](https://framework.zend.com/blog/2017-05-16-zend-escaper.html)にあります。

### その他のエンコーディングとインジェクション対策

エンコーディング/エスケーピングはその他の形のインジェクションに対してコンテンツを中和することもできます。例えば、一定の特殊なメタ文字がOSコマンドに追加入力されるのを中和する事ができます。これは「OSコマンドエスケーピング」や「シェルエスケーピング」などと呼ばれています。これは「コマンドインジェクション」脆弱性を防ぎます。

他のエスケーピングにも、例えばXML属性エスケーピングが様々な形のXMLインジェクションやXMLパスインジェクションを防ぐ事ができる、LDAP識別名エスケーピングが様々な形のLDAPインジェクションを防ぐ事ができるなど、インジェクションの防止となる形式があります。

### 文字エンコーディングと平準化

ユニコードエンコーディングはマルチバイトでの文字符号化法です。入力が可能な箇所において[Unicode](https://www.owasp.org/index.php/Unicode_Encoding)を用いる事で悪意のあるコードを偽装し、様々な攻撃を可能とします。[RFC 2279](https://tools.ietf.org/html/rfc2279)にはテキストをエンコードする多くの方法が参照されています。

平準化はシステムがデータを平易で標準的な形に変換する手法です。Webアプリケーションは一般的に、全てのコンテンツが保存時や表示時に同じ文字タイプとなる保証のために、平準化を行います。

悪意のあるUnicodeやその他不正な文字表現を入力されてもアプリケーションが安全であれば、平準化関連の攻撃から安全であると言えます。

## 本対策で防げる脆弱性

* [OWASP Top 10 2017 - A1: Injection](https://www.owasp.org/index.php/Top_10-2017_A1-Injection)
* [OWASP Top 10 2017 - A7: Cross Site Scripting (XSS)](https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS))
* [OWASP Mobile_Top_10_2014-M7 Client Side Injection](https://www.owasp.org/index.php/Mobile_Top_10_2014-M7)

## 参考文献

* [XSS](https://www.owasp.org/index.php/Cross-site_Scripting_(XSS)) - General information
* [OWASP Cheat Sheet: XSS Prevention](https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet) - Stopping XSS in your web application
* [OWASP Cheat Sheet: DOM based XSS Prevention](https://www.owasp.org/index.php/DOM_based_XSS_Prevention_Cheat_Sheet)
* [OWASP Cheat Sheet: Injection Prevention](https://www.owasp.org/index.php/Injection_Prevention_Cheat_Sheet)

## ツール

* [OWASP Java Encoder Project](https://www.owasp.org/index.php/OWASP_Java_Encoder_Project)
* [AntiXSSEncoder](https://docs.microsoft.com/en-us/dotnet/api/system.web.security.antixss.antixssencoder?redirectedfrom=MSDN&view=netframework-4.7.2)
* [Zend\Escaper](https://framework.zend.com/blog/2017-05-16-zend-escaper.html) - examples of contextual encoding
