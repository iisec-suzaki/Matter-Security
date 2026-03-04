# Node-REDのMatter仮想デバイスのビルド手順

Node-REDのMatter Pluginの手順(https://flows.nodered.org/node/@node-red-matter/node-red-matter)
を参考にMatter仮想デバイスを作成。

## 環境
- Windows 11

## 準備
### １．Node.jsを以下からインストール
https://nodejs.org/ja/download

### ２．Node-REDのインストール
```bash
npm install -g --unsafe-perm node-red
```

#### Node-REDの起動
```bash
node-red
```

#### Node-REDへのアクセス
http://127.0.0.1:1880
などでブラウザにアクセスすると、仮想デバイス作成画面に移る。
ノード検索などでMatterデバイスを作成し、電球などのデバイスカテゴリを設定して作成。
手順に従って作成し、表示されるRQコードでコミッショニングできる。
