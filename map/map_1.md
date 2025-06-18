# 地図の作り方_1
問題なくロボットを自律移動させるには, 環境地図の作成が重要です. ここでは, 大きく２つの地図作成手法を紹介します.  

---

## 方法1: 二次元地図の作成(例: [slam_toolbox](https://github.com/SteveMacenski/slam_toolbox))
主に2D-LiDARを用いて比較的簡単に二次元地図を作成する手法です. オンラインでもオフラインでも作成できます.  
詳細は[こちら](./map_2.md)

## 方法2: 三次元地図の作成後, スライスして二次元地図を作成(例: [glim](https://github.com/koide3/glim) + [pointcloud2pgm_slicer](https://github.com/cafeline/pointcloud2pgm_slicer))
主に3D-LiDARを用いてオフラインで地図を作成し, 任意の高さで2Dスライスすることで二次元地図を作成する手法です. GPUがなく, CPUのみでの作成は少々限界があるように思えます.  
詳細は[こちら]()


