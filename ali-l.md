---
#### ali-l
---

#### description
> プレート間の位置関係を求める。[ali-g](ali-g.md) と違って探索パラメータ範囲を総当たりしてはいないが、その分高速動作する。  

#### usage
> #### ali-l pl0[-zone0 pl1[-zone1] [options]  
> <br>
> default value for zone0/1 are 0  
>

#### options ( those are in **bold** must be given )
- **--descriptor [event-descriptor](event-descriptor.md)**
> set [event-descriptor](event-descriptor.md)  

- --io fname-io  
> override [event-descriptor](event-descriptor.md) entries  

- **--rc runcard-file**
> set runcard  

- **--id-geom geom**
> set geometry 0/1/...  

- **--c [correction-map-file](correction-map.md) [w/a]**
> 探索結果である [相対補正マップ](correction-map.md) のファイル名を指定する。  
> 上書き(w) か追記(a) を指定可。デフォルトは上書き。  

- **--view view-step view-overlap**  
  **--view view-list-file-name**
> [処理区画](view-list.md) を、view-step と view-overlap で指定して自動生成するか、  
> [mk_views](mk_views.md) で作成した [区画リスト](view-list.md) のファイル名で指定する。  
> ファイル名での指定を推奨。  

- --search-mode 1/2 \[ [correction-map-i](correction-map.md) \]
> このオプションで探索基点の決定法 (search-mode) と、基点の決定に使う [correction-map-i](correction-map.md) を指定する。  
> search-mode = 1 では、常に [correction-map-i](correction-map.md) 中の最近接区画を基点とする。  
> search-mode = 2 では、探索に成功した区画リスト中の最近接区画を基点とするが、  
> 探索に成功した区画が得られる迄は [correction-map-i](correction-map.md) から基点を選ぶ。  
> [correction-map-i](correction-map.md) が未指定の場合は無変換パラメータを基点とする。  
> search-mode = 0 は search-mode = 1 と同じ動作をする ( 削除予定 )。  

- --[filter-list](filter-list.md) [filter-list](filter-list.md) +1/-1
> include (+1) or exclude (-1) [[[filter-list](filter-list.md)|[filter-list](filter-list.md)]]  

- --filter-max-track-per-view value
> avoid process in views having max-track-per-view track or more

#### FAQ
[ali-gのFAQ](ali-g.md#FAQ) も参照

### runcard example
共通のパラメータについては、[ali-gのruncard](ali-g.md#runcard)も参照。

```
[LocalAlign]
AngleCut        = 0 -0.40 0.40 -0.40 0.40   # ali-g.md を参照
Enable_Affine   = 1
Enable_GapTune  = 1
SearchArea      = 50.0              # +/-SearchAreaの範囲内のピークを探す。
SearchAngle     = 0.020             # projection angle difference for tracks to be connected
Significance    = 5.0
PHCUT           = 1 0.1
PHSUMCUT        = 0 0.1 0
VolCut          = 1 0.1 3
GhostFilter     = 1.0 0.007
BinWidth        = 6.0 0.015
PeakSizeMin     = 1.0 0.000         # minimum peak size ( $1+$2*gap ) to be searched 
PeakSizeMax     = 50.0              # maximum peak size to be serarched 
									# 期待される位置ズレよりも大きくするのがお勧め。
Gap             = 0.01 50.0 1.0     # 探索範囲 +/- ( 50.0 + 0.01xnominal-dz ) を 1.0micron ステップ
IsVerbose       = 0
AffineFitMode   = 1                 # 0 = full affine fit / 1 = shrink+rot+shift / 2 = rot+shift
Zproj           = 0.5               # 最近接ベース面間距離を Zproj : 1-Zproj に内分する z で alignment 探索を行う。default = 0.5
PatchMe         = 1                 # 0 = 従来動作(デフォルト) / 1 = 新規(こちらに統合したい。詳細は notes の algorithm change in dc,ali-i を参照の事。)
```

> #### 上記パラメータと、内部で使われるヒストグラムのビン幅等の関係メモ  
> c0 = BinWidth::$1、c1 = BinWidth::$2、dz = nominal-gap として  
> w = sqrt( c0<sup>2</sup> + ( c1 &times; dz &times; 0.5 )<sup>2</sup> );  
> Nbin = (int)( SearchArea+PeakSizeMax &times; 2 / w )+1;  
> Nbin &times; Nbin のヒストグラムに詰めている。  
