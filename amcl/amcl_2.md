# AMCLのパラメータ調整方法_2
ここでは[AMCLのパラメータ調整方法_1](./amcl_1.md)で説明していない(調整の優先度が低い)パラメータを説明します. 

## パラメータの説明([参考](http://wiki.ros.org/amcl#Parameters))
### `min_particles`
- **意味**: 最小パーティクル数 *(default: 100)*
### `max_particles`
- **意味**: 最大パーティクル数 *(default: 5000)*
### `kld_err`
- **意味**: 推定位置の最大誤差 *(default: 0.01)*
### `kld_z`
- **意味**: パーティクルの信頼度係数 *(default: 0.99)*
### `update_min_d`
- **意味**: フィルタの更新を実行するために必要な距離(m) *(default: 0.2)*
### `update_min_a`
- **意味**: フィルタの更新を実行するために必要な回転角度(rad) *(default: π/6.0)*
### `transform_tolerance`
- **意味**: amclに必要な入力トピックの時間誤差の許容値(s) *(default: 0.1)*
### `initial_pose_x`
初期座標x *(default: 0.0)*
### `initial_pose_y`
初期座標y *(default: 0.0)*
### `initial_pose_a`
初期ヨー角 *(default: 0.0)*
### `laser_z_hit`
モデルの z_hit 部分の混合重み *(default: 0.95)*
### `laser_z_short`
モデルの z_short 部分の混合重み *(default: 0.1)*
### `laser_z_max`
モデルの z_max 部分の混合重み *(default: 0.05)*
### `laser_z_rand` 
モデルの z_rand 部分の混合重み *(default: 0.05)*

---

## パラメータ調整/設定  
上記パラメータは変更すると, 結果として[AMCLのパラメータ調整方法_1](./amcl_1.md)で調整したパーティクルの散らばり方が変わるので注意してください. また, これらのパラメータはデフォルト値でも津田沼チャレンジのコースを完走できることは確認済みです.

- `min_particles`, `max_particles`  
パーティクル数はCPUの負荷に直結するので, 過度に大きくすることは控えてください. 

- `kld_err`, `kld_z`  
これらのパラメータを変更すると, 極端にパーティクルの散らばり方が変わるので注意してください. `kld_err`を大きくしたり, `kld_z`を小さくすると, パーティクルが膨張します. [AMCLのパラメータ調整方法_1](./amcl_1.md)の調整が適切であれば調整の必要はないと思います.  

- `update_min_d`, `update_min_a`  
パーティクルの更新頻度です. 小さくすると, CPUの負荷が大きくなります. ただ過度に大きくすると, 自己位置推定の役割がなくなるので, 注意してください.   

- `transform_tolerance`  
例えば, 何らかのノードで3D-LiDARのpoint_cloudをlaserscanに変換して, その/scanをamclの入力にしているような場合があるとします. このとき, 入力トピックの時間誤差の許容値が低いと, 変換時にかかる時間により遅延が発生し, amclが動いてくれない/おかしな挙動になることがあります. このとき, amcl実行時のターミナルにはその旨が含まれているROSWARNが出力されていると思います.  

- `initial_pose_x`, `initial_pose_y`, `initial_pose_a`  
初期位置です. Rviz上の操作(2D Pose Estimate)で調整することは可能ですが, 津田沼チャレンジなど, スタート位置が固定されている場合は設定すると便利です. RvizでTFからbaselink等の`Position`(左からx, y, a)を参照して設定してください.  
<img src="images/position.png" width="500">   

- `laser_z_hit`, `laser_z_short`, `laser_z_max`, `laser_z_rand`  
これらのパラメータを変更すると, 極端にパーティクルの散らばり方が変わるので注意してください. デフォルトのままで問題ないです.  