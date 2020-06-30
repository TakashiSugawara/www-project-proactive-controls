---

layout: col-document
tags: OWASP Top Ten Proactive Controls 2018, C6
document: OWASP Top Ten Proactive Controls 2018
order: 10

---

# C6: デジタルアイデンティティの実装

## 概要

デジタルアイデンティティ(デジタルID)とは、オンライントランザクションに従事するユーザー(やその他の対象物)についての固有の表現です。認証とは、個人や実体が主張通りであるかの確認プロセスです。セッション管理とは、サーバーがユーザー認証の状態を維持することで、ユーザーが再認証なしにシステムの継続利用を可能とするプロセスです。[NIST Special Publication 800-63B: Digital Identity Guidelines (Authentication and Lifecycle Management](https://pages.nist.gov/800-63-3/sp800-63b.html) (NIST 800-63b) は、デジタルID、認証、セッション管理らの実装における確固たるガイダンスです。

次に安全な実装のための推奨を紹介します。

## 認証レベル

NIST 800-63bには、認証保証レベル(AAL)と呼ばれる認証保証の3つのレベルが記されています。AALレベル1では個人を特定できる情報(Personally Identifiable Information = PII)やその他のプライベート情報を含まない低リスクのアプリケーション用に当てられています。AALレベル1では、一般的にはパスワードによる単一要素認証しか要求されません。

### レベル 1 : パスワード

パスワードは本当に重要です。ポリシーが、安全な保管が、時にはユーザーによるリセットが、必要となります。

#### パスワード要件

パスワードは少なくとも次の要件を満たさねばなりません。

* 多要素認証(MFA)やその他の管理がある際には少なくとも8文字以上の長さとします。MFAが使えない場合には10文字以上に増やしましょう。
* 保存する鍵には空白を含む全ての印字可能アスキー文字を使えるようしましょう。
* 長いパスワードやパスフレーズを推奨します。
* 複雑性に付いての要件は効果が限定的であることが知られているため、含めてはなりません。代替としてはMFAや、より長いパスワード長を推奨します。
* パスワードが過去の事件で既に漏洩した良く知られているパスワードでない事を確認します。漏洩したパスワードリストのよく使われるパスワード上位1,000あるいは10,000のうち、先述のパスワード長を満たすものをブロックするのが良いです。最も頻繁に使われているパスワードのリストは次のリンクから得られます。<https://github.com/danielmiessler/SecLists/tree/master/Passwords>

#### 安全なパスワード復旧機構の実装

アプリケーションの良くある機構に、ユーザーがパスワードを忘れた際でもアカウントにアクセスできるようする仕組みがあります。パスワード復旧機能の良い設計ワークフローとして、MFAが使えます。例えば知識要素たるセキュリティ質問をし、生成されたトークンを所持要素たるデバイスに送信します。

[Forgot_Password_Cheat_Sheet](https://www.owasp.org/index.php/Forgot_Password_Cheat_Sheet) を見て、 [Choosing_and_Using_Security_Questions_Cheat_Sheet](https://www.owasp.org/index.php/Choosing_and_Using_Security_Questions_Cheat_Sheet) にて更なる詳細を確認します。

#### 安全なパスワード保存の実装

強力な認証管理の提供のためには、アプリケーションはユーザークレデンシャルを安全に保管せねばなりません。加えて、仮にクレデンシャル(例えばパスワード)が漏洩したとしても直ちにこの情報が攻撃者に渡らぬように、暗号化による制御をしましょう。

**パスワード保存のPHPにおける例**

次のPHPの`password_hash()`(5.5.0から利用可能)を利用した安全なパスワードハッシュの例です。この関数はデフォルトでbcryptアルゴリズムを利用します。作業計数は15を使っています。

```php
    <?php
        $cost = 15;
        $password_hash = password_hash("secret_password", PASSWORD_DEFAULT, ["cost" => $cost] ); 
    ?>
```

詳細については [OWASP Password Storage Cheat Sheet](https://www.owasp.org/index.php/Password_Storage_Cheat_Sheet) をご覧下さい。

### レベル 2 : 多要素認証(MFA)

NIST 800-63b AALレベル2は、「自己申告されたPIIやオンラインで利用可能なその他の個人情報」を含む高リスクアプリケーション用に当てられています。AALレベル2では、ワンタイムパスワード(OTP)やその他の形式の多要素認証が必要となります。

MFAは、ユーザーが自身の主張通りの人物である識別のために、次の組み合わせを用いて確認します。

* 知識要素 - パスワードや個人識別番号(Personal Identification Number = PIN)
* 所持要素 - トークンデバイスや電話
* 生体要素 - 指紋などの生体情報

単独の要素として利用されるパスワードのセキュリティは脆弱です。多要素認証では攻撃者がサービスでの認証の際に複数の要素を求めるため、強固なソリューションとなります。

生体情報の注意点として、認証の単一要素としての利用下では、デジタル認証における秘密情報としては許容されないことです。これらはオンラインからであったり、カメラ付き携帯電話での撮影(顔画像など)だったり、人が触った物からの採取(残留指紋など)だったり、高解像度画像(光彩模様など)だったり、から当人の自覚無く入手可能です。生体情報は物理的な認証装置(所有要素)における多要素認証の一要素としてのみしか、利用できません。例えば、ユーザーがMFA用OTP生成器に手動入力して、デバイスにアクセスできるかを確認します。

### レベル 3 : 暗号ベース認証

NIST 800-63b AALレベル3は漏洩したシステムが個人への危害、高額の金銭的損失、公益の毀損、あるいは民事・刑事上の違反を招く恐れのある場合に当てられています。AALレベル3は「暗号化プロトコルを介する鍵の所有証明に基づく」認証が必要です。このタイプの認証は、最強の認証保証の実現のために、使われています。一般的にはハードウェア暗号化モジュールにより、これを実現します。

#### セッション管理

一度ユーザー認証した後は、アプリケーションは一定の時間制限の下で認証状態を追跡し、維持する様にもできます。これにより、ユーザーはリクエストの度に再認証すること無く、アプリケーションを継続利用できます。このユーザー認証の追跡のことをセッション管理と呼びます。

#### Session Generation and Expiration
User state is tracked in a session. This session is typically stored on the server for traditional web based session management. A session identifier is then given to the user so the user can identify which server-side session contains the correct user data. The client only needs to maintain this session identifier, which also keeps sensitive server-side session data off of the client.

Here are a few controls to consider when building or implementing session management solutions:

* Ensure that the session id is long, unique and random.
* The application should generate a new session or at least rotate the session id during authentication and re-authentication.
* The application should implement an idle timeout after a period of inactivity and an absolute maximum lifetime for each session, after which users must re-authenticate. The length of the timeouts should be inversely proportional with the value of the data protected.

Please see the [Session Management Cheat Sheet](https://www.owasp.org/index.php/Session_Management_Cheat_Sheet) further details. ASVS Section 3 covers additional session management requirements.

#### Browser Cookies
Browser cookies are a common method for web application to store session identifiers for web applications implementing standard session management techniques. Here are some defenses to consider when using browser cookies.

* When browser cookies are used as the mechanism for tracking the session of an authenticated user,  these should be accessible to a minimum set of domains and paths and should be tagged to expire at, or soon after, the session's validity period.
* The 'secure' flag should be set to ensure the transfer is done via secure channel only (TLS).
* HttpOnly flag should be set to prevent the cookie from  being accessed via JavaScript.
* Adding "[samesite](https://www.owasp.org/index.php/SameSite)" attributes to cookies prevents some [modern browsers](https://caniuse.com/#search=samesite) from sending cookies with cross-site requests and provides protection against cross-site request forgery and information leakage attacks.

#### Tokens
Server-side sessions can be limiting for some forms of authentication. "Stateless services" allow for client side management of session data for performance purposes so they server has less of a burden to store and verify user session. These "stateless" applications generate a short-lived access token which can be used to authenticate a client request without sending the user's credentials after the initial authentication.

#### JWT (JSON Web Tokens)
JSON Web Token (JWT) is an open standard ([RFC 7519](https://tools.ietf.org/html/rfc7519) ) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed. A JWT token is created during authentication and is verified by the server (or servers) before any processing.

However, JWT's are often not saved by the server after initial creation. JWT's are typically created and then handed to a client without being saved by the server in any way. The integrity of the token is maintained through the use of digital signatures so a server can later verify that the JWT is still valid and was not tampered with since its creation.

This approach is both stateless and portable in the way that client and server technologies can be different yet still interact.

**Caution** 

Digital identity, authentication and session management are very big topics. We're scratching the surface of the topic of Digital Identity here. Ensure that your most capable engineering talent is responsible for maintaining the complexity involved with most Identity solutions.

## Vulnerabilities Prevented
* [OWASP Top 10 2017 A2- Broken Authentication and Session Management](https://www.owasp.org/index.php/Top_10-2017_A2-Broken_Authentication)
* [OWASP Mobile Top 10 2014-M5- Poor Authorization and Authentication](https://www.owasp.org/index.php/Mobile_Top_10_2014-M5)

## References
* [OWASP Cheat Sheet: Authentication](https://www.owasp.org/index.php/Authentication_Cheat_Sheet)
* [OWASP Cheat Sheet: Password Storage](https://www.owasp.org/index.php/Password_Storage_Cheat_Sheet)
* [OWASP Cheat Sheet: Forgot Password](https://www.owasp.org/index.php/Password_Storage_Cheat_Sheet)
* [OWASP Cheat Sheet: Choosing and Using Security Questions](https://www.owasp.org/index.php/Choosing_and_Using_Security_Questions_Cheat_Sheet)
* [OWASP Cheat Sheet: Session Management](https://www.owasp.org/index.php/Session_Management_Cheat_Sheet)
* [OWASP Cheat Sheet: IOS Developer](https://www.owasp.org/index.php/IOS_Developer_Cheat_Sheet)
* [OWASP Testing Guide: Testing for Authentication](https://www.owasp.org/index.php/Testing_for_authentication)
* [NIST Special Publication 800-63 Revision 3 - Digital Identity Guidelines](https://pages.nist.gov/800-63-3/sp800-63-3.html)

## Tools
* Daniel Miessler: [Most commonly found passwords](https://github.com/danielmiessler/SecLists/tree/master/Passwords)
