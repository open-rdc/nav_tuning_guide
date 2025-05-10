# Move Baseのパラメータ調整方法_1  
   
Move Baseの土台となるパラメータを調整します. ここで述べるパラメータはデフォルト値でも動作しますが, ロボットを動かす環境等に応じて適切に調整することで, より安定した経路計画を実現できます. 

## パラメータの説明([参考](https://robo-marc.github.io/navigation_documents/move_base.html))
### `base_global_planner`
- **意味**: 使用するグローバルプランナーのプラグイン名 *(default: navfn/NavfnROS)*
### `base_local_planner`
- **意味**: 使用する ローカルプランナーのプラグイン名 *(default: base_local_planner/TrajectoryPlannerROS)*
### `controller_frequency`
- **意味**: 制御ループを実行し、速度コマンドをベースに送信する周期(Hz) *(default: 20.0)*
### `planner_frequency`
- **意味**: グローバルプランニングループを実行する周期(Hz) *(default: 0.0)*
### `planner_patience`
- **意味**: プランナーが有効なプランを見つけられないとき, スペースクリアリング操作が実行されるまでの待機時間(秒)  *(default: 5.0)*
### `controller_patience`
- **意味**: コントローラーが有効なコントロールを受信しないとき, スペースクリアリング操作が実行されるまでの待機時間(秒) *(default: 15.0)*
### `oscillation_timeout`
- **意味**: スタック状態になってから, リカバリ動作を実行するまでの時間(秒) *(default: 0.0)*
### `oscillation_distance`
- **意味**: スタック状態ではないと見なされるために, ロボットが移動しなければならない距離(m) *(default: 0.5)*
---
## パラメータの調整/設定
- `base_global_planner`  
デフォルトが`navfn/NavfnROS`ですが, これより柔軟な代替として構築された`global_planner/GlobalPlanner`を選択できます.  

- `base_local_planner`  
デフォルトが`base_local_planner/TrajectoryPlannerROS`ですが, これよりも理解しやすいインターフェースを備え, ホロノミックロボットにも対応している`dwa_local_planner/DWAPlannerROS`を選択できます.  
- `controller_frequency`  
低すぎると動きが鈍く, 高すぎるとCPU負荷も高くなります. 
- `planner_frequency`  
デフォルト値でも, 経路がブロックされている場合や目標地を更新するときには再計画します. それら以外で再計画が必要な場合はデフォルト値よりも大きな値にしてください.  
- `planner_patience`, `controller_patience`, `oscillation_timeout`  
`oscillation_timeout`のみデフォルト値が`0.0`なので注意. 各値はリカバリ動作の実行のしやすさなので, ナビゲーション環境に応じて変更してください.  
- `oscillation_distance`  
小さすぎるとスタック状態とみなされないので注意. 
---
他の調整↓
- [recovery_behaviorのパラメータ](recovery_behavior.md)
- [costmapのパラメータ](costmap.md)
- [local_plannerのパラメータ](local_planner.md)
- [global_plannerのパラメータ](global_planner.md)

