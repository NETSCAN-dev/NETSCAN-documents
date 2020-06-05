---
#### ali-g
---

プレート間の位置関係を求める。[ali-l](ali-l.md)もある。

計算できるパラメータは、位置ずれ(dx dy)、角度ずれ(dax day)、位置の回転(theta)、およびギャップ(dz)である。

+ description : search alignment in large scan area ( global ) in given process-views  
  > For MaxPeaks=1, MinimumTracks are auto-incremented  
  > while scanning alignment parameter to suppress junk peaks.  
  > Rotation correction in position obtained is also applied to angle correction.  
+ usage : ali-g pl0[-zone0] pl1[-zone1] [options]  
  > default value for zone0/1 are 0  
+ options ( those are in **bold** must be given )
  - **--descriptor [event-descriptor](event-descriptor.md)**
  > set [event-descriptor](event-descriptor.md)  

  - --io fname-io  
  > override [event-descriptor](event-descriptor.md) entries  

  - **--rc runcard-file**
  > set runcard  

  - **--id-geom geom**
  > set geometry 0/1/...  

  - **--c [correction-map](correction-map.md)-file [w/a]**
  > set [[[correction-map](correction-map.md)-align(relative)|[correction-map](correction-map.md)]]  
  > result is over-written (w) or appended (a) to this file ( default is to over-write ).  
  > w:上書きモード, a:追記モード

  - **--view view-step view-overlap [view-select-mode]**  
    **--view view-list-file-name [view-select-mode]**
  > 処理区画 (view) を view-step と view-overlap の値を指定して自動生成するか、ファイル [view-list-file](view_list.md) で指定する。  
  > view-step と view-overlap の定義は [mk_views](mk_views.md) のオプション --views の記述を参照の事。  
  > view-select-mode は、処理対象を、指定した view リストの一部だけに制限したい場合に使う。  
  > view-select-mode = 1 は中央の 1 view だけを、view-select-mode = 2 は中央と四隅の計 5 view だけを処理する。  

  - --search-mode 1/2 \[ [correction-map](correction-map.md)-i \]
  > このオプションで探索基点の決定法 (search-mode) と、基点の決定に使う [correction-map](correction-map.md)-i を指定する。  
  > search-mode = 1 では、常に [correction-map](correction-map.md)-i 中の最近接 view を基点とする。  
  > search-mode = 2 では、探索に成功した view リスト中の最近接 view を基点とするが、  
  > 探索に成功した view が得られる迄は [correction-map](correction-map.md)-i から基点を選ぶ。  
  > [correction-map](correction-map.md)-i が未指定の場合は無変換パラメータを基点とする。  
  > search-mode = 0 は search-mode = 1 と同じ動作をする ( to be obsoleted )。  

  - --offset-xy offset-i offset-x offset-y
  > modify GlobalAlign::PositionWindow as  
  > when offset_i = 1 &rArr; xmin-offset_x, xmax-offset_x, ymin-offset_y, ymax-offset_y  
  > when offset_i = 2 &rArr; xmin+offset_x, xmax+offset_x, ymin+offset_y, ymax+offset_y  

  - --[filter-list](filter-list.md) [filter-list](filter-list.md) +1/-1
  > include (+1) or exclude (-1) [[[filter-list](filter-list.md)|[filter-list](filter-list.md)]]  

  - --filter-max-track-per-view value
  > avoid process in views having max-track-per-view track or more

  - --filter-area area-list-file
  > For each view, use tracks in areas given in area-list-file only.  
  > This option will be useful for whole plate (pc) alignment.  
  > First 4 columns of each line in area-list-file should be xmin, xmax, ymin and ymax.  


#### FAQ
* Q. 最初ビームトラックを使って位置と角度ずれを補正、次に角度の大きな飛跡を使ってギャップを補正みたいなことはできる?<br>
  A. 原理的にできるはずだが、実用上難しい
* Q. ベーストラックとマイクロトラックの角度差が小さい飛跡だけ使うことはできるのか?<br>
  A. できない。b_filter等でもできないので、dumpしてカットしてbvxxを作り直すしかない
