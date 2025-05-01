# AMCLのパラメータ調整方法_0

まずここで紹介するパラメータを, yamlファイルを通じてパラメータサーバに登録しないと, 自己位置推定がすぐにジャンプするなどの問題が起こります.  
ここでは端的にそれらのパラメータの意味を説明し, その調整方法を述べます. 

---
## パラメータの説明([参照](http://wiki.ros.org/amcl#Parameters))
### `recovery_alpha_slow`
- **意味**: 低速平均重みフィルタの指数関数的減衰率 *(default: 0.0)*
### `recovery_alpha_fast`
- **意味**: 高速平均重みフィルタの指数関数的減衰率 *(default: 0.0)*

---
## 必須のパラメータの設定(yamlに書き込むだけ)
- `recovery_alpha_slow`, `recovery_alpha_fast`を0.0にして, loadしているyamlファイルに書き込んでください  

localization.yaml: 
```
recovery_alpha_slow: 0.0 
recovery_alpha_fast: 0.0  
```
default値と一緒ならわざわざyamlファイルに書き込む必要がないと思う方も多いでしょうが, これをするか否かで自己位置推定に大きな影響を与えます.  
下の図はyamlファイルに書き込まなかった場合です. 一瞬, パーティクルが地図全体に広まり, その後ジャンプします. 個人的にはバグだと考えています.  

![](images/big_jump.gif)  




---
本当の調整はここからです.  
[次の調整へ](amcl_1.md)