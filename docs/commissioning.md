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
Node IDには任意の数字を入力（例：1）
```bash
chip-tool pairing code-wifi <Node-ID> "$SSID" "$PASS" $MPC \
  --paa-trust-store-path ../../credentials/production/paa-root-certs
```

## ３．コミッショニングができたか確認

### コミッショニングしたMatterデバイスの電源OFFができるか
Node IDとEndpointを1と指定して操作
```bash
chip-tool onoff off <Node-ID>  1
```
