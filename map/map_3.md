# 地図の作り方_2(glim)
ここでは[glim](https://github.com/koide3/glim)で地図を作成する手順と, 有効なパラメータを紹介します. glimは3D-LiDARとIMUを組み合わせて高精度な三次元地図を構築するGraphベースSLAMです.   

## パラメータ設定
[こちら](https://github.com/YuseiShiozawa/glim/tree/ok/config)に私が調整したものを置いています. これが最善なパラメータであるかは分かりませんが, 津田沼チャレンジのコースであれば問題なく作成できることを確認済みです.  
なお、より正確な地図を作成したい場合は, パラメータを精度重視の設定に変更することで改善が見込めます. ただし, その分計算負荷が高くなり, CPU使用率も上昇します.  
つまり, 使用するPCの性能に結果が大きく左右されるということです.  

実際, 私の周囲でglimを使用している方々の多くは, GPUを搭載した高性能なPCで動かしていました.  

参考までに, 私が地図作成時に使用したPCのスペックを以下に記載しておきます.  
CPU: Intel Core i7-1165G7  
メモリ: 32GB  

## 作成手順
### 1. rosbagの記録
まず, `rosbag`を記録します. コントローラでロボットを操作させて記録してください. 

```bash
$ rosbag record -a
```

上記だと, 津田沼チャレンジコースでは数十GBくらいになるので下記を推奨. 必要最低限のトピックのみを記録します. 

```bash
$ rosbag record /tf /tf_static /rfans/surestar_points /imu_data
```

### 2. glimの実行
メモリ使用率が100%近くなると, PCがフリーズする可能性があるため注意してください.  
```bash
# 端末1
$ roslaunch glim_ros glim_rosnode.launch 

# 端末2
$ rosbag play rosbagファイル名
CPUに負荷がかかりすぎている場合は, 再生速度を下げてください

```
rosbagの再生が完了し, 地図の生成が終わったことを確認したら, `Ctrl+C`で終了してください.  
地図は`~/tmp/`に`dump`というディレクトリ名で保存されます.  `dump`ディレクトリは, `mv`コマンドでどこか分かりやすいパスに移動させてください.  

### 3. 地図をply形式で保存し, pcd形式に変換する
```bash
# 端末1
$ roscore

# 端末2
$ rosrun glim_ros offline_viewer
```
offline_viewerを起動すると, マップビューアの画面（screen）が表示されます.  
左上のメニューから`File`→`Open New Map`を選択し, 先ほど保存した`dump`ディレクトリを指定してください. これでマップが画面上に表示されます.  

次に, 再び左上の`File`→`Save`→`Export Points`を選び, 出力ファイルの名前と拡張子を入力します.  
このとき, 拡張子は`ply`にしてください.  
次に[glim_converter](https://github.com/Ryusei-Baba/glim_converter)を使ってpcd形式に変換します. スクリプト内のパスを適切に変更して実行してください.  
これで三次元地図の作成は完了です.    

### 4. 三次元地図をスライスして二次元地図へ
[pointcloud2pgm_slicer](https://github.com/cafeline/pointcloud2pgm_slicer)を使ってで二次元地図を出力できます. 