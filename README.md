# IPsec (IKE)

* RFC2409: IKEv1
	* IPsec(IKE)におけるDH用に4つのDHグループ（2つのMODPグループと2つのEC2Nグループからなる5つのOakleyグループ）を定義。

# TLS

* RFC2246: TLS1.0
* RFC4346: TLS1.1
* RFC4492: TLS用の楕円曲線暗号サイファースイート [(japanese)](rfc4492.txt)
	* TLS1.0/1.1でのECDHの使用。TLS1.2[RFC5246]でUpdateされた。
* RFC5114: SMIME,SSH,TLS,IKEなどのIETF標準用のDiffie-Hellmanグループ追加 [(japanese)](rfc5114.txt)
	* DH鍵を用いるITEFプロトコルで使用可能な8つのDHグループ（3つのMODPグループ、5つのECPグループ）のパラメーターとテストデータに関する記述。
* RFC5246: TLS1.2
* RFC7525: TLSとDTLSの安全な利用に関する推奨事項 [(japanese)](rfc7525.txt)
	* TLS1.2以前に対する推奨事項。本RFCはTLS1.3でUpdateされるだろう。
* draft-ietf-tls-negotiated-ff-dhe-10: Negotiated Finite Field Diffie-Hellman Ephemeral Parameters for TLS [(english)](https://www.ietf.org/id/draft-ietf-tls-negotiated-ff-dhe-10.txt)

# PKI（公開鍵基盤）

* RFC3279: インターネットX.509 PKI証明書とCRLのプロファイルのためのアルゴリズムと識別子
* RFC5280: インターネットX.509 PKI証明書とCRLのプロファイル