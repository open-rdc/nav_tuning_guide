# Move_Baseにおける`controller_frequency`/`update_frequency`に関するWarningと挙動について
## 問題の概要
自律移動中に以下のようなWarningが表示され, ロボットが**突然停止する・動作が不安定になる**ことがあります.  
`controller_frequency`のWarning: 
```
[ WARN] [1725021051.947846184]: Control loop missed its destred ate of 20.0000Hz... the loop actually took 0.0539 seconds
```  

`update_frequency`のWarning: 
```
[ WARN] [1725021163.981655272]: Map update loop missed its desire rate of 2.0000Hz... the loop actually took 1.0089 seconds
```
## 原因
`controller_frequency`のWarning
- 制御ループの実行周期が, 設定した`controller_frequency`よりも遅延している. 
- これによりロボットの速度指令が出なくなり, 結果として停止する.  

## 解決策
## 補足：再現しやすいケース