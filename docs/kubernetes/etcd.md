# etcd Index

etcd関連のページ

## Menu

### コマンドの変更

所々でetcdのkey valueの追加に `etcdctl set key value`というのを見かけるが
3.4から API versionのデフォルトが3になったことで setオプションが使えなくなっている

https://etcd.io/docs/v3.3/upgrades/upgrade_3_4/