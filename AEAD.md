## RFC

* [RFC5116: An Interface and Algorithms for Authenticated Encryption](https://tools.ietf.org/html/rfc5116) ([txt](rfc5116.txt))

	* AEADのアルゴリズムとインターフェースを規定


* [RFC5246: The Transport Layer Security (TLS) Protocol Version 1.2](https://tools.ietf.org/html/rfc5246) ([txt](rfc5246.txt))

	* TLS1.2に認証付き暗号(AEAD: authenticated encryption with additional data)を追加

* [RFC5288: AES Galois Counter Mode (GCM) Cipher Suites for TLS](https://tools.ietf.org/html/rfc5288) ([txt](rfc5288.txt))

	* AEADの1つであるAES-GCMをTLS1.2に追加


* [RFC5289: TLS Elliptic Curve Cipher Suites with SHA-256/384 and AES Galois Counter Mode (GCM)](https://tools.ietf.org/html/rfc5289) ([txt](rfc5289.txt))

	* RFC4492で追加されたECCサイファースイートはSHA1サポートのみであった
	* RFC5289はSHA256、SHA384のECCサイファースイートと、GCMのECCサイファースイートを追加


## AES-GCMサイファースイート

| サイファースイート | RFC | TLS ver. |
|:-----------|:------------:|:------------:|
| TLS_RSA_WITH_AES_128_GCM_SHA256 | RFC5288 | TLS1.2 |
| TLS_RSA_WITH_AES_256_GCM_SHA384 | RFC5288 | TLS1.2 |
| TLS_DHE_RSA_WITH_AES_128_GCM_SHA256 | RFC5288 | TLS1.2 |
| TLS_DHE_RSA_WITH_AES_256_GCM_SHA384 | RFC5288 | TLS1.2 |
| TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 | RFC5289 | TLS1.2 |
| TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 | RFC5289 | TLS1.2 |


## AEAD

RFC5246より。

圧縮された平文`TLSCompressed.fragment`をAEADで暗号化して暗号文`TLSCiphertext.fragment`を得る。

	TLSCompressed.fragment -> AEAD-Encrypt -> GenericAEADCipher

	struct {
		ContentType type;       /* same as TLSPlaintext.type */
		ProtocolVersion version;/* same as TLSPlaintext.version */
		uint16 length;
		opaque fragment[TLSCompressed.length];
	} TLSCompressed;

	struct {
		ContentType type;
		ProtocolVersion version;
		uint16 length;
		select (SecurityParameters.cipher_type) {
			case stream: GenericStreamCipher;
			case block:  GenericBlockCipher;
			case aead:   GenericAEADCipher;
		} fragment;
	} TLSCiphertext;

	struct {
	   opaque nonce_explicit[SecurityParameters.record_iv_length];
	   aead-ciphered struct {
		   opaque content[TLSCompressed.length];
	   };
	} GenericAEADCipher;

AEAD暗号は入力として鍵、ノンス、平文、additional_dataを受け取る。

	AEADEncrypted = AEAD-Encrypt(write_key, nonce, plaintext, additional_data)

### 鍵

key_blockから得られるclient_write_key, server_write_keyのどちらかを使う。

### ノンス

ノンスを得る方法、GenericAEADCipher.nonce_explicitの長さは、AEADの種類ごとに定義。(AES-GCMはAEADの一種)

多くの場合でRFC5116のimplicitノンスが使用される。

この場合、key_blockから得られるclient_write_IV, server_write_IVをimplicitノンスとして使う。

### key_block

	key_block = PRF(SecurityParameters.master_secret,
					"key expansion",
					SecurityParameters.server_random +
					SecurityParameters.client_random);

	client_write_MAC_key[SecurityParameters.mac_key_length] -> AEADでは使わない
	server_write_MAC_key[SecurityParameters.mac_key_length] -> AEADでは使わない
	client_write_key[SecurityParameters.enc_key_length]     -> AEAD鍵
	server_write_key[SecurityParameters.enc_key_length]     -> AEAD鍵
	client_write_IV[SecurityParameters.fixed_iv_length]     -> AEAD implicitノンス(非公開ノンス)
	server_write_IV[SecurityParameters.fixed_iv_length]     -> AEAD implicitノンス(非公開ノンス)

	PRF(secret, label, seed) = P_<hash>(secret, label + seed)

	P_hash(secret, seed) = HMAC_hash(secret, A(1) + seed) +
						   HMAC_hash(secret, A(2) + seed) +
						   HMAC_hash(secret, A(3) + seed) + ...
						   
	A(0) = seed
	A(i) = HMAC_hash(secret, A(i-1))

P_<hash>はTLS1.2ではP_SHA256

### additional_data

	additional_data = seq_num + TLSCompressed.type + TLSCompressed.version + TLSCompressed.length;

`+`は連結を意味する。

seq_num=8バイト, type=1バイト, version=2バイト, length=2バイト

よってadditional_data=13バイト


## AES-GCM

### ノンス

	struct {
	   opaque salt[4];           //implicitノンス (key_blockから取得)
	   opaque nonce_explicit[8]; //explicitノンス (64bitシーケンス番号)
	} GCMNonce;

### SecurityParameters

	SecurityParameters.fixed_iv_length = 4
	SecurityParameters.record_iv_length = 8

### key_block

	client_write_key  -> AES-GCM鍵
	server_write_key  -> AES-GCM鍵
	client_write_IV   -> GCMNonce.salt[4]
	server_write_IV   -> GCMNonce.salt[4]

P_<hash>はP_SHA256またはP_SHA384

