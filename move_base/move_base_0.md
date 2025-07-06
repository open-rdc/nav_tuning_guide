# Move Baseのパラメータ調整方法_0
`move_base`ノードを問題なく動作させるには, いくつかの**最低限のパラメータ調整**が必要です.  
ここでは, それぞれのパラメータの意味と, **最低限設定しておくべき値**を紹介します. 

## パラメータの説明([参考](http://wiki.ros.org/costmap_2d))
### costmap_2d
---
#### common
##### `footprint`
- **意味**: ロボットの形状を定義するためのパラメータ *(default: use `robot_radius`, default: 0.46)*
##### `publish_frequency`
- **意味**: Rvizで可視化するためのパブリッシュ周波数(Hz) *(default: 0.0)*
##### `observation_sources`
- **意味**: センサの名前リスト, 名前空間の定義 *(default: "")*
##### `data_type`(`observation_sources`で定義した名前空間内で)
- **意味**: トピックに関連付けられているデータ型 *(default: PointCloud)*
##### `clearing`(`observation_sources`で定義した名前空間内で)
- **意味**: 障害物がなくなった場所を地図上から消すためのパラメータ *(default: false)*
---
#### local
##### `rolling_window`
- **意味**: ローリングウィンドウバージョンのコストマップを使用するかどうか *(default: false)*
- `static_map`パラメータがtrueに設定されている場合, このパラメータは`false`に設定する必要がある
---
#### global
##### `map_topic`
- **意味**: コストマップが静的マップをサブスクライブするトピック名 *(default: map)*
##### `static_map`
- **意味**: グローバルコストマップにおいて、`map_server` から提供される静的マップを使用するためのパラメータ *(default: false)*
---
## 必須のパラメータ調整/設定
### costmap_common_params.yaml
- `footprint`  
設定しないと, ロボットの形状はデフォルトで**円形**(左図の赤線)になります.  
しかし, このままでは, 実際の形状と合っていないため, 意図しない停止や経路計画の失敗が起こる可能性があります.  
orne-boxでは, 以下のように設定することで, **実際のロボットの形に近い四角形の形状**(右図)を指定できます.  
```
footprint: [[0.433, 0.254], [-0.187, 0.254], [-0.187, -0.254], [0.433, -0.254]]
```
<img src="images/default_foopri.png" width="200" style="margin-right: 50px;">        <img src="images/box1_foopri2.png" width="190">   


- `publish_frequency`  
このパラメータを設定しないと, **Rviz上でコストマップが表示されません.**  
たとえば以下のように, `2.0`など適度な頻度で指定することで, 地図がリアルタイムに更新され, 視覚的にも確認しやすくなります.  
```
publish_frequency: 2.0
```

- `observation_sources`  
センサ（例: LiDAR）からのデータを使用するために, **使用するトピック名**（例: `scan`）を指定します.  
これを設定しないと, **地図に存在しない障害物を認識できません.**     
- `data_type`  
`observation_sources` で指定した各名前空間内で定義します. 
トピックのデータ型がデフォルトの`PointCloud`であれば省略可能ですが, `LaserScan`など異なる型を使用している場合は必ず指定してください.  
- `clearing`  
`observation_sources` で指定した各名前空間内で定義します.  
これを`true`にしないと, **一度現れた障害物が消えずに残り続けてしまいます.**  

例
```
footprint: [[0.433, 0.254], [-0.187, 0.254], [-0.187, -0.254], [0.433, -0.254]]
publish_frequency: 2.0
observation_sources: scan
scan: 
  {
    data_type: LaserScan,     
    clearing: true,   
  }

```
---
### local_costmap_params.yaml
- `rolling_window`  
`local_costmap`では, 静的な地図（static_map）を使わず, ロボット周辺の動的な地図を更新し続けるために, `rolling_window`を `true`に設定する必要があります.  

```
local_costmap:       
  rolling_window: true   
```
---

### global_costmap_params.yaml
- `map_topic`  
コストマップと自己位置推定で異なる地図を使用したい場合, コストマップ用のトピック名を指定してください. 

- `static_map`  
`true`に設定すると, `map_server`が提供する静的マップをコストマップとして使用できます. （通常はこちらで問題ありません）  
ただしこの設定では, **ナビゲーション中に地図を切り替えることができません.** 地図を切り替えたい場合は, `static_map`を`false`にし, 代わりに`plugins`を用いて`StaticLayer`を明示的に定義し, `map_topic`を指定する必要があります.  

例1）地図の切り替えが不要な場合  
```
global_costmap:
   map_topic: map_for_costmap 
   static_map: true 
```  


例2）地図の切り替えが必要な場合    
```
global_costmap:
   plugins:
    - name: StaticLayer
      type: costmap_2d::StaticLayer
    - name: inflationlayer
      type: costmap_2d::InflationLayer
   StaticLayer: 
     map_topic: map_for_costmap
```
---
ここからの調整↓
- [Move baseのパラメータ](move_base_1.md)
- [recovery_behaviorのパラメータ](recovery_behavior.md)
- [costmapのパラメータ](costmap.md)
- [local_plannerのパラメータ](local_planner.md)
- [global_plannerのパラメータ](global_planner.md)