# オドメトリの調整方法  
以下の説明は[こちらのROS_WIKI](https://wiki.ros.org/navigation/Tutorials/Navigation%20Tuning%20Guide#Odometry)を参考にしています. 
何よりもまず始めにオドメトリを調整することをオススメします. 調整するパラメータは以下の２つです.
- wheel_radius_multiplier 
- wheel_separation_multiplier
  - [icart](https://github.com/open-rdc/icart)では`~/icart_mini/icart_mini_control/config`の`icart_mini_control.yaml`で設定されています

---
## 1. 調整の準備
まず, 調整のための準備を行います. Rvizを開き, Global OptionsのFixed Frameを`odom`に設定します. そしてaddパネルから`LaserScan`を追加し, その`Decay Time`を高く(20くらい)設定します. 
`LaserScan`のtopicも指定してください. (/scan等)  
設定をsaveさせておくと次回以降楽です.  
### 注意: センサフュージョンさせている場合はオドメトリのみにしてください. 

## 2. 並進成分のテスト
準備が整ったら, 並進成分のテストを行います. 壁から数メートル離してロボットを起動させ, 先程設定したRvizを立ち上げます. 壁に向かって1メートル走らせ, そしてスキャンに数センチ以上の厚みがないことを確認します. 数メートルに渡ってスキャンが広がっていたら`wheel_radius_multiplier`を調整してください. 

## 3. 回転成分のテスト
次に回転成分のテストを行います. 先程同様, 設定したRvizを立ち上げ, ロボットをその場で回転させます. 理想はスキャンがぴったり重なることですが, 多少の回転ドリフトは予想されるので, スキャンが1～2度以上ずれていないことを確認します. それよりも大きくずれている場合, `wheel_separation_multiplier`を調整してください.  
最後に, このテストを終えたら, もう一度 [2. 並進成分のテスト](#2-並進成分のテスト)を実行してください. ずれがないことを再確認してオドメトリの調整は完了です. 