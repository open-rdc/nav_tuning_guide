# AMCLのパラメータ調整方法_0

まず最初に, ここで紹介するパラメータを**必ず**`yaml`**ファイルを通してパラメータサーバに登録してください.**    
これをしないと, **自己位置がジャンプする/パーティクルが地図全体に広がる**などの問題が高確率で発生します.  
ここでは, そのパラメータの意味と設定方法を簡潔に解説します.  

---
## パラメータの説明([参照](http://wiki.ros.org/amcl#Parameters))
### `recovery_alpha_slow`
- **意味**: 低速平均重みフィルタの指数関数的減衰率 *(default: 0.0)*
### `recovery_alpha_fast`
- **意味**: 高速平均重みフィルタの指数関数的減衰率 *(default: 0.0)*

---
## 必須のパラメータの設定(yamlに書き込むだけ)
- `recovery_alpha_slow`, `recovery_alpha_fast`を`0.0`にして, loadしているyamlファイルに書き込んでください  

localization.yaml: 
```
recovery_alpha_slow: 0.0 
recovery_alpha_fast: 0.0  
```
❓ 「デフォルトが0.0なら, 書かなくても同じでは？」と思うかもしれません.  
しかし, **実際には書かないとAMCLが異常な動作をするケースがあります.**  
下図は, これらのパラメータをyamlファイルに書き込まなかった場合の様子です. **一瞬でパーティクルが地図全体に広まり, その後ジャンプします.** (個人的にはバグだと思っています)

![](images/big_jump.gif)  




---
## ✅ 次のステップへ
ここまではAMCLが最低限まともに動作するための設定です.  
本格的な調整はこのあとです.   
[次の調整へ (amcl_1.md)](amcl_1.md)