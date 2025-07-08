# PAA・PAI・DACの確認
証明書発行の順番はPAA→PAI→DAC

コミッショニング時に確認するのはDAC→PAI→PAA

## PAAの確認

### PAAを探す
例えばTapoのPAA証明書を取得したい場合、以下のコマンドでファイル名に「Tapo」が含まれるPAA証明書だけを探す。
```bash
ls ../../credentials/production/paa-root-certs/ | grep -i Tapo
```

この場合、以下のような結果になる。
```bash
dcld_mirror_CN_Tapo_Matter_PAA_vid_0x1392.der
dcld_mirror_CN_Tapo_Matter_PAA_vid_0x1392.pem
```

### 見つけたPEM形式の証明書をOpenSSLで確認する
```bash
openssl x509 -in  ../../credentials/production/paa-root-certs/dcld_mirror_CN_Tapo_Matter_PAA_vid_0x1392.pem -subject
```

## PAIの確認
docs/commissioning.mdのコミッショニング（詳細ver.）のログから、DAC/PAI (471)下の以下のようなログをコピーして保存する。

```text
-----BEGIN CERTIFICATE-----
MI・・・・・・・・・・・・
-----END CERTIFICATE-----
```

## DACの確認
docs/commissioning.mdのコミッショニング（詳細ver.）のログから、DAC/PAI (488)下の以下のようなログをコピーして保存する。

```text
-----BEGIN CERTIFICATE-----
MI・・・・・・・・・・・・
-----END CERTIFICATE-----
```
