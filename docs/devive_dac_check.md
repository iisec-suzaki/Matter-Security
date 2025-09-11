# PAA・PAI・DACの確認

## そもそもDACとは

DAC(Device Attestation Certificate)とは、デバイスの真正性を証明する証明書のこと。製造時にデバイスに埋め込まれている。

DACはPAA(Product Attestation Authority:CSAが承認・登録するルート証明書)→PAI(Product Attestation Intermediate:ベンダーが発行する中間証明書)→DACの順番に発行されている。

コミッショニング時にコントローラが署名検証する際は発行された順番とは逆にDAC→PAI→PAAの順番で確認する。

## PAAの確認

### PAAを探す
例えばベンダー××のPAA証明書を取得したい場合、以下のコマンドでファイル名に「××」が含まれるPAA証明書だけを探す。
```bash
ls ../../credentials/production/paa-root-certs/ | grep -i ××
```

この場合、以下のような結果になる。
```bash
dcld_mirror_CN_××_Matter_PAA_vid_vendorid.der
dcld_mirror_CN_××_Matter_PAA_vid_vendorid.pem
```

### 見つけたPEM形式の証明書をOpenSSLで確認する
```bash
openssl x509 -in  ../../credentials/production/paa-root-certs/dcld_mirror_CN_××_Matter_PAA_vid_vendorid.pem -subject
```
以下ををpem形式で名前をつけて保存（ここではcertificatePAA.pem）。
```text
-----BEGIN CERTIFICATE-----
MI・・・・・・・・・・・・
-----END CERTIFICATE-----
```


### PEM形式のPAA証明書を読める形に変換
```bash
openssl x509 -text -noout -in ~/certificatePAA.pem
```
以下のような証明書が出力される。
```text
Certificate:
	Data:
    	Version: 3 (0x2)
    	Serial Number: △△△△△ (〇〇〇)
    	Signature Algorithm: ecdsa-with-SHA256
    	Issuer: CN = ×× Matter PAA, ******** = vendorid
    	Validity
        	Not Before: Nov  3 14:23:43 2022 GMT
        	Not After : Dec 31 23:59:59 9999 GMT
    	Subject: CN = ×× Matter PAA, ******** = vendorid
~
```

## PAIの確認
docs/commissioning.mdのコミッショニング（詳細ver.）のログから、DAC/PAI (471)下の以下のようなログをpem形式でコピーして保存（ここではcertificatePAI.pem）。
```text
-----BEGIN CERTIFICATE-----
MI・・・・・・・・・・・・
-----END CERTIFICATE-----
```

### 見つけたPEM形式のPAI証明書を読める形に変換
```bash
openssl x509 -text -noout -in ~/certificatePAI.pem
```
以下のような証明書が出力される。
```text
Certificate:
	Data:
    	Version: 3 (0x2)
    	Serial Number: △△△△△ (〇〇〇)
    	Signature Algorithm: ecdsa-with-SHA256
    	Issuer: CN = ×× Matter PAA, ******** = vendorid
    	Validity
        	Not Before: Nov  3 14:23:43 2022 GMT
        	Not After : Dec 31 23:59:59 9999 GMT
    	Subject: CN = ×× Matter PAI, ******** = vendorid
~
```

## DACの確認
docs/commissioning.mdのコミッショニング（詳細ver.）のログから、DAC/PAI (488)下の以下のようなログをpem形式でコピーして保存（ここではcertificateDAC.pem）。

```text
-----BEGIN CERTIFICATE-----
MI・・・・・・・・・・・・
-----END CERTIFICATE-----
```

### 見つけたPEM形式のPAI証明書を読める形に変換
```bash
openssl x509 -text -noout -in ~/certificateDAC.pem
```
以下のような証明書が出力される。
```text
Certificate:
	Data:
    	Version: 3 (0x2)
    	Serial Number: △△△△△ (〇〇〇)
    	Signature Algorithm: ecdsa-with-SHA256
    	Issuer: CN = ×× Matter PAI, ******** = vendorid
    	Validity
        	Not Before: Nov  3 14:23:43 2022 GMT
        	Not After : Dec 31 23:59:59 9999 GMT
    	Subject: CN = ×× Matter DAC, ******** = vendorid
~
```

## PAA、PAI、DACの検証
opensslで検証チェーンを確認するのやり方は以下。
```bash
openssl verify \
  -CAfile certificatePAA.pem \
  -untrusted certificatePAI.pem \
  certificateDAC.pem
```
