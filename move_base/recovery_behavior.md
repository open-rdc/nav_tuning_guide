# recovery_behaviorのパラメータ調整方法

recovery_behaviorは, ロボットがナビゲーション中に障害物によりスタックした場合, 再度進行可能な状態に戻すために実行される動作です. `move_base` では複数のリカバリ動作を順番に試行します.  
ここでは, その設定方法について述べます.  

## 提供されているプラグイン
| プラグイン名 | 説明 |
|--------------|------|
| `clear_costmap_recovery/ClearCostmapRecovery` | コストマップの一部（もしくは全体）をクリアして再探索を試みる |
| `rotate_recovery/RotateRecovery` | その場で回転して別方向からの再探索を試みる |
| `move_slow_and_clear/MoveSlowAndClear` | ゆっくりと前進しながらコストマップをクリアする |
---
## パラメータの説明([参考](https://robo-marc.github.io/navigation_documents/packages.html#id7))
### clear_costmap_recovery
#### `reset_distance`
- **意味**: ロボットの現在位置を中心とした「正方形エリア」のサイズ *(default: 3.0)*
#### `force_updating`
- **意味**: `true`にすると, リカバリ後にcostmapの更新を即座にトリガーします *(default: false)*
#### `affected_maps`
- **意味**: どのcostmapを対象にクリアするかを指定 *(default: both)*
#### `layer_names`
- **意味**: costmap の中のどの「レイヤー」をクリアするかを制限 *(default: obstacles)*
---
### rotate_recovery
#### `sim_granularity`
- **意味**: 回転動作中のシミュレーションにおける角度の刻み幅(rad) *(default: 0.017)*
#### `frequency`
- **意味**: コマンドを送信する制御ループの周波数(Hz) *(default: 20.0)*
---
### move_slow_and_clear
#### `clearing_distance`
- **意味**: 障害物が除去されるロボットからの半径(m), この半径の内部の障害物がクリアされる *(default: 0.5)*
#### `limited_trans_speed`
- **意味**: このリカバリ動作の実行中にロボットが制限される並進速度(m/s) *(default: 0.25)*
#### `limited_rot_speed`
- **意味**: このリカバリ動作の実行中にロボットが制限される回転速度(rad/s) *(default: 0.45)*
##### `limited_distance`
- **意味**: 速度制限が解除される前にロボットが移動しなければならない距離(m) *(default: 0.3)*
#### `planner_namespace`
- **意味**: パラメータを再構成するプランナーの名前 *(default: DWAPlannerROS)*
---

## パラメータの調整/設定
### プラグイン設定
ここで指定した順番通りに実行されます.  
例  
```
  recovery_behaviors:
    - name: conservative_reset
      type: clear_costmap_recovery/ClearCostmapRecovery
    - name: rotate_recovery
      type: rotate_recovery/RotateRecovery
　　- name: move_slow_and_clear
      type: move_slow_and_clear/MoveSlowAndClear
```
---
### `clear_costmap_recovery`パラメータ
下記パラメータはプラグイン設定で指定した名前空間で定義してください.  
- `reset_distance`  
`0.0`に設定すれば, コストマップ全域をリセットします.  

- `force_updating`  
リカバリ時に即時反映させたいのなら`true`を推奨します.  
- `affected_maps`  
`local`ならローカルコストマップを, `global`ならグローバルコストマップを, `both`なら両方のコストマップをクリアします.  
- `layer_names`  
costmapに登録されている名前と一致している必要があります. 複数指定可能.  

---
### `rotate_recovery`パラメータ
下記パラメータはプラグイン設定で指定した名前空間で定義してください.  
- `sim_granularity`  
デフォルト値は約1度なので, 場合に応じて調整してください.  

- `frequency`  
デフォルト値で十分です.  

---
### `move_slow_and_clear`
下記パラメータはプラグイン設定で指定した名前空間で定義してください.  
- `clearing_distance`  
指定した半径の内部がクリアされます. 小さすぎると効果がないので注意.  

- `limited_trans_speed`, `limited_rot_speed`  
お好みで調整してください.  
- `limited_distance`  
どのくらいの距離まで速度を制限させるかなので, お好みで調整してください.  
- `planner_namespace`   
使用しているローカルプランナーの値に設定してください.  

---
他の調整↓
- [Move baseのパラメータ](move_base_1.md)
- [costmapのパラメータ](costmap.md)
- [local_plannerのパラメータ](local_planner.md)
- [global_plannerのパラメータ](global_planner.md)