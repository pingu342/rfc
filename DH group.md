# DHの安全な利用に関する情報

## 概要

DHの脆弱性を突く攻撃は3つ

* small subgroups攻撃
* replay攻撃
* MITM攻撃

small-subgroups攻撃を回避するには

* 安全な素数であること ([FFDHE-TLS]で定義されているDHパラメーターは安全な素数だが、サーバーが独自のDHパラメーターをクライアントに提示する可能性もあり、その場合はクライアントがパラメーターをチェックする必要があるがその方法は...?)
* 秘密鍵を再利用しないこと (DHEでも秘密鍵を再利用する実装が存在)
* 相手のDH公開鍵をチェックすること ([FFDHE-TLS])

replay攻撃を回避するには

* [FFDHE-TLS]で定義済みのDHグループを使うこと
* 他を使うならクライアントがチェックすること (チェック方法は...?)
* チェックできないならDHE以外の暗号スイートを使うこと

MTIM攻撃を回避するには

* クライアントはDHパラメータのサイズをチェックして短いものは受け入れないこと ([FFDHE-TLS]では1024bit以上必須)


## DHパラメーター

RFC2409, RFC3526でIKE用に定義され、[IANA](http://www.iana.org/assignments/ikev2-parameters/ikev2-parameters.xhtml#ikev2-parameters-8)に登録されている768～8192bitのMODP Groupが8つ。

RFC5114でTLSやIKE用に定義され、[IANA](http://www.iana.org/assignments/ikev2-parameters/ikev2-parameters.xhtml#ikev2-parameters-8)に登録されているMODP Group with xxx-bit Prime Order Subgroupが3つ。

RFC2409, RFC3526の8つのDHパラメーターは安全な素数。しかしFFDHE-TLSでは採用されなかった。

RFC5114の3つのDHパラメーターは、X9.42スタイルで、これには安全な素数でないものもある。




## RFC7525: TLSとDTLSの安全な利用に関する推奨事項

>4.3.  Public Key Length
>   With a key exchange based on modular exponential (MODP) Diffie-
>   Hellman groups ("DHE" cipher suites), DH key lengths of at least 2048
>   bits are RECOMMENDED.
>
><span style="color:red">
>   **冪剰余 (MODP) DHグループ ("DHE"サイファースイート) に基づく鍵交換では、DH鍵長は少なくとも2048ビットを推奨する。**
></span>

>4.4.  Modular Exponential vs. Elliptic Curve DH Cipher Suites
>   ... several implementations do not perform appropriate
>   validation of group parameters and are vulnerable to attacks
>   referenced in Section 2.9 of [RFC7457]
>
><span style="color:red">
>   **幾つかの実装はグループパラメータの検証を適切に行っておらず、[RFC7547]セクション2.9の攻撃に対して脆弱である。**
></span>

>6.4.  Diffie-Hellman Exponent Reuse
>   o  TLS implementations that reuse exponents should test the DH public
>      key they receive for group membership, in order to avoid some
>      known attacks.  These tests are not standardized in TLS at the
>      time of writing.  See [RFC6989] for recipient tests required of
>      IKEv2 implementations that reuse DH exponents.
>
>  <span style="color:red">**指数を再利用するTLS実装は、既知の攻撃を回避するために、受け取ったDH公開鍵を試験すべきである。→後述のsmall subgroups攻撃???**</span>
>
>  これを書いている時点では、これらのテストはTLSで標準化されていない。DH鍵を再利用するIKEv2実装に求められるテストのレシピは [RFC6989] を見よ。

## RFC7457: TLS及びDTLSにおける既知の攻撃

>2.9.  Diffie-Hellman Parameters
>   TLS allows the definition of ephemeral Diffie-Hellman (DH) and
>   Elliptic Curve Diffie-Hellman parameters in its respective key
>   exchange modes.  This results in an attack detailed in
>   [Cross-Protocol].  Using predefined DH groups, as proposed in
>   [FFDHE-TLS], would mitigate this attack.
>
>   TLSは短命なDH及びECDHパラメーターを許している。
>   このことは**<span style="color:red">[Cross-Protocol]の攻撃をもたらす。
>   [FFDHE-TLS]の定義済みDHグループを使用することはこの攻撃を軽減する。→後述のReplay攻撃</span>**
>
>   In addition, clients that do not properly verify the received
>   parameters are exposed to man-in-the-middle (MITM) attacks.
>   Unfortunately, the TLS protocol does not mandate this verification
>   (see [RFC6989] for analogous information for IPsec).
>
>**<span style="color:red">
>   加えて、受信パラメーターを正しく検証しないクライアントはMITM攻撃に晒される。
>   残念ながらTLSではこの検証は必須でない。→後述のMITM攻撃**</span>
>   (IPSecに対する類似の情報は[RFC6989]を参照)


## FFDHE-TLS: Negotiated Finite Field Diffie-Hellman Ephemeral Parameters for Transport Layer Security (TLS)

moduloに安全な素数を使ったグループパラメータを定義。

	ffdhe2048
	ffdhe3072
	ffdhe4096
	ffdhe6144
	ffdhe8192

上記グループのどれを使うか、TLSクライアントとサーバーが交渉できるようにClientHello拡張を修正。


## RFC6989: Additional Diffie-Hellman Tests for the Internet Key Exchange Protocol Version 2 (IKEv2)

>2.1.  Sophie Germain Prime MODP Groups
>
>   pが安全な素数、つまり(p-1)/2もまた素数であるDHグループ。
>
>   RFC7296: 768-bit MODP Group, 1024-bit MODP Group
>
>   RFC3526: 1536-bit MODP Group, 2048-bit MODP Group, 3072-bit MODP Group, 4096-bit MODP Group, 6144-bit MODP Group, 8192-bit MODP Group)
>
>   ピアの公開鍵rが`(1 < r < p-1)`の範囲内であることを検証
>
>2.2.  MODP Groups with Small Subgroups
>
>   [RFC5114]で定義されている、小サブグループを持つ冪剰余グループ
>
>   DH秘密鍵を再利用しない && ピアの公開鍵rが`(1 < r < p-1)`の範囲内であることをチェック


## Small subgroup攻撃

### [2016年1月28日に公表されたOpenSSLの脆弱性について](https://www.cellos-consortium.org/jp/index.php?Key_Recovery_Attack_on_DH_small_subgroups_20160130J)

> 本脆弱性は、DHで使う素数のパラメータの中に安全（safe）でないものが含まれていたことが原因であり、攻撃者は安全でない素数(non-safe prime)を利用してprivate DH exponent（秘密鍵に相当）を入手できます。
> この攻撃法では、例えば、TLSのサーバが ephemeral DH ciphersuites を利用して private DH exponent を使い回すか、static DH ciphersuiteを利用している場合に、サーバの private DH exponentを入手できます。

### [DH small subgroups (CVE-2016-0701)](https://www.openssl.org/news/secadv/20160128.txt)

>  これまでOpenSSLは安全な素数に基づくDHパラメーターのみ生成している。
>  v1.0.2ではRFC5114で必須となるX9.42スタイルのパラメーターファイルの生成をサポートした。
>  そのようなファイルで使用される素数は安全でないかもしれない。
>  アプリケーションが、安全でない素数に基づくパラメーターでDHを使用した場合、攻撃者はこの事実を利用してピアの秘密鍵を探しだせる。
>  この攻撃では攻撃者は、ピアが同じ秘密鍵を使用するハンドシェイクを何回か行う必要がある。
>  例えば、秘密鍵が再利用されていたり、静的なDHサイファースイートが使われている場合、TLSサーバーの秘密鍵を発見することが可能である。
>
>  攻撃を検知するには、サブグループのサイズ"q"が有効かどうかをチェックする。
>  "q"が有効ならX9.42スタイルである


## Replay攻撃

### RFC7919 Negotiated FFDHE for TLS

>   [SECURE-RESUMPTION], [CROSS-PROTOCOL], and [SSL3-ANALYSIS] all show a
>   malicious peer using a bad FFDHE group to maneuver a client into
>   selecting a premaster secret of the peer's choice, which can be
>   replayed to another server using a non-FFDHE key exchange and can
>   then be bootstrapped to replay client authentication.
>
>   [SECURE-RESUMPTION],[CROSS-PROTOCOL],[SSL3-ANALYSIS]
>   悪意のあるピアが、悪いFFDHEグループを使い、悪意のあるピアが選んだ
>   プリマスターシークレットをクライアントに選ばせる。
>
>   To prevent this attack (barring the session hash fix documented in
>   [RFC7627]), a client would need not only to implement this document,
>   but also to reject non-negotiated FFDHE cipher suites whose group
>   structure it cannot afford to verify.  Such a client would need to
>   abort the initial handshake and reconnect to the server in question
>   without listing any FFDHE cipher suites on the subsequent connection.
>
>   この攻撃([RFC7627]に記載されている...)を防ぐには、クライアントはこの
>   ドキュメントを実装するだけでなく、クライアントが確認する余裕のない
>   グループ構造を持ったnon-negotiated FFDHEサイファースイートをリジェクト
>   する必要がある。そのようなクライアントは、最初のハンドシェークを
>   アボートさせ、FFDHEサイファースイートをリストから除き、問題になっている
>   サーバーに再接続する必要があるだろう。


## MITM攻撃

### [Diffie-Hellman（DH）鍵交換の脆弱性を使ったTLSプロトコルに対する攻撃(Logjam)](http://d.hatena.ne.jp/Kango/20150521/1432219012)

> 以下の条件が成立する場合、通信内容の盗聴や改ざんの影響を受ける可能性がある。
> 
> * 接続元・接続先の通信間に第三者の介入が可能である。
> * 接続先サーバーが輸出グレード(512bit)、あるいはそれ相当のDHパラメータを生成し、使用している。
> * 接続元クライアントでDHパラメータの短いDH鍵交換を受け入れるソフトウェアを使用している。


## その他

** Cross-Protocol: [A Cross-Protocol Attack on the TLS Protocol](https://securewww.esat.kuleuven.be/cosic/publications/article-2216.pdf) **

