---
#### mk_corrmap
---

#### description
> 補正マップの操作に関して、下記の機能を実装している。  
> 機能追加のためにコマンドラインの書式を大きく変更したため、**旧版との互換性は無い。**  
> - 隣接プレート間の[相対補正マップ](correction-map.md)を積分して、複数枚飛ばしのプレート間の[相対補正マップ](correction-map.md)を作成する。  
> - [相対補正マップ](correction-map.md)から[絶対補正マップ](correction-map.md)を作成する。  

#### usage
> mk_corrmap *correction-map-input* *correction-map-output* [ options ]  

#### options ( どれか1つは必須 )
- **--int *[event-descriptor](event-descriptor.md)* *pos_1* *pos_2* ... *pos_n***
> 隣接プレート間の相対補正マップ ( pos_1 ... pos_n ) を積分して、pos_1 と pos_n の間の相対補正マップを作成する。  

- **--r2a *pos_1* *pos_2***  
> *pos_1* と *pos_2* の間の相対補正マップから各々の絶対補正マップを作成する。  
> *pos_1* の絶対補正マップは無変換であり、*pos_2* の絶対補正マップでは領域座標を逆変換(外接四角形)している。  

#### 旧版互換性のための仕様
> 下記（これまでのコマンド書式）で --int と同じ動作となる。  
> mk_corrmap *correction-map-rel-input* *correction-map-rel-output* *[event-descriptor](event-descriptor.md)* *pos_1* *pos_2* ... *pos_n*  
