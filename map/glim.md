# 地図の作り方_3(glim)
ここでは[glim](https://github.com/koide3/glim)で地図を作成する手順と, 有効なパラメータ設定を紹介します. glimは3D-LiDARとIMUを用いて高精度な三次元地図を構築するGraphベースSLAMです.   

## パラメータ設定
[こちら](https://github.com/YuseiShiozawa/glim/tree/ok/config)に私が調整したものを置いています.  
この設定で, 津田沼チャレンジのコースにおいて問題なく地図を作成できることを確認済みです.  

💡 精度をさらに高めたい場合は, パラメータを精度重視の設定に変更することで改善できます. ただし, その分計算負荷が高くなり, CPUやメモリの使用率が大幅に上昇します.  

glimは重めの処理を伴うため, **GPUを搭載した高性能PCが推奨**されます.  
実際, 私の周りでglimを使用している方々の多くは, GPUを搭載したPCで動かしていました.  

【参考スペック（筆者PC）】
- CPU：Intel Core i7-1165G7
- メモリ：32GB

## 作成手順
### 1. rosbagの記録
コントローラ操作でロボットを走らせ, 次のコマンドで必要なトピックを記録します. 

```bash
$ rosbag record /tf /tf_static /rfans/surestar_scan
```
※ フル記録（-a）も可能ですが, rosbagのファイルサイズが数十GBになるため注意が必要です. 

### 2. glimの実行
⚠️ GPUなしのPCで実行する際の注意：  
メモリ使用率が高くなるため, PCがフリーズしないよう常にリソース状況に注意してください. 必要であれば再生速度を下げることも検討してください. 
```bash
# 端末1
$ roslaunch glim_ros glim_rosnode.launch 

# 端末2
$ rosbag play rosbagファイル名
CPUに負荷がかかりすぎている場合は, 再生速度を下げてください

```

地図の作成が終わったら, `Ctrl+C`でglimを終了します.  
作成された地図は`~/tmp/dump/`に保存されるため, `mv`コマンドで任意の場所へ移動してください.  
※`~/tmp/dump/`は地図を作成する度に上書き保存されるので注意

### 3. 地図をply形式で保存し, pcd形式に変換する
1. 必要なノードを立ち上げる：
```bash
# 端末1
$ roscore

# 端末2
$ rosrun glim_ros offline_viewer
```
2. offline_viewerが起動したら, 画面左上メニューから`File`→`Open New Map`を選択し, 先ほど保存した`dump`ディレクトリを読み込みます.  

3. 次に, 再び左上の`File`→`Save`→`Export Points`を選択し, `.ply`拡張子で保存します.   

4. 保存した`.ply`ファイルを[glim_converter](https://github.com/Ryusei-Baba/glim_converter)を使って`.pcd`に変換します.  
※パスはスクリプト内で適切に設定してください.  
 
 

### 4. 三次元地図をスライスして二次元地図へ
[pointcloud2pgm_slicer](https://github.com/cafeline/pointcloud2pgm_slicer)を使って, 作成した三次元地図を任意の高さでスライスし, 二次元地図として出力できます. 