# 鍵交換
* **Diffie-Hellman鍵共有**
	* [PKCS#3](http://japan.emc.com/emc-plus/rsa-labs/standards-initiatives/pkcs-3-diffie-hellman-key-agreement-standar.htm) | [Japanese(9. Object identifierのみ)](pkcs-3.asc)
	* ANSI X9.42
		* PKCS#3より新しいDH法
		* X9.42対応Opensslは未リリース (2016/5/21時点)
			* [note OpenSSL does not support ANSI X9.42 in the released versions - support is available in the as yet unreleased 1.0.2 and 1.1.0](https://wiki.openssl.org/index.php/Diffie_Hellman)
	* [RFC2631](https://tools.ietf.org/html/rfc2631) | [Japanese(未)](rfc2631.txt)
		* X9.42に基づくDH法
	* [NIST Special Publication 800-56A](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-56Ar2.pdf)

# IPsec (IKE)

* **IKEv1**
	* [RFC2409](https://tools.ietf.org/html/rfc2409) (Obsoleted by RFC4306:IKEv2)
		* IPsec(IKE)に4つのDHグループ(2つのMODPグループと2つのEC2Nグループからなる4つのOakleyグループ)を定義。
* **IKE用の(MODP)DHグループ**
	* [RFC3526](https://tools.ietf.org/html/rfc3526)
		* IPsec(IKE)に6つのDHグループ(MODP)を追加定義。
* **IKEv1とIKEv2用のECPグループ**
	* [RFC4753](https://tools.ietf.org/html/rfc4753) (Obsoleted by RFC5903)
		* IPsec(IKE)に3つのDHグループ(ECP)を追加定義。
	* [RFC5903](https://tools.ietf.org/html/rfc5903)
* **IKEv2**
	* RFC4306 (Obsoleted by RFC5996)
	* RFC5996 (Obsoleted by RFC7296) 
	* RFC7296
* **IANA: Internet Key Exchange Version 2 (IKEv2) Parameters** [link](http://www.iana.org/assignments/ikev2-parameters/ikev2-parameters.xhtml)
	* [Transform Type 4 - Diffie-Hellman Group Transform IDs](http://www.iana.org/assignments/ikev2-parameters/ikev2-parameters.xhtml#ikev2-parameters-8)

# TLS

* **TLS1.0**
	* [RFC2246](https://tools.ietf.org/html/rfc2246) (Obsoleted by RFC4346:TLS1.1)
* **TLS1.1**
	* [RFC4346](https://tools.ietf.org/html/rfc4346) (Obsoleted by RFC5246:TLS1.2)
* **TLS用の楕円曲線暗号サイファースイート**
	* [RFC4492](https://tools.ietf.org/html/rfc4492) | [Japanese](rfc4492.txt)
		* TLS1.0/1.1でのECDHの使用。TLS1.2[RFC5246]でUpdateされた。
* **SMIME,SSH,TLS,IKEなどのIETF標準用のDiffie-Hellmanグループ追加**
	* [RFC5114](https://tools.ietf.org/html/rfc5114) | [Japanese](rfc5114.txt)
		* DH鍵を用いるITEFプロトコルで使用可能な8つのDHグループ（3つのMODPグループ、5つのECPグループ）のパラメーターとテストデータに関する記述。
		* 3つのMODPグループはOpensslでサポート予定 (2016/5/21時点)
			* [RFC 5114 defines 3 standard sets of parameters for use with Diffie-Hellman (OpenSSL will have built-in support for these parameters from OpenSSL 1.0.2 - not yet released)](https://wiki.openssl.org/index.php/Diffie_Hellman)
* **TLS1.2**
	* [RFC5246](https://tools.ietf.org/html/rfc5246) | [Japanese(セッション再開部分のみ)](rfc5246.txt)
* **TLSとDTLSの安全な利用に関する推奨事項**
	* [RFC7525](https://tools.ietf.org/html/rfc7525) | [Japanese](rfc7525.txt)
		* TLS1.2以前に対する推奨事項。本RFCはTLS1.3でUpdateされるだろう。
* **Negotiated Finite Field Diffie-Hellman Ephemeral Parameters for TLS**
	* [draft-ietf-tls-negotiated-ff-dhe-10](https://www.ietf.org/id/draft-ietf-tls-negotiated-ff-dhe-10.txt)
* **IANA: Transport Layer Security (TLS) Parameters** [link](http://www.iana.org/assignments/tls-parameters/tls-parameters.xhtml)
	* [Supported Groups Registry](http://www.iana.org/assignments/tls-parameters/tls-parameters.xhtml#tls-parameters-8)

# PKI（公開鍵基盤）

* **インターネットX.509 PKI証明書とCRLのプロファイルのためのアルゴリズムと識別子**
	* RFC3279
* **インターネットX.509 PKI証明書とCRLのプロファイル**
	* RFC5280
* **X.509 PKI用の新しいASN.1モジュール