# global_plannerのパラメータ  
ここでは, 使用しているプランナーが`global_planner/GlobalPlanner`であることを前提に説明します. 他のプランナーでも同じ名前, もしくは似たような名前で同じ役割のパラメータがあります. また, ここで説明するパラメータは特に変更しなくても津田沼チャレンジのコースで完走できることを確認しています.  

## パラメータの説明([参考](https://robo-marc.github.io/navigation_documents/global_planner.html#globalplanner-parameters)) 
下記パラメータは, 選択したlocal_plannerの名前空間で定義してください.  
### `use_dijkstra`
- **意味**: falseの場合, A\*アルゴリズムを利用します *(default: true)*  
### `use_quadratic`  
- **意味**: falseの場合, 単純加算を使用するため, 生成されるパスが滑らかではなくなります *(default: true)*  
### `lethal_cost`
- **意味**: 致命的コストの値 *(default: 253)*  
### `neutral_cost`
- **意味**: ニューラルコストの値 *(default: 50)*  
### `cost_factor`
- **意味**: コストマップのコストをスケーリングするための値 *(default: 3.0)*


## パラメータ調整/設定  
- `use_dijkstra`  
dijkstra法とA*\アルゴリズムのどちらを使うかです. dijkstraでも, 特に目に見えてパス生成が遅いというわけではないので, デフォルトのままでよいかと思います.  

- `use_quadratic`  
falseの場合, 生成されるパスが滑らかではなくなるため, 実ロボットの動きもカクつきます. それでも走れないことはないですが, デフォルトのままでよいと思います.  

- `lethal_cost`, `neutral_cost`, `cost_factor`  
これら3つの値で生成されるパスの軌道が変わります. これら3つの値は変更しなくても, 津田沼チャレンジでは完走できるので特に変更する必要はないと思います. 

---
ここからの調整↓
- [Move baseのパラメータ](move_base_1.md)
- [recovery_behaviorのパラメータ](recovery_behavior.md)
- [costmapのパラメータ](costmap.md)
- [local_plannerのパラメータ](local_planner.md)
