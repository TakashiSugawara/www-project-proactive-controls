---

layout: col-document
tags: OWASP Top Ten Proactive Controls 2018, C2
document: OWASP Top Ten Proactive Controls 2018
order: 6

---

# C2: セキュリティフレームワークやライブラリの活用

## 概要

セキュアコーディングライブラリや、セキュリティが組み込まれたソフトウェアフレームワークは、ソフトウェア開発者をセキュリティに関した設計や実装不備から守ってくれます。ゼロからアプリケーションを書く開発者には、セキュリティ概念を適切に実装したり維持したりするのに必要な十分な知識、時間、予算を持ち合わせていないかもしれません。セキュリティフレームワークの活用は、セキュリティの目標をより効率的、正確に達成する助けとなります。

## 実装のベストプラクティス

サードパーティのライブラリやフレームワークをソフトウェアに組み込む際には、以下のベストプラクティスを考慮に入れる事が重要です。

1. 多くのアプリケーションで使われ、活発にメンテナンスされている、信頼できるソースからのライブラリやフレームワークを使用する。
2. 全てのサードパーティライブラリの全カタログを作成し、メンテナンスする。
3. ライブラリやコンポーネントを積極的に最新に保つ。[OWASP Dependency Check](https://www.owasp.org/index.php/OWASP_Dependency_Check) や [Retire.JS](https://retirejs.github.io/retire.js/) の様なツールを用いて、プロジェクトの依存性の識別や、全てのサードパーティのコードに既知の脆弱性があるか確認して下さい。
4. ライブラリをカプセル化し、ソフトウェアに必要な動作のみを公開する事で、攻撃を減じて下さい。

## 本対策で防げる脆弱性

セキュアなフレームワークやライブラリは、Webアプリケーションにおける幅広い脆弱性を防ぐのに役立ちます。これらのフレームワークやライブラリを最新に保つことは、[Top Ten 2017 risks](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project) にて 既知の脆弱性を持つコンポーネントの利用 の項目で説明される通り、非常に重要です。

## ツール
* [OWASP Dependency Check](https://www.owasp.org/index.php/OWASP_Dependency_Check) - identifies project dependencies and checks for publicly disclosed vulnerabilities
* [Retire.JS](http://retirejs.github.io/retire.js/) scanner for JavaScript libraries
