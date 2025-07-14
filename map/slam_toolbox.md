# 地図の作り方_2(slam_toolbox)
ここでは, [slam_toolbox](https://github.com/SteveMacenski/slam_toolbox)を使って**2D地図を作成する手順**と, **有効なパラメータの設定**について説明します.  
多数ある2D地図作成手法の中で, **CPUのみ**で津田沼チャレンジの地図を問題なく作成できた方法を紹介しています.  

## パラメータ設定
オフラインで使用するパラメータは, [mapper_params_offline.yaml](./mapper_params_offline.yaml)にまとめてあります.  
この設定が最適解という保証はありませんが, 津田沼チャレンジのコースでは十分に使えることを確認済みです.  

このファイルを`~/slam_toolbox/config/`にある`mapper_params_offline.yaml`にコピー＆ペーストしてください.  

💡 高精度な地図を作りたい場合は, パラメータを「精度重視」寄りに変更可能ですが, その分CPU負荷が増加します.  
高性能なPCを使っている場合は, より細かく調整することで地図精度をさらに向上させられます.  

【参考スペック（筆者PC）】
- CPU：Intel Core i7-1165G7
- メモリ：32GB


## 作成手順
### 入力トピックの注意点
slam_toolboxで津田沼チャレンジのコースの地図を作成するには, 3D-LiDARから得られる`/surestar_points`(PointCloud2型)をLaserscan型に変換し, それを入力トピックにする必要があります. (詳しくは[こちら](https://github.com/open-rdc/orne-box/issues/130)) 

- 変換には`pointcloud_to_laserscan`を使用します
- orne-box2では[3D-LiDAR起動launchファイル](https://github.com/YuseiShiozawa/orne-box/blob/box2/orne_box_bringup/launch/includes/rfans16.launch)にこの処理も含まれています   

### -オフライン- (おすすめ)
#### 1. rosbagの記録
コントローラ操作でロボットを走らせ, 次のコマンドで必要なトピックを記録します. 

```bash
$ rosbag record /tf /tf_static /rfans/surestar_scan
```
※ フル記録（-a）も可能ですが, rosbagのファイルサイズが数十GBになるため注意が必要です. 

#### 2. slam_toolboxの実行
パッケージは`apt`でインストール, もしくはクローンしてください.  

```bash
# 端末1：slam_toolboxの起動
$ roslaunch slam_toolbox offline.launch 

# 端末2：地図の可視化（Rviz）
$ rviz 
Rviz画面左下の`add`からtopicの`map`を追加すると, 作成過程を可視化できます.  

# 端末3：rosbagの再生
$ rosbag play rosbagファイル名
CPUに負荷がかかりすぎている場合は, -r オプションで再生速度を下げてください

# 端末4：地図の保存（再生終了後に実行）
$ cd 地図を保存したいディレクトリ
$ rosrun map_server map_saver -f 地図の名前
```

---

### -オンライン-
#### slam_toolboxの実行
コントローラでロボットを操作できる状態で下記を実行

```bash
# 端末1：slam_toolbox（オンライン）の起動
$ roslaunch slam_toolbox online_async.launch 

# 端末2：Rvizで地図を確認
$ rviz 
Rviz画面左下の`add`からtopicの`map`を追加すると, 作成過程を可視化できます.  

# 端末3：地図の保存（完成確認後）
$ cd 地図を保存したいディレクトリ
$ rosrun map_server map_saver -f 地図の名前
```

## 地図の手直し
作成した地図には, 人や一時的な障害物など, 自律移動に不要な情報が含まれることがあります.  
このような不要部分を取り除くには, 地図画像を編集す必要が有ります.  
**おすすめ編集ソフト**: `gimp`
```bash
$ sudo apt install gimp
```

## パラメータ和訳([参考](https://github.com/SteveMacenski/slam_toolbox/blob/ros2/README.md))
### 📌 基本設定（フレーム・トピック等）
| パラメータ名               | 説明                                 |
| -------------------- | ---------------------------------- |
| `odom_frame`         | オドメトリのTFフレーム名（例：`odom`）            |
| `map_frame`          | マップのTFフレーム名（例：`map`）               |
| `base_frame`         | ロボット本体のフレーム（例：`base_link`）         |
| `scan_topic`         | 使用するLaserScanトピック（絶対パス `/scan` など） |
| `scan_queue_size`    | scanのバッファサイズ。非同期モードでは `1` 推奨       |
| `transform_timeout`  | TFルックアップのタイムアウト時間（秒）               |
| `tf_buffer_duration` | TFを保存しておく時間（オフライン再生で重要）            |

### 💾 マップ生成・保存関係
| パラメータ名                | 説明                               |
| --------------------- | -------------------------------- |
| `use_map_saver`       | `true`にすると自動で地図保存サービスを立ち上げる      |
| `map_file_name`       | 起動時に読み込むpose-graphファイル名（任意）      |
| `map_start_pose`      | 既存マップから開始する場合の初期姿勢               |
| `map_start_at_dock`   | pose-graphの最初のノードからスタートするかどうか    |
| `map_update_interval` | Occupancy Gridを何秒ごとに更新するか（RViz用） |

### ⚙️ 動作モード・出力制御
| パラメータ名                      | 説明                                    |
| --------------------------- | ------------------------------------- |
| `enable_interactive_mode`   | 対話的な地図編集モードを有効にするか                    |
| `debug_logging`             | `true`にするとデバッグログ出力を有効化                |
| `localization_on_configure` | 起動時にローカリゼーションモードにするか（`false`で地図作成モード） |

### 📐 地図の精度・分解能
| パラメータ名                                | 説明                                 |
| ------------------------------------- | ---------------------------------- |
| `resolution`                          | Occupancy Mapの解像度（例：`0.05`m/pixel） |
| `min_laser_range` / `max_laser_range` | 地図作成に使うLiDARの有効距離（最小/最大）           |
| `minimum_travel_distance`             | 新しいスキャン処理を行う最小移動距離（メートル）           |
| `minimum_time_interval`               | 同期モードでのスキャン処理の最小時間間隔               |

### 🧠 Scan Matching関連
| パラメータ名                                               | 説明                                      |
| ---------------------------------------------------- | --------------------------------------- |
| `use_scan_matching`                                  | スキャンマッチングを行うか（基本 `true`）                |
| `use_scan_barycenter`                                | バリセンタ（重心）を使うか、通常は `false`               |
| `minimum_travel_heading`                             | 方位角の変化がこの値以上でスキャン更新                     |
| `position_covariance_scale` / `yaw_covariance_scale` | 発行される自己位置の不確かさ（大きいと下流の自己位置推定への影響が小さくなる） |

### 🔁 Loop Closure 関係（ループ閉じ）
| パラメータ名                                                                                                           | 説明                      |
| ---------------------------------------------------------------------------------------------------------------- | ----------------------- |
| `do_loop_closing`                                                                                                | ループ閉じを有効にするか（通常 `true`） |
| `loop_search_maximum_distance`                                                                                   | 閉じるスキャン候補との最大距離         |
| `loop_match_minimum_chain_size`                                                                                  | 閉じる対象とするスキャンの最小連続数      |
| `loop_match_maximum_variance_coarse` / `loop_match_minimum_response_coarse` / `loop_match_minimum_response_fine` | マッチング通過に必要な精度の閾値        |
| `loop_search_space_dimension` / `loop_search_space_resolution` / `loop_search_space_smear_deviation`             | ループ探索の範囲・解像度・平滑化係数      |

### 🔍 Scan Matching パラメータ（詳細）
| パラメータ名                                                                                                                    | 説明                       |
| ------------------------------------------------------------------------------------------------------------------------- | ------------------------ |
| `scan_buffer_size`                                                                                                        | ローカルマッチングに使うスキャンのバッファサイズ |
| `scan_buffer_maximum_scan_distance`                                                                                       | バッファ内のスキャンの有効距離（遠すぎると除外） |
| `link_match_minimum_response_fine`                                                                                        | スキャンマッチが成功と判断する閾値        |
| `link_scan_maximum_distance`                                                                                              | リンク可能なスキャン間の最大距離         |
| `correlation_search_space_dimension` / `correlation_search_space_resolution` / `correlation_search_space_smear_deviation` | 相関探索の範囲・解像度・平滑化          |
| `distance_variance_penalty` / `angle_variance_penalty`                                                                    | オドメトリから離れるほど与えるペナルティ重み   |
| `fine_search_angle_offset` / `coarse_search_angle_offset`                                                                 | 各モードでのスキャン角度探索範囲         |
| `coarse_angle_resolution`                                                                                                 | coarseモードでの角度探索ステップ      |
| `minimum_angle_penalty` / `minimum_distance_penalty`                                                                      | ペナルティ最小値（無限増加を防ぐ）        |
| `use_response_expansion`                                                                                                  | マッチしない場合に探索範囲を自動拡大するか    |

### 🧱 Occupancy Grid 関連
| パラメータ名                | 説明                         |
| --------------------- | -------------------------- |
| `min_pass_through`    | セルが占有 or 非占有になるのに必要な通過ビーム数 |
| `occupancy_threshold` | 占有と判断するためのビームヒット比率         |

### 🧵 その他
| パラメータ名                     | 説明                        |
| -------------------------- | ------------------------- |
| `stack_size_to_use`        | YAMLファイルのデシリアライズ用のスタックサイズ |
| `throttle_scans`           | 同期モードで何スキャンに1回処理するか       |
| `transform_publish_period` | `map→odom` のTFを何秒ごとに発行するか |
