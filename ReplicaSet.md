# レプリカセット


## 手順１ /etc/mongo.conf の設定
３台全てのmongo.confに以下の設定を行う

```
> vi /etc/mongod.conf
```  
```
net:  
  port: 27017  
  bindIp: 0.0.0.0  

replication:
  replSetName: "testRS01"  ※任意の名前
```


## 手順２ mongod サービスの起動  
３台全てでサービスを起動させる (successfullyが書かれていれば成功)

```
> mongod --config /etc/mongod.conf 
```  
```
about to fork child process, waiting until server is ready for connections.
forked process: <プロセス番号>
child process started successfully, parent exiting
```


## 手順３ レプリカセットの初期設定
#### 1.プライマリーとなるマシンのmongoに接続

```
> mongo 
```

もしくは

```
> mongo　--host <primary-machin-hostname> 
```


#### 2.レプリカセットの初期化

```
> config = { _id : "testRS01", members: [ { _id: 0, host: "host01.address.co.jp:27017" }, { _id: 1, host: "host02.address.co.jp:27017" }, { _id: 2, host: "host03.address.co.jp:27017","arbiterOnly" : true} ]}
```  

```
> rs.initiate(config)

{
        "ok" : 1,
        "operationTime" : Timestamp(1513932167, 1),
        "$clusterTime" : {
                "clusterTime" : Timestamp(1513932167, 1),
                "signature" : {
                        "hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),
                        "keyId" : NumberLong(0)
                }
        }
}
```  

## 手順４ 確認
```
> rs.status()

{
        "set" : "testRS01",
        },
        "members" : [
                {
                        "_id" : 0,
                        "name" : "host01.address.co.jp:27017",
                        "stateStr" : "PRIMARY",
                },
                {
                        "_id" : 1,
                        "name" : "host02.address.co.jp:27017",
                        "stateStr" : "SECONDARY",
                },
                {
                        "_id" : 2,
                        "name" : "host03.address.co.jp:27017",
                        "stateStr" : "ARBITER"",
                }
        ],
        "ok" : 1,
        "operationTime" : Timestamp(1513934980, 1),
        "$clusterTime" : {
                "clusterTime" : Timestamp(1513934980, 1),
                "signature" : {
                        "hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),
                        "keyId" : NumberLong(0)
                }
        }
}
```
