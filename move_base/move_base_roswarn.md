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
- 制御ループの実行周期が, 設定した`controller_frequency`よりも遅延している 
- これによりロボットの速度指令が出なくなり, 結果として停止する  

`update_frequency`のWarning
- コストマップの更新ループが遅延して, コストマップが正常に更新されないことが起こる
- 結果としてロボットが停止, もしくは障害物を無視した動きをする

## 解決策
例1: `controller_frequency`の場合
```
[ WARN] [xxxx]: Control loop missed its desired rate of 20.0000Hz... the loop actually took 0.0539 seconds
```
このログから,  
- 実際にループ処理にかかった時間 → 0.0539s
- 周波数換算すると → 1 ÷ 0.0539 ≒ 18.5Hz

よって理論上の上限周波数は18.5Hz
→ 余裕を持って15Hz程度に設定する



例2: `update_frequency`の場合
```
[ WARN] [xxxx]: Map update loop missed its desired rate of 2.0000Hz... the loop actually took 1.0089 seconds
```
- 実際の更新時間 → 1.0089s
- 周波数換算すると → 1 ÷ 1.0089 ≒ 0.99Hz

よって理論上の上限周波数は0.99Hz
→ 1Hz程度に設定

## 補足：再現しやすいケース
- CPUが高負荷で処理が遅れている