---
#### ali-g の runcard 指定に関するメモ
---

#### 探索範囲とステップの指定

- XY (位置)
> XY の探索範囲は、`PositionWindow = xmin xmax ymin ymax` で指定する。  
> `PositionWindow = dw` は `PositionWindow = -ww +ww -ww +ww` と等価。  
>  
> 探索ステップ（位置ズレヒストグラムのビン幅）を指定するには `InternalMode` を下記のいずれかに指定する。  
> `InternalMode = 0` あるいは未指定の場合、内部で計算した値を使うが ... これは、廃止予定。  
> `InternalMode = 1` では PositionError の値を使う。  
> `InternalMode = 2` sqrt( (PositionError)<sup>2</sup> + (AngleError&times;dz)<sup>2</sup> ) の値を使う。
>> dz は parts.kar と chamber.kar で指定される nominal なプレート間距離。（この値を確認するには conv2st を使用）  

- ROT (回転)
> `RotationWindows = rmax dr` で [-rmax,+rmax] の範囲を dr のステップで探索する。  

- DT (角度ズレ)
> `ShiftAx = dtmin dtmax dt` と `ShiftAy = dtmin dtmax dt` で、各々の範囲をとステップを指定する。

- DZ (ギャップ)
> `Dz = dzmin dzmax ddz` で [-dzmin,+dzmax] の範囲を ddz のステップで探索する。

#### 探索条件の指定

- 位置ズレを評価する Z 面の指定
> `Zproj = f` で、最近接ベース面間距離を f : 1-f に内分する Z 面で位置ズレ評価を行う。 default = 0.5

- 探索に使う飛跡の選択
> `PhCut`,`VolCut`,`AngleCut`  

#### 探索成否判定

- `MinimumTracks` 以上の飛跡がつながる事。かつ、見つけたピークが `Significance` 以上である事。  

#### その他

- FitMode
> `MinimumTracks` と `Significance` の条件を満たす位置関係で、`PositionError` と `AngleError` の許容範囲でつながる飛跡集団を使い、  
> 位置関係をフィットした結果を出力しているが、そのフィットの自由度を指定する。  
> `FitMode = 0:offset / 1:offset＋rotation / 2:offset＋shrink / 3:offset＋rotation＋shrink`  

- AngleError
> 位置ズレ探索（ヒストグラム作成）とフィットの際に、飛跡の接続判定に使う。  

- GhostFilter
> 位置関係の探索に使う飛跡に適用しておく GhostFilter のパラメータ指定。

- ヒストグラム作成 ( 未リリース )  
> `Histo = mode wbin xmin xmax ymin ymax`   
> `mode = 0` では、、ピークを見つけた時の ROT,DZ,DT での、探索範囲内の XY のヒストグラムを出力する。  
> `mode = 1` では、見つけたピークを中心にした位置ズレヒストグラムを、[xmin,xmax], [ymin,ymax] の範囲で、ビン幅 wbin で出力する ( 単位は &mu;m )。  
> 出力するファイル名は --histo fname-histo で指定する。  

#### 廃止予定

- MaxPeaks  
> MaxPeaks までのピークを出力する。原則 `MaxPeaks = 1` で使うべし。  

- IsBaseTrack

- ClusterSize
> MaxPeaks = 1 の場合は意味なし。  
> MaxPeaks > 1 の場合に、位置ズレがこの値以下のピークをクラスタリングする。
