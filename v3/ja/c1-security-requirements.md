---

layout: col-document
tags: OWASP Top Ten Proactive Controls 2018, C1
document: OWASP Top Ten Proactive Controls 2018
order: 5

---

# C1: セキュリティ要件を定義する

## 概要

セキュリティ要件とは、多くの異なるセキュリティ属性のうち一つを満たす保証のために必要なセキュリティ機能についての記述です。セキュリティ要件は業界標準、適用される法律、過去の脆弱性の歴史から作られました。セキュリティ要件は、特定のセキュリティ課題の解決や潜在的な脆弱性の排除のための、新機能や追加機能を定めます。

セキュリティ要件は、アプリケーションの検証済みセキュリティ機能のための基礎となります。全てのアプリケーションに個別の対策を講じるのではなく標準的なセキュリティ要件を使用することで、開発者はセキュリティ対策とベストプラクティスの定義を再利用できます。等しく吟味されたセキュリティ要件は過去に発生したセキュリティ問題の解決策となります。これらの要件は過去のセキュリティ上の失敗を繰り返さない為にあります。

### The OWASP ASVS

[The OWASP アプリケーションセキュリティ検証基準 (ASVS)](https://www.owasp.org/index.php/Category:OWASP_Application_Security_Verification_Standard_Project)は、利用可能なセキュリティ要件と検証基準のカタログです。OWASP ASVSは開発チームにとって詳細なセキュリティ要件の情報源になります。

セキュリティ要件は、高次に順序づけられたセキュリティ機能に基づいて分類されています。例えば、ASVSには認証、アクセス制御、エラー処理/ロギング、Webサービスなどのカテゴリーがあります。
各カテゴリーは、要件の検証となる命令文として起された当該カテゴリーのベストプラクティスの集合を含んでいます。

### ユーザーストーリーと誤用事例から要件を拡張する

ASVSの要件はその検証となる命令文の基礎であり、ユーザーストーリや誤用事例を基に拡張が可能です。ユーザーストーリーや誤用事例の利点は、システムがユーザーになにをするかではなく、ユーザーや攻撃者がシステムに何をするかが正確にアプリケーションと結びついていることです。

ここでASVS 3.0.1の要件を拡張する例をあげます。ASVS 3.0.1の"認証の検証要件"項の、要件2.19ではデフォルトパスワードに焦点を当てています。

> 2.19 フレームワークやその他コンポーネントのデフォルトパスワード(例えば"admin/password")がアプリケーションにない事を確認しなさい。

この要件にはデフォルトパスワードが存在しないことを確認するアクションと、アプリケーションでデフォルトパスワードを使用しないガイダンス、を共に含んでいます。

ユーザーストーリーは、システムのユーザー、管理者、攻撃者からの視点に注目し、ユーザーがシステムに何をして欲しいかに基づいて機能を説明します。ユーザーストーリーは「ユーザーは、xと、y、zを行えます」という形をとります。

    As a user, I can enter my username and password to gain access to the application.
    As a user, I can enter a long password that has a maximum of 1023 characters.

When the story is focused on the attacker and their actions, it is referred to as a misuse case.

    As an attacker, I can enter in a default username and password to gain access.
    

This story contains the same message as the traditional requirement from ASVS, with additional user or attacker details to help make the requirement more testable.

## Implementation
Successful use of security requirements involves four steps. The process includes discovering / selecting, documenting, implementing, and then confirming correct implementation of new security features and functionality within an application. 

### Discovery and Selection
The process begins with discovery and selection of security requirements. In this phase, the developer is understanding security requirements from a standard source such as ASVS and choosing which requirements to include for a given release of an application. The point of discovery and selection is to choose a manageable number of security requirements for this release or sprint, and then continue to iterate for each sprint, adding more security functionality over time.

### Investigation and Documentation
During investigation and documentation, the developer reviews the existing application against the new set of security requirements to determine whether the application currently meets the requirement or if some development is required. This investigation culminates in the documentation of the results of the review.

### Implementation and Test
After the need is determined for development, the developer must now modify the application in some way to add the new functionality or eliminate an insecure option. In this phase the developer first determines the design required to address the requirement, and then completes the code changes to meet the requirement. Test cases should be created to confirm the existence of the new functionality or disprove the existence of a previously insecure option.

## Vulnerabilities Prevented
Security requirements define the security functionality of an application. Better security built in from the beginning of an applications life cycle results in the prevention of many types of vulnerabilities. 

## References
* [OWASP Application Security Verification Standard (ASVS)](https://www.owasp.org/index.php/Category:OWASP_Application_Security_Verification_Standard_Project)
* [OWASP Mobile Application Security Verification Standard (MASVS)](https://github.com/OWASP/owasp-masvs)
* [OWASP Top Ten](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
