[The Key Agreement Schemes Validation System (KASVS)](http://csrc.nist.gov/groups/STM/cavp/key-establishment.html)

# 1 Introduction

このドキュメントは、鍵合意スキームの実装を検証する手順を規定する。
これには、SP 800-56Aで規定されている鍵確認なし及び鍵確認ありの鍵合意スキームを実装したIUT(Implementations Under Tests)のためのテストが包含されている。
KASVSは、IUT上で自動的にテストを実行できるような形で設計されている。

加えて、KASVSは、SP800-56Aが規定していないKDFを実装しているIUTのテストも提供している。
このために、CAVPは、"KDFを除いたSP800-56Aの全て"のテストと呼ばれるSP(special publication)を提供している。
このテストは、実装ガイドラインD.2において使用が認可されているKDF(これはSP800-56Aでは規定されていないAcceptable Key Establishment Protocolsと呼ばれるKDF)を使用しているIUT用である。

テストにはFunctionテストとValidityテストという2つのカテゴリがある。
Requirementと管理手順に続き、IUTの検証の探索を示す。
Requirementは、IUTとKASVSの間のデータ通信に関する規定と、IUTがPASSしなければならない検証テストの詳細と、KASVSの使用方法を記述する。


# 2 Scope

このドキュメントは、鍵確認なし及び鍵確認ありの鍵合意スキームに適合したSP800-56A実装を検証するためのテストと、"KDFを除いたSP800-56Aの全て"の実装を検証するためのテストを規定する。
このテストをIUTに適用するとき、KASVSは、鍵合意スキーム及び(もし適用可能であれば)鍵確認規定の実装の正しさを確認するためのテストを提供する。
IUTの実装の正しさの確認は、アルゴリズムレベルでアドレス可能な"shall"の文章で識別されるテスト要件と、規定によって識別される要件の両方を包んでいる。

Recommendation(NIST 800-56A)で詳述されているように、DLCはFFCとECCを含んでいる。
これらの暗号それぞれに対して分離された検証テストスイートが設計されている。
これらの検証テストスイートは、各鍵合意スキームに対する検証テストを含んでいる。
検証テストは、IUTがRecommendation(NIST 800-56A)の規定に従って鍵合意スキームのコンポーネントを実装していることを検証する。
これらのコンポーネントは、DLCプリミティブ(共有秘密Z)の計算や、KDFを通してのDKMの計算を含んでいる。
もし鍵確認がサポートされている場合、検証テストスイートは、IUTがRecommendation(NIST 800-56A)の規定通りに鍵確認のコンポーネントを実装していることを検証する。
これは、DKMの解析と、MacDataの生成と、MacTagの計算を含んでいる。
アルゴリズムレベルでアドレス可能かつ"shall"の文章で識別された要件は、付録Bにリストされている検証テストによってテストされる。

もしIUTが、SP800-56Aで規定されていないKDFを実装している場合、"KDFを除いたSP800-56Aの全て"のコンポーネントテストがテストされる。

KASVSの検証プロセスは、実装に含まれている暗号機能の中で、SP800-56Aによって使用されているがSP(special publication)では定義されていない暗号機能について、その定義を要求する。
これらの機能は、なんの検証テストが必要とされるかをKASVSが決定するための情報として、KASVSに提供する。
加えて、これらの機能は、保障の獲得を助けるために使用される。(しかしこれはCAVPのスコープ外である)

鍵確認なしの検証テストは、MACアルゴリズムにテストを実行することを求めることに注意してください。
これは検証されたIUTを得るための必要条件ではない。
同様に、もし"KDFを除いたSP800-56Aの全て"がテストされる場合、テスト要件は共有秘密ZZがハッシュされることを要求する。
ハッシュ関数はテストを実行するためだけに使用され、検証されたIUTを得るための必要条件ではない。


# 6 Key Agreement Scheme Validation System (KASVS) Test

KASVSは鍵合意と鍵確認のプロセスの実装がSP800-56Aと一致していることをテストする。


## 6.2 The Function Test

... 


### 6.2.3 Testing of the 800-56A Processing through Shared Secret Computation (ZZ) for IUTs that do not use a KDF specified in 800-56A

分割されたファイルが、各鍵合意スキームと役割の組合せに対して生成される。
例えば、もしIUTがdhHybrid1鍵合意スキームをサポートしており、IUTがinitiatorとresponderの両方をサポートしている場合、2つのファイルが生成される。

KASFunctionTest_FFCHybrid1_NOKC_ZZOnly_init.req と、
KASFunctionTest_FFCHybrid1_NOKC_ ZZOnly_resp.req.

各リクエストファイル内に、...
これに加えて、もしFFCが使用される場合、ドメインパラメーター(p,g)の1セットが、10セットのデータで使用するために含まれている。
...
テストされるスキームに応じて、このデータのセットは、静的な公開鍵あるいは一時的な公開鍵を含んでいる。

IUTは、ドメインパラメーター値かNIST-approvedな曲線を、公開/秘密鍵ペアの生成に使用する。
IUTは、KASVSが供給する公開鍵と、自身の公開/秘密鍵ペアを使用して、共有秘密Zを計算する。
Zは、テストされるスキームに応じたDLCを使用して計算される。(NIST SP800-56Aの5.7セクション)
IUTは、Zを平文で出力すべきでないので、Zはハッシュされる。
このハッシュ実装はテスト用途のみで使用される。

IUTが生成した値は、SAMPLEファイルで規定された書式のRESPONSEファイルに保存される。

...


## 6.3 The Validity Test

CAVSツールから受信した、鍵確認なしまたは鍵確認ありの鍵合意プロセスによって生成された結果の有効、無効をIUTが理解する能力をテストすることが目的である。


### 6.3.3 Testing of the 800-56A Processing through Shared Secret Computation (ZZ) for IUTs that do not use a KDF specified in 800-56A

...


