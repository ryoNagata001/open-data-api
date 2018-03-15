# org.opendata.biznet

## 1. 事前準備
- [こちらを参考にしてみてください](https://hyperledger.github.io/composer/installing/development-tools.html)

## 2. 展開先のHyperledger Fablicを起動する
1. 必要なファイルのダウンロードと解凍
```
$ mkdir ~/fabric-tools && cd ~/fabric-tools
 
$ curl -O https://raw.githubusercontent.com/hyperledger/composer-tools/master/packages/fabric-dev-servers/fabric-dev-servers.zip
$ unzip fabric-dev-servers.zip
```
2. 起動
この際Dockerを起動してある必要があります。
```
$ cd ~/fabric-tools
$ ./downloadFabric.sh
$ ./startFabric.sh
$ ./createPeerAdminCard.sh
```
## 3. deploy
1. composer runtimeをinstallします。
```
$ composer runtime install --card PeerAdmin@hlfv1 --businessNetworkName open-data-ver1_0_0
```
2. ビジネスネットワークを展開します。
```
$ composer network start --card PeerAdmin@hlfv1 --networkAdmin admin --networkAdminEnrollSecret adminpw --archiveFile open-data-ver1_0_0@0.0.1.bna --file networkadmin.card
```
3. ネットワーク管理者IDをインポートします。
```
$ composer card import --file networkadmin.card
```
4. 正常に展開されたか確認する。
```
$ composer network ping --card admin@open-data-ver1_0_0
```

## 4. RESTサーバを生成する
```
$ composer-rest-server
? Enter the name of the business network card to use: admin@open-data-ver1_0_0
? Specify if you want namespaces in the generated REST API: always use namespaces
? Specify if you want to enable authentication for the REST API using Passport: No
? Specify if you want to enable event publication over WebSockets: Yes
? Specify if you want to enable TLS security for the REST API: No
```
commandがうまく通ったら
`http://localhost:3000`
にアクセスしてみてください。


## 5. Hyperledger Fablicの終了
```
$ cd ~/fabric-tools
$ ./stopFabric.sh
$ ./teardownFabric.sh
```