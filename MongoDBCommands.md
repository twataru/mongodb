# mongodbコマンド
#### 状態の監視
```
> mongostat --discover -h <host-name>
```

リクエストをどのホストで捌いてるか見れる


#### クエリの実行計画の出力
```
> db.collection.find().explain()
```
スキャンの仕方とか見れる  
```db.collection.find().explain("executionStats") ```とすると件数とか処理時間とかも見れる


# レプリカセットコマンド  
#### レプリカセットステータスの参照 (プライマリーとかセカンダリーとか)
```
 > rs.status()
```

#### レプリカセットコンフィグの参照（各メンバーの設定値とか）
```
# rs.conf()
```


#### レプリカセットコンフィグの再設定
```
> rs.reconfig(<config>)
```

reconfig = rs.conf() とかすると 変数 にコンフィグ内容が格納される（中身はjson形式）  
reconfig.member[0].priority = 1 みたいな感じに設定したあと、rs.reconfig(reconfig) みたいな感じに使う



#### メンバーの追加
```
> rs.add(<host-name>)
```


#### メンバーの削除
```
> rs.remove(<host-name>)
```


#### メンバーの降格
```
> rs.stepDown()
```
プライマリメンバーなんかをセカンダリに降格させられる
