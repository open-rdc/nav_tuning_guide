# nav_tuning_guide
ロボットのナビゲーションを調整するためのドキュメント. 以下の一部ファイル名およびコマンドは, [orne-box](https://github.com/open-rdc/orne-box)を基準にしています.  
🚧作成中🚧  

## 目次

1. [パッケージ別パラメータ調整](#パッケージ別パラメータ調整)  
   - [icart（オドメトリ調整）](#icartオドメトリ調整) 
   - [AMCL（自己位置推定）](#amcl自己位置推定)  
   - [Move Base（経路計画）](#move-base経路計画)  
2. [地図作成](#地図作成)   

---

## パッケージ別パラメータ調整
### icart（オドメトリ調整）
まずオドメトリの調整を.  
- [icart_1.md](./icart/icart_1.md): オドメトリ調整の手順

### AMCL（自己位置推定）
rosbagを活用してパラメータを調整します.  
- [amcl_0.md](./amcl/amcl_0.md): 最低限の設定
- [amcl_1.md](./amcl/amcl_1.md): 主要パラメータの調整
- [amcl_2.md](./amcl/amcl_2.md): その他パラメータの調整

### Move Base（経路計画）
Move Baseの各要素の調整と設定. 
- [move_base_0.md](./move_base/move_base_0.md): 最低限の調整/設定
- [move_base_1.md](./move_base/move_base_1.md): Move Baseの土台となるパラメータ調整
- [recovery_behavior.md](./move_base/recovery_behavior.md): リカバリ動作のパラメータ調整
- [costmap.md](./move_base/costmap.md): コストマップのパラメータ調整
- [local_planner.md](./move_base/local_planner.md): ローカルプランナーのパラメータ調整
- [global_planner.md](./move_base/global_planner.md):グローバルプランナーのパラメータ調整

## 地図作成
地図作成の手順とパラメータ. 
- [map_1.md](./map/map_1.md): 地図作成方法の説明
- [map_2.md](./map/map_2.md): slam_toolboxでの地図作成方法
- [map_3.md](./map/map_3.md): glimでの地図作成方法