# AMCLでの`transform_tolerance`に関するエラーと挙動について
## 問題の概要
amcl使用中に以下のような`Extrapolation Error` が出ると, ロボットが自律移動中に「停止 → 少し進む → また停止」を繰り返すことがあります.  
```
[ERROR] [1747128117.289389728]: Extrapolation Error: Lookup would require extrapolation 0.015358691s into the future. Requested time 1747128117.254471779 but the latest data is at time
1747128117.239113092, when looking up transform from frame [odom]
to frame [map]
```
これは, `tf`で必要なtransformが「未来の時刻」を要求してしまい, **AMCLがtransformを取得できずに一時停止するために発生します.**

## 原因
`transform_tolerance`の設定が小さい
- transform_tolerance は, `map`→`odom`間のTransformをどれだけ「過去のデータに対して余裕を持たせるか」を設定するパラメータです

- デフォルトでは`0.1`秒ですが, タイミングずれや処理負荷で「未来の時間が要求される」ケースが発生します

- `transform_tolerance`の余裕が足りないと, lookup失敗→再計画→動作停止を繰り返します

## 解決策
`transform_tolerance`を十分に大きく設定する
- AMCLの`transform_tolerance`を, 例えば`0.5`秒に変更します

```
# amcl.yaml
amcl:
  transform_tolerance: 0.3
```

## 補足：再現しやすいケース
- 3D-LiDARの点群を`pointcloud_to_laserscan`で変換し, それをAMCLの入力トピックとすることによる遅延
- CPUが高負荷で処理が遅れている