# NOC・ICAC・RCACの確認

## そもそもNOCとは

NOC(Node Operational Certificate)とは、Fabric内でノードを一意に識別し、認証するための証明書。

NOCははRCAC(Root CA Certificate:認証局によって発行されたルート証明書)→ICAC(Intermediate CA Certificate:中間証明書)→NOCの順番に発行されている。

デバイスがNOC保存時に署名検証する際は発行された順番とは逆にNOC→ICAC→RCACの順番で確認する。

## RCACの確認
### RCACを探す
docs/commissioning.mdのコミッショニング（詳細ver.）のログから、RCACValue (231)下の以下のようなログをコピー。
```text
RCACValue (231) =
[1749039898.301] [63681:63683] [DMG] {
FT~
[1749039898.301] [63681:63683] [DMG] }
```

コピーしたRCACValueのbase64をデコードして保存（ここではrcac.cert）。
```bash
echo "FT~" | base64 -d > rcac.cert
```

### MatterSDKのchip-certツールを使って読める形に変換
```bash
chip-cert print-cert rcac.cert
```
以下のような証明書が出力される。
```text
CHIP Certificate:
    Signature Algo  : ECDSAWithSHA256
    Issuer          : [[ MatterRCACId = 0000000000000001 ]]
    Not Before      : 0x27812280  ( 2021/01/01 00:00:00 )
    Not After       : 0x3A4D2580  ( 2030/12/30 00:00:00 )
    Subject         : [[ MatterRCACId = 0000000000000001 ]]
    Public Key Algo : ECPublicKey
    Curve Id        : prime256v1
    Public Key      : 
    Extensions:
        Is CA            : true
        Key Usage        : KeyCertSign CRLSign 
        Subject Key Id   : 
        Authority Key Id : 
    Signature       : 
~
```

## ICACの確認
### ICACを探す
docs/commissioning.mdのコミッショニング（詳細ver.）のログから、ICACValue (231)下の以下のようなログをコピー。
```text
ICACValue (231) =
[1749039898.751] [63681:63683] [DMG] {
FT~
[1749039898.751] [63681:63683] [DMG] }
```

コピーしたICACValueのbase64をデコードして保存（ここではicac.cert）。
```bash
echo "FT~" | base64 -d > icac.cert
```

### MatterSDKのchip-certツールを使って読める形に変換
```bash
chip-cert print-cert rcac.cert
```
以下のような証明書が出力される。
```text
CHIP Certificate:
    Signature Algo  : ECDSAWithSHA256
    Issuer          : [[ MatterRCACId = 0000000000000001 ]]
    Not Before      : 0x27812280  ( 2021/01/01 00:00:00 )
    Not After       : 0x3A4D2580  ( 2030/12/30 00:00:00 )
    Subject         : [[ MatterICACId = 0000000000000002 ]]
    Public Key Algo : ECPublicKey
    Curve Id        : prime256v1
    Public Key      : 
    Extensions:
        Is CA            : true
        Key Usage        : KeyCertSign CRLSign 
        Subject Key Id   :  
        Authority Key Id : 
    Signature       : 
~
```

## NOCの確認
### NOCを探す
docs/commissioning.mdのコミッショニング（詳細ver.）のログから、NOCValue (241) =下の以下のようなログをコピー。
```text
NOCValue (241) =
[1749039898.751] [63681:63683] [DMG] {
FT~
[1749039898.751] [63681:63683] [DMG] }
```

コピーしたICACValueのbase64をデコードして保存（ここではnoc.cert）。
```bash
echo "FT~" | base64 -d > noc.cert
```

### MatterSDKのchip-certツールを使って読める形に変換
```bash
chip-cert print-cert noc.cert
```
以下のような証明書が出力される。
```text
CHIP Certificate:
    Signature Algo  : ECDSAWithSHA256
    Issuer          : [[ MatterICACId = 0000000000000002 ]]
    Not Before      : 0x27812280  ( 2021/01/01 00:00:00 )
    Not After       : 0x3A4D2580  ( 2030/12/30 00:00:00 )
    Subject         : [[ MatterFabricId = 0000000000000001,
                         MatterNodeId = 0000000000000010 ]]
    Public Key Algo : ECPublicKey
    Curve Id        : prime256v1
    Public Key      : 0
    Extensions:
        Is CA            : false
        Key Usage        : DigitalSignature 
        Key Purpose      : ServerAuth ClientAuth 
        Subject Key Id   : 
        Authority Key Id :  
    Signature       : 
~
```

## NOC、ICAC、RCACの検証
chip-certで検証チェーンを確認するのは以下。
```bash
chip-cert validate-cert noc.cert \
 -c icac.cert \
 -t rcac.cert
```
