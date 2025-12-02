# chip-tool・chip-certのビルド手順

## 環境
- Ubuntu 22.04

## 準備
### １．依存パッケージのインストール
```bash
sudo apt update
sudo apt install -y git build-essential python3 python3-venv ninja-build \
                 clang llvm libssl-dev libavahi-client-dev libglib2.0-dev \
                 libdbus-1-dev libreadline-dev
```

### ２．Matter SDK取得
```bash
git clone https://github.com/project-chip/connectedhomeip.git
cd connectedhomeip
git submodule update --init --recursive
```

### ３．ビルド環境セットアップ
```bash
./scripts/bootstrap.sh
source scripts/activate.sh
```

## chip-toolのビルド

### １．ビルド実行
```bash
cd connectedhomeip
source scripts/activate.sh
./scripts/examples/gn_build_example.sh examples/chip-tool out/chip-tool
```

### ２．動作確認
```bash
cd out/chip-tool
./chip-tool help
```

## chip-certのビルド

### １．ビルド実行
```bash
cd connectedhomeip
source scripts/activate.sh

gn gen out/host
ninja -C out/host chip-cert
```

### ２．動作確認
```bash
cd out/host
./chip-cert help
```
