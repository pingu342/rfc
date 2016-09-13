# DH�̈��S�ȗ��p�Ɋւ�����

## �T�v

DH�̐Ǝ㐫��˂��U����3��

* small subgroups�U��
* replay�U��
* MITM�U��

small-subgroups�U�����������ɂ�

* ���S�ȑf���ł��邱�� ([FFDHE-TLS]�Œ�`����Ă���DH�p�����[�^�[�͈��S�ȑf�������A�T�[�o�[���Ǝ���DH�p�����[�^�[���N���C�A���g�ɒ񎦂���\��������A���̏ꍇ�̓N���C�A���g���p�����[�^�[���`�F�b�N����K�v�����邪���̕��@��...?)
* �閧�����ė��p���Ȃ����� (DHE�ł��閧�����ė��p�������������)
* �����DH���J�����`�F�b�N���邱�� ([FFDHE-TLS])

replay�U�����������ɂ�

* [FFDHE-TLS]�Œ�`�ς݂�DH�O���[�v���g������
* �����g���Ȃ�N���C�A���g���`�F�b�N���邱�� (�`�F�b�N���@��...?)
* �`�F�b�N�ł��Ȃ��Ȃ�DHE�ȊO�̈Í��X�C�[�g���g������

MTIM�U�����������ɂ�

* �N���C�A���g��DH�p�����[�^�̃T�C�Y���`�F�b�N���ĒZ�����͎̂󂯓���Ȃ����� ([FFDHE-TLS]�ł�1024bit�ȏ�K�{)


## DH�p�����[�^�[

RFC2409, RFC3526��IKE�p�ɒ�`����A[IANA](http://www.iana.org/assignments/ikev2-parameters/ikev2-parameters.xhtml#ikev2-parameters-8)�ɓo�^����Ă���768�`8192bit��MODP Group��8�B

RFC5114��TLS��IKE�p�ɒ�`����A[IANA](http://www.iana.org/assignments/ikev2-parameters/ikev2-parameters.xhtml#ikev2-parameters-8)�ɓo�^����Ă���MODP Group with xxx-bit Prime Order Subgroup��3�B

RFC2409, RFC3526��8��DH�p�����[�^�[�͈��S�ȑf���B������FFDHE-TLS�ł͍̗p����Ȃ������B

RFC5114��3��DH�p�����[�^�[�́AX9.42�X�^�C���ŁA����ɂ͈��S�ȑf���łȂ����̂�����B




## RFC7525: TLS��DTLS�̈��S�ȗ��p�Ɋւ��鐄������

>4.3.  Public Key Length
>   With a key exchange based on modular exponential (MODP) Diffie-
>   Hellman groups ("DHE" cipher suites), DH key lengths of at least 2048
>   bits are RECOMMENDED.
>
><span style="color:red">
>   **�p��] (MODP) DH�O���[�v ("DHE"�T�C�t�@�[�X�C�[�g) �Ɋ�Â��������ł́ADH�����͏��Ȃ��Ƃ�2048�r�b�g�𐄏�����B**
></span>

>4.4.  Modular Exponential vs. Elliptic Curve DH Cipher Suites
>   ... several implementations do not perform appropriate
>   validation of group parameters and are vulnerable to attacks
>   referenced in Section 2.9 of [RFC7457]
>
><span style="color:red">
>   **����̎����̓O���[�v�p�����[�^�̌��؂�K�؂ɍs���Ă��炸�A[RFC7547]�Z�N�V����2.9�̍U���ɑ΂��ĐƎ�ł���B**
></span>

>6.4.  Diffie-Hellman Exponent Reuse
>   o  TLS implementations that reuse exponents should test the DH public
>      key they receive for group membership, in order to avoid some
>      known attacks.  These tests are not standardized in TLS at the
>      time of writing.  See [RFC6989] for recipient tests required of
>      IKEv2 implementations that reuse DH exponents.
>
>  <span style="color:red">**�w�����ė��p����TLS�����́A���m�̍U����������邽�߂ɁA�󂯎����DH���J�����������ׂ��ł���B����q��small subgroups�U��???**</span>
>
>  ����������Ă��鎞�_�ł́A�����̃e�X�g��TLS�ŕW��������Ă��Ȃ��BDH�����ė��p����IKEv2�����ɋ��߂���e�X�g�̃��V�s�� [RFC6989] ������B

## RFC7457: TLS�y��DTLS�ɂ�������m�̍U��

>2.9.  Diffie-Hellman Parameters
>   TLS allows the definition of ephemeral Diffie-Hellman (DH) and
>   Elliptic Curve Diffie-Hellman parameters in its respective key
>   exchange modes.  This results in an attack detailed in
>   [Cross-Protocol].  Using predefined DH groups, as proposed in
>   [FFDHE-TLS], would mitigate this attack.
>
>   TLS�͒Z����DH�y��ECDH�p�����[�^�[�������Ă���B
>   ���̂��Ƃ�**<span style="color:red">[Cross-Protocol]�̍U���������炷�B
>   [FFDHE-TLS]�̒�`�ς�DH�O���[�v���g�p���邱�Ƃ͂��̍U�����y������B����q��Replay�U��</span>**
>
>   In addition, clients that do not properly verify the received
>   parameters are exposed to man-in-the-middle (MITM) attacks.
>   Unfortunately, the TLS protocol does not mandate this verification
>   (see [RFC6989] for analogous information for IPsec).
>
>**<span style="color:red">
>   �����āA��M�p�����[�^�[�𐳂������؂��Ȃ��N���C�A���g��MITM�U���ɎN�����B
>   �c�O�Ȃ���TLS�ł͂��̌��؂͕K�{�łȂ��B����q��MITM�U��**</span>
>   (IPSec�ɑ΂���ގ��̏���[RFC6989]���Q��)


## FFDHE-TLS: Negotiated Finite Field Diffie-Hellman Ephemeral Parameters for Transport Layer Security (TLS)

modulo�Ɉ��S�ȑf�����g�����O���[�v�p�����[�^���`�B

	ffdhe2048
	ffdhe3072
	ffdhe4096
	ffdhe6144
	ffdhe8192

��L�O���[�v�̂ǂ���g�����ATLS�N���C�A���g�ƃT�[�o�[�����ł���悤��ClientHello�g�����C���B


## RFC6989: Additional Diffie-Hellman Tests for the Internet Key Exchange Protocol Version 2 (IKEv2)

>2.1.  Sophie Germain Prime MODP Groups
>
>   p�����S�ȑf���A�܂�(p-1)/2���܂��f���ł���DH�O���[�v�B
>
>   RFC7296: 768-bit MODP Group, 1024-bit MODP Group
>
>   RFC3526: 1536-bit MODP Group, 2048-bit MODP Group, 3072-bit MODP Group, 4096-bit MODP Group, 6144-bit MODP Group, 8192-bit MODP Group)
>
>   �s�A�̌��J��r��`(1 < r < p-1)`�͈͓̔��ł��邱�Ƃ�����
>
>2.2.  MODP Groups with Small Subgroups
>
>   [RFC5114]�Œ�`����Ă���A���T�u�O���[�v�����p��]�O���[�v
>
>   DH�閧�����ė��p���Ȃ� && �s�A�̌��J��r��`(1 < r < p-1)`�͈͓̔��ł��邱�Ƃ��`�F�b�N


## Small subgroup�U��

### [2016�N1��28���Ɍ��\���ꂽOpenSSL�̐Ǝ㐫�ɂ���](https://www.cellos-consortium.org/jp/index.php?Key_Recovery_Attack_on_DH_small_subgroups_20160130J)

> �{�Ǝ㐫�́ADH�Ŏg���f���̃p�����[�^�̒��Ɉ��S�isafe�j�łȂ����̂��܂܂�Ă������Ƃ������ł���A�U���҂͈��S�łȂ��f��(non-safe prime)�𗘗p����private DH exponent�i�閧���ɑ����j�����ł��܂��B
> ���̍U���@�ł́A�Ⴆ�΁ATLS�̃T�[�o�� ephemeral DH ciphersuites �𗘗p���� private DH exponent ���g���񂷂��Astatic DH ciphersuite�𗘗p���Ă���ꍇ�ɁA�T�[�o�� private DH exponent�����ł��܂��B

### [DH small subgroups (CVE-2016-0701)](https://www.openssl.org/news/secadv/20160128.txt)

>  ����܂�OpenSSL�͈��S�ȑf���Ɋ�Â�DH�p�����[�^�[�̂ݐ������Ă���B
>  v1.0.2�ł�RFC5114�ŕK�{�ƂȂ�X9.42�X�^�C���̃p�����[�^�[�t�@�C���̐������T�|�[�g�����B
>  ���̂悤�ȃt�@�C���Ŏg�p�����f���͈��S�łȂ���������Ȃ��B
>  �A�v���P�[�V�������A���S�łȂ��f���Ɋ�Â��p�����[�^�[��DH���g�p�����ꍇ�A�U���҂͂��̎����𗘗p���ăs�A�̔閧����T��������B
>  ���̍U���ł͍U���҂́A�s�A�������閧�����g�p����n���h�V�F�C�N�����񂩍s���K�v������B
>  �Ⴆ�΁A�閧�����ė��p����Ă�����A�ÓI��DH�T�C�t�@�[�X�C�[�g���g���Ă���ꍇ�ATLS�T�[�o�[�̔閧���𔭌����邱�Ƃ��\�ł���B
>
>  �U�������m����ɂ́A�T�u�O���[�v�̃T�C�Y"q"���L�����ǂ������`�F�b�N����B
>  "q"���L���Ȃ�X9.42�X�^�C���ł���


## Replay�U��

### RFC7919 Negotiated FFDHE for TLS

>   [SECURE-RESUMPTION], [CROSS-PROTOCOL], and [SSL3-ANALYSIS] all show a
>   malicious peer using a bad FFDHE group to maneuver a client into
>   selecting a premaster secret of the peer's choice, which can be
>   replayed to another server using a non-FFDHE key exchange and can
>   then be bootstrapped to replay client authentication.
>
>   [SECURE-RESUMPTION],[CROSS-PROTOCOL],[SSL3-ANALYSIS]
>   ���ӂ̂���s�A���A����FFDHE�O���[�v���g���A���ӂ̂���s�A���I��
>   �v���}�X�^�[�V�[�N���b�g���N���C�A���g�ɑI�΂���B
>
>   To prevent this attack (barring the session hash fix documented in
>   [RFC7627]), a client would need not only to implement this document,
>   but also to reject non-negotiated FFDHE cipher suites whose group
>   structure it cannot afford to verify.  Such a client would need to
>   abort the initial handshake and reconnect to the server in question
>   without listing any FFDHE cipher suites on the subsequent connection.
>
>   ���̍U��([RFC7627]�ɋL�ڂ���Ă���...)��h���ɂ́A�N���C�A���g�͂���
>   �h�L�������g���������邾���łȂ��A�N���C�A���g���m�F����]�T�̂Ȃ�
>   �O���[�v�\����������non-negotiated FFDHE�T�C�t�@�[�X�C�[�g�����W�F�N�g
>   ����K�v������B���̂悤�ȃN���C�A���g�́A�ŏ��̃n���h�V�F�[�N��
>   �A�{�[�g�����AFFDHE�T�C�t�@�[�X�C�[�g�����X�g���珜���A���ɂȂ��Ă���
>   �T�[�o�[�ɍĐڑ�����K�v�����邾�낤�B


## MITM�U��

### [Diffie-Hellman�iDH�j�������̐Ǝ㐫���g����TLS�v���g�R���ɑ΂���U��(Logjam)](http://d.hatena.ne.jp/Kango/20150521/1432219012)

> �ȉ��̏�������������ꍇ�A�ʐM���e�̓����������̉e�����󂯂�\��������B
> 
> * �ڑ����E�ڑ���̒ʐM�Ԃɑ�O�҂̉�����\�ł���B
> * �ڑ���T�[�o�[���A�o�O���[�h(512bit)�A���邢�͂��ꑊ����DH�p�����[�^�𐶐����A�g�p���Ă���B
> * �ڑ����N���C�A���g��DH�p�����[�^�̒Z��DH���������󂯓����\�t�g�E�F�A���g�p���Ă���B


## ���̑�

** Cross-Protocol: [A Cross-Protocol Attack on the TLS Protocol](https://securewww.esat.kuleuven.be/cosic/publications/article-2216.pdf) **

