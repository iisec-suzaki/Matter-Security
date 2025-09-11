# Fabric情報の確認
コミッショングしたデバイスのFabric情報を確認する。

## デバイスが保存しているファブリックの数の上限
```bash
chip-tool operationalcredentials read supported-fabrics <Node-ID> 0
```
出力結果例(保存しているファブリックの数が7の場合)
```text
~
SupportedFabrics: 7
~
```

## デバイスが現在保存しているファブリックの数
```bash
chip-tool operationalcredentials read commissioned-fabrics <Node-ID>  0
```
出力結果例(保存しているファブリックの数が3の場合)
```text
~
CommissionedFabrics: 3
~
```

## デバイスが現在保存しているファブリック情報の一覧
```bash
chip-tool operationalcredentials read fabrics <Node-ID> 0 \
  --fabric-filtered 0 \
```
出力結果例(保存しているファブリックの数が3の場合)
```text
~
[1751006601.625] [8294:8296] [TOO]   Fabrics: 3 entries
[1751006601.625] [8294:8296] [TOO]     [1]: {
[1751006601.625] [8294:8296] [TOO]       RootPublicKey: **********
[1751006601.626] [8294:8296] [TOO]       VendorID: △△△
[1751006601.626] [8294:8296] [TOO]       FabricID: 1
[1751006601.626] [8294:8296] [TOO]       NodeID: 1
[1751006601.626] [8294:8296] [TOO]       Label: 
[1751006601.626] [8294:8296] [TOO]       FabricIndex: 1
[1751006601.626] [8294:8296] [TOO]      }
[1751006601.626] [8294:8296] [TOO]     [2]: {
[1751006601.626] [8294:8296] [TOO]       RootPublicKey: **********
[1751006601.626] [8294:8296] [TOO]       VendorID: △△△
[1751006601.626] [8294:8296] [TOO]       FabricID: 1
[1751006601.626] [8294:8296] [TOO]       NodeID: 2
[1751006601.626] [8294:8296] [TOO]       Label: 
[1751006601.626] [8294:8296] [TOO]       FabricIndex: 2
[1751006601.626] [8294:8296] [TOO]      }
[1751006601.626] [8294:8296] [TOO]     [3]: {
[1751006601.626] [8294:8296] [TOO]       RootPublicKey: **********
[1751006601.626] [8294:8296] [TOO]       VendorID: △△△
[1751006601.626] [8294:8296] [TOO]       FabricID: 1
[1751006601.626] [8294:8296] [TOO]       NodeID: 3
[1751006601.626] [8294:8296] [TOO]       Label: 
[1751006601.626] [8294:8296] [TOO]       FabricIndex: 3
[1751006601.626] [8294:8296] [TOO]      }
~
```

## デバイスの保存しているファブリックを指定して削除
取得したファブリック情報からわかったFabricIndexとNodeIDを指定して、特定のファブリック情報を削除する。
```bash
chip-tool operationalcredentials remove-fabric <FabricIndex> <Node-ID> 0 
```
