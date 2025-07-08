# Move Baseのパラメータ調整方法_1  
   
Move Baseの土台となるパラメータを調整します. ここで述べるパラメータはデフォルト値でも動作しますが, ロボットを動かす環境等に応じて適切に調整することで, より安定した経路計画を実現できます. 

## パラメータの説明([参考](http://wiki.ros.org/move_base#Parameters))
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
周期が低いとロボットの動きが遅れます. 高すぎるとCPU負荷が増大するため, バランスを取って設定してください.  
また, このパラメータが適切な値でないと, **ROSWARNが出る**ことがあります. (実行時のターミナルに注意)  
- `planner_frequency`  
デフォルト値でも, 経路がブロックされている場合や目標地を更新するときには再計画します. 通常時にも定期的に再計画したい場合は, 適当なHz（例: 1.0）を設定してください.  
- `planner_patience`, `controller_patience`, `oscillation_timeout`  
いずれも「リカバリ動作を実行するまでの待機時間」です.  
特に`oscillation_timeout`はデフォルトが`0.0`で無効になっているため, スタック対策として有効化する場合は値を設定する必要があります.  
- `oscillation_distance`  
移動距離がこの値を超えないとスタックと判断されません. 小さすぎるとリカバリが発動しない場合があるので注意してください.  

---

他の調整↓
- [recovery_behaviorのパラメータ](recovery_behavior.md)
- [costmapのパラメータ](costmap.md)
- [local_plannerのパラメータ](local_planner.md)
- [global_plannerのパラメータ](global_planner.md)

