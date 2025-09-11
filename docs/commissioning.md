# コミッショニング手順(chip-tool)

## １．デバイスの準備
- デバイスの電源ON
- デバイスに付属している11桁の数字から構成されたMPC(Manual Pairinig Code)を確認

## ２．コントローラでペアリング

### Wi-FiとMPCの情報を入力
```bash
export SSID="SSIDの値"
export PASS="PASSの値"
export MPC="MPCの値"
```

### コミッショニング

#### コミッショニング（通常ver.）
- Node IDには任意の数字を入力（例：1）
```bash
chip-tool pairing code-wifi <Node-ID> "$SSID" "$PASS" "$MPC" \
  --paa-trust-store-path ../../credentials/production/paa-root-certs
```

#### コミッショニング（詳細ver.）
- 証明書の情報などの詳細コミッショニングログを取得したい場合
```bash
chip-tool pairing code-wifi <Node-ID> "$SSID" "$PASS" "$MPC" --trace_decode 1 --paa-trust-store-path ../../credentials/production/paa-root-certs
```

## ３．デバイスの操作例

### コミッショニングしたMatterデバイスの電源OFFができるか
Node IDとEndpointを1と指定して操作
```bash
chip-tool onoff off <Node-ID>  1
```

### コミッショニングしたMatterデバイスの電源ONができるか
Node IDとEndpointを1と指定して操作
```bash
chip-tool onoff on <Node-ID>  1
```
