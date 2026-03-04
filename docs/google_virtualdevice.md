# GoogleのMatter仮想デバイスのビルド手順

公式のMatter仮想デバイス作成手順(https://developers.home.google.com/codelabs/matter-device-virtual?hl=ja#1)
に従ってMatter仮想デバイスを作成。

## 環境
- Debian GNU/Linux 12

## 準備
### １．DockerEngineインストール
```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
#### Docker確認
```bash
sudo systemctl status docker
```

### ２．Docker 動作確認
```bash
sudo docker run hello-world
```

## 仮想デバイスのビルド

### １．git clone
```bash
git clone https://github.com/project-chip/connectedhomeip.git
```

### ２．ビルドイメージ
```bash
buildimage=$(grep chip-build .github/workflows/chef.yaml | head -n 1 | awk '{print $2}')
echo $buildimage
xhost local:1000
```

### ３．コンテナ起動
```bash
docker run -it --ipc=host --net=host -e DISPLAY --name matter-container --mount source=$(pwd),target=/workspace,type=bind   --workdir="/workspace" $buildimage /bin/bash
```

### ４．SDK初期化
```bash
source scripts/bootstrap.sh
python3 scripts/checkout_submodules.py --shallow --platform linux
```

### ５．デバイスの設定
Google Homeデベロッパーコンソールを開き、公式手順を参考にデバイスの設定を行う。

### ６．デバイスの構築
Debian側でデバイスをビルドした後、デバイスを実行する。

####デバイスのビルド
```bash
cd examples/chef
./chef.py -zbr -d rootnode_dimmablelight_bCwGYSDpoe -t linux
```

####デバイスの実行
```bash
./linux/out/rootnode_dimmablelight_bCwGYSDpoe
```

デバイスを実行後、ログに表示される１１桁のManual Pairing CodeかURLに表示されるQRコードを用いて、GoogleHomeAppなどのコントローラでコミッショニングすることができる。
