# global_plannerのパラメータ  
ここでは, global_planner/GlobalPlannerを使う前提でパラメータを説明します.  
他のプランナーでも, 同名または類似のパラメータが存在し, 似たような役割を持つ場合があります.  

なお, ここで紹介するパラメータは**特に調整しなくても津田沼チャレンジのコースでは完走可能**であることを確認しています.  

## パラメータの説明([参考](http://wiki.ros.org/global_planner?distro=noetic)) 
下記パラメータは, **使用しているグローバルプランナーの名前空間**で定義してください.    
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
`false`にすると, 生成される経路が直線的かつカクカクになります. そのため, ロボットの動きが滑らかでなくなることがあります.  
こちらも基本的にはデフォルトで問題ありません.  

- `lethal_cost`, `neutral_cost`, `cost_factor`  
この3つの値を組み合わせて経路生成のコスト関数が決まります.  
そのため, 数値を変更すると, ロボットが通るルートが微妙に変化する場合があります.  
ただし, 津田沼チャレンジのコースではデフォルト設定で問題なく走行可能であるため, 特に変更する必要はないと考えられます.  

---
ここからの調整↓
- [Move baseのパラメータ](move_base_1.md)
- [recovery_behaviorのパラメータ](recovery_behavior.md)
- [costmapのパラメータ](costmap.md)
- [local_plannerのパラメータ](local_planner.md)
