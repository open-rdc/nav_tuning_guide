# local_plannerのパラメータ  
ここでは, 使用しているプランナーが`base_local_planner::TrajectoryPlannerROS`であることを前提に説明します. 他のプランナーでも同じ名前, もしくは似たような名前で同じ役割のパラメータがあります.  

## パラメータの説明([参考](https://robo-marc.github.io/navigation_documents/base_local_planner.html#parameters-baselocalplanner))  
下記パラメータは, 選択したlocal_plannerの名前空間で定義してください.  
### `acc_lim_x`
- **意味**: ロボットの縦方向の最大加速度(m/s^2) *(default: 2.5)*  
### `acc_lim_theta`
- **意味**: ロボットの回転方向の最大加速度(rad/s^2) *(default: 3.2)*  
### `max_vel_x`
- **意味**: ロボットの縦方向の最大速度(m/s) *(default: 0.5)*  
### `min_vel_x`
- **意味**: ロボットの縦方向の最小速度(m/s) *(default: 0.1)*
### `max_vel_theta` 
- **意味**: ロボットの回転方向の最大速度(rad/s) *(default: 1.0)*
### `min_vel_theta`
- **意味**: ロボットの回転方向の最小速度(rad/s) *(default: -1.0)*
### `min_in_place_vel_theta`
- **意味**: その場で旋回するときの回転方向の最小速度(rad/s) *(default: 0.4)*
### `escape_vel`
- **意味**: 脱出動作中, ロボットが後退するときの速度(m/s) *(default: -0.1)*
### `escape_reset_dist`
- **意味**: 脱出動作が解除されるまでにロボットが移動する必要がある距離(m) *(default: 0.1)*  
### `escape_reset_theta`
- **意味**: 脱出動作が解除されるまでにロボットが回転する必要がある角度(rad) *(default: 0.5 * π)*  
### `yaw_goal_tolerance`
- **意味**: ロボットが目標地点に到達したときの, 向きの許容誤差(rad) *(default: 0.5)*  
### `xy_goal_tolerance`  
- **意味**: ロボットが目標地点に到達したときの, 距離の許容誤差(m) *(default: 0.10)*  
### `sim_time`  
- **意味**: 軌道をフォワードシミュレーションする時間(s) *(default: 1.0)*  
### `meter_scoring`
- **意味**: `gdist_scale`, `pdist_scale`がメートル単位で表されるか否かのパラメータ, falseの場合はセル単位 *(default: false)*  
### `pdist_scale` 
- **意味**: ロボットがどれだけパスに沿って動こうとするかの重み *(default: 0.6)*  
### `gdist_scale`  
- **意味**: ロボットがローカルの目標にどれだけ近付こうとするかの重み *(default: 0.8)*  
### `occdist_scale`  
- **意味**: ロボットが障害物をどれだけ回避しようとするかの重み  *(default: 0.01)* 
---
## パラメータ調整/設定  
- `acc_lim_x`, `acc_lim_theta`  
それぞれ加速度に関わるパラメータです. orne-boxでは, 特に変更する必要はないように思います.  

- `max_vel_x`, `min_vel_x`, `max_vel_theta`, `min_vel_theta`, `min_in_place_vel_theta`  
最大/最小速度に関わるパラメータです. `max_vel_x`に関しては, orne-boxではデフォルト値よりやや高め(0.8 ~ 1.0)にしないと, 自律走行時やや遅すぎるように思います.  

- `escape_vel`
ロボットを障害物の回避等で後退させたい場合は負の値である必要があります.  

- `escape_reset_dist`, `escape_reset_theta`  
どちらかが満たされれば脱出動作は解除されます. デフォルトではかなり解除されやすくなっているので, 必要に応じて大きくしてください.  

- `yaw_goal_tolerance`, `xy_goal_tolerance`  
デフォルトでは, 目標(waypoint)に対し, かなり厳密に姿勢を合わせようとします. 必要に応じて大きくしてください.  

- `sim_time`
local_plannerがどれだけ先までパスを生成するかのパラメータですが, 大きくすると極端にCPUに負荷をかけることになります.  

- `meter_scoring`  
単位変更パラメータです. 使いやすい方を選べばいいと思います.  

- `pdist_scale`, `gdist_scale`, `occdist_scale`  
それぞれパスの追従性に関わる重みパラメータです. `pdist_scale`は大きくすると障害物回避に弱くなりますが, グローバルパスの追従性は上がります. `gdist_scale`は大きくすると, グローバルパスの追従性は下がりますが, 障害物を大きく回避できます. `occdist_scale`は大きくすると, 障害物をそもそも回避しなくなるので注意が必要です.  

---
次の調整↓
- [Move baseのパラメータ](move_base_1.md)
- [recovery_behaviorのパラメータ](recovery_behavior.md)
- [costmapのパラメータ](costmap.md)
- [global_plannerのパラメータ](global_planner.md)