# 地図の作り方_2(slam_toolbox)
ここでは[slam_toolbox](https://github.com/SteveMacenski/slam_toolbox)で地図を作成する手順と, 有効なパラメータを紹介します. また, 二次元地図の作成手法は他にも多くありますが, CPUのみで津田沼チャレンジのための地図を問題なく作成できたものを選んでおります.  

## パラメータ設定
[このファイル](slam_toolbox_config.yaml)に私が調整したものを置いています. これが最善なパラメータであるかは分かりませんが, 津田沼チャレンジのコースであれば問題なく作成できることを確認しています. これを`~/slam_toolbox/config`にあるyamlファイルにコピペしてください.  
これに関しては, パラメータを精度重視の設定に変更すれば, より正確な地図を作ることができます. ただし, その分計算量が増えるため, CPU使用率も高くなります. つまり, PCの性能によって結果が左右されるということです. 高性能なPCを使っている人であれば, パラメータ調整により, さらに正確な地図を作成できると思います. 参考までに, 下記に著者のPCスペックを簡単に述べます.  
CPU: Intel Core i7-1165G7  
メモリ: 32GB  


## 作成手順
slam_toolboxで津田沼チャレンジのコースの地図を作成するには, 3D-LiDARから得られる`/surestar_points`(PointCloud2型)をLaserscan型に変換し, それを入力トピックにする必要があります. (詳しくは[こちら](https://github.com/open-rdc/orne-box/issues/130))  
そのため, 2D-LiDARから得られる`/scan`ではなく`pointcloud_to_laserscan`で変換された`/surestar_scan`を使っています.  
また, orne-box2の[3D-LiDAR起動launchファイル](https://github.com/YuseiShiozawa/orne-box/blob/box2/orne_box_bringup/launch/includes/rfans16.launch)では, 上記の処理も同時に実行しています. 

### -オフライン- (おすすめ)
#### 1. rosbagの記録
まず, `rosbag`を記録します. コントローラでロボットを操作させて記録してください. 

```bash
$ rosbag record -a
```

上記だと, 津田沼チャレンジコースでは数十GBくらいになるので下記を推奨. 必要最低限のトピックのみを記録します. 

```bash
$ rosbag record /tf /tf_static /rfans/surestar_scan
```

#### 2. slam_toolboxの実行
パッケージは`apt`でインストール, もしくはクローンしてください.  

2D-LiDARの/scanトピックから地図を作成する場合
```bash
# 端末1
$ roslaunch slam_toolbox offline.launch 

# 端末2
$ rviz 
Rviz画面左下の`add`からtopicの`map`を追加すると, 作成過程を可視化できます.  

# 端末3
$ rosbag play rosbagファイル名
CPUに負荷がかかりすぎている場合は, 再生速度を下げてください

# 端末4(rosbagの再生が終わり, 地図が完成したことを確認したら)
$ cd 地図を保存したいディレクトリ
$ rosrun map_server map_saver -f 地図の名前
```

---

### -オンライン-
#### slam_toolboxの実行
コントローラでロボットを操作できる状態で下記を実行

```bash
# 端末1
$ roslaunch slam_toolbox online_async.launch 

# 端末2
$ rviz 
Rviz画面左下の`add`からtopicの`map`を追加すると, 作成過程を可視化できます.  

# 端末3(コントローラでゴールまでたどり着き, 地図が完成したことを確認したら)
$ cd 地図を保存したいディレクトリ
$ rosrun map_server map_saver -f 地図の名前
```

## 地図の手直し
作成した地図は, 自律走行の上で必要ないもの(人, ノイズ等)が描かれている場合が多々あります. そのため, 地図を手直しする必要があります. このとき, 編集ソフトを使うのですが, おすすめは`gimp`です. 下記のコマンドでインストールでき, 簡単に編集できます. 
```bash
$ sudo apt install gimp
```