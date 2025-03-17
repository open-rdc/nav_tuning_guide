# amclのパラメータ調整方法_1
重要なのはodom_alpha1~4であり, このパラメータがデフォルトのままでは自己位置が破綻することが多いです.  
ここでは. 端的にそれらのパラメータの役割を説明し, その調整方法を述べます.  

## `odom_alpha1`
- **意味**: ロボットの動きの回転成分から予想される回転推定におけるノイズ  
## `odom_alpha2`
- **意味**: ロボットの動きの並進成分から予想される回転推定におけるノイズ  
## `odom_alpha3`
- **意味**: ロボットの動きの並進成分から予想される並進推定におけるノイズ  
## `odom_alpha4`
- **意味**: ロボットの動きの回転成分から予想される並進推定におけるノイズ   

## 調整方法
### rosbag
まずnavigation開始時にrosbagをとります.  
```bash 
$ rosbag record -a
```
自己位置が破綻したら, `Ctrl+C`で終えてください. 

### Rvizでパーティクルの散らばりを確認
Rvizでパーティクルの散らばりを確認して, どのパラメータに問題があるかをある程度予想します. 
```bash
端末1$ cd ~/catkin_ws/src/orne-box/orne_box_navigation_executor/rviz_cfg
端末1$ rviz -d nav.rviz
端末2$ rosbag play rosbagファイル名
```