* Q. 位置ズレの初期値を与えるにはどうしたらいい?<br>
  A. 
* Q. ベーストラック間のギャップの値の設計値が既知であれば、AngleErrorは設定する必要はないのでは?<br>
  A. そのとおり。0でよい。
* Q. Significanceってどうやって計算するの?<br>
  A. 
* Q. ClusterSizeって何?<br>
  A. 
* Q. 乾板の伸び縮みは計算できる?<br>
  A. 各区画単位で、アフィンパラメータでフィットするときに、計算していることになってる。

### runcard
+ runcard template( m:/prg/netscan/ver-2011-03-01/rc/align-0.rc )


#### 例
```
[GlobalAlign]
AngleCut        = 0 -0.40 0.40 -0.40 0.40   # 後述
PhCut           = 18
VolCut          = 3
MinimumTracks   = 10                
# multiple threshold for faster search is not needed from this version on, for MaPeaks = 1. 
# MaPeaks = 1の場合、このバージョンから高速検索のための複数のしきい値は必要ありません。
PositionWindow  = -2500.0 +2500.0 -2500.0 +2500.0   # xmin xmax ymin ymax
#single value is treated as -value +value -value +value
#位置ずれの窓の大きさ。パラメータが一つの場合は自動的に |dx|< value かつ |dy| < value に展開する。
RotationWindow  = 0.020 0.002       # 回転探索範囲(±)、ステップ
PositionError   = 3.0               # 接続判定で許容する位置ズレは PositionError + AngleError x Gap /2 。
AngleError      = 0.020             # Gapに依存する位置ずれの許容値
Significance    = 10.0              # minimum significance for peak search. 
ClusterSize     = 30.0              # When MaxPeaks > 1, obtained peaks which has distance < ClusterSize are clustered.  
IsBaseTrack     = 1
MaxPeaks        = 1                 
# number of peaks to be searched. use 1 for usual cases. 
Dz              = -10.0 10.0 10.0   # 探索範囲の下限値、上限値、ステップ
ShiftAx         = -0.000 0.000 0.020 # 探索範囲の下限値、上限値、ステップ この設定では探索しない
ShiftAy         = -0.000 0.000 0.020 # 探索範囲の下限値、上限値、ステップ この設定では探索しない
FitMode         = 1                 # 1:offset＋rotation / 2:offset＋shrink / 3:offset＋rotation＋shrink
HistogramFileTemplate   =           # 後述
Zproj           = 0.5               # 最近接ベース面間距離を Zproj : 1-Zproj に内分する z で alignment 探索を行う。default = 0.5
GhostFilter     = 1.0 0.007         # [b_filter](b_filter.md)の *--ghost* と同じ機能。
InternalMode    = 2                 
#InteranlMode=1/2 を推奨。0 は廃止予定。
#* 0:従来動作の内部計算
#* 1:PositionError をビン幅とする
#* 2:PositonError+=nominal-dz*AngleError をビン幅とする。
```

#### AngleCut
First column controls how this region is parsed ... 0(disabled)/1(included)/-1(excluded)

例1: |tanθx|<0.1 かつ |tanθy|<0.1を使う

> 1 -0.10 0.10 -0.10 0.10

例2: |tanθx|>0.1 または |tanθx|>0.1を使う

> -1 -0.10 0.10 -0.10 0.10

#### HistogramFileTemplate

dump histogram data if specified like hist-%02d-%02d-%05d-%d.dmp
( format specifiers are for pl0,pl1,view-id,peak-id in the file-name. see below for format spec. )
File specified by HistogramFileTemplate has [dump-file format](ali-histogram.md).


hist-％02d-％02d-％05d-％d.dmpのように指定した場合、ヒストグラムデータをダンプ出力する。
フォーマット指定子はファイル名の中のpl0、pl1、view-id、peak-idのためのもの。フォーマット指定については下記を参照。
HistogramFileTemplateで指定されたファイルは [dump-file format](ali-histogram.md)を含む。

