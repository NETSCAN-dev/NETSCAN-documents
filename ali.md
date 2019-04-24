---
#### ali
---

+ description : search alignment first large scan area ( global ) in a view and then smaller scan area ( local )
  > For MaxPeaks=1 in global alignment, MinimumTracks are auto-incremented  
  > while scanning alignment parameter to suppress junk peaks.  
  > Rotation correction in position obtained for global alignment is also applied to angle correction.  
+ usage : ali *pl0* *pl1* [options]  
  > for ali-g and ali-l, *pl0* and *pl1* can be pl[-zone]  
+ options ( those are in **bold** must be given )
  - **--descriptor [event-descriptor](event-descriptor)**
  > set [event-descriptor](event-descriptor)  

  - --io fname-io  
  > override [event-descriptor](event-descriptor) entries  

  - **--rc runcard-file**
  > set runcard  

  - **--id-geom geom**
  > set geometry 0/1/...  

  - **--pc correction-map-pc-file**
  > set [[correction-map-pc(relative)|correction-map]] ( global )  

  - **--c correction-map-file**
  > set [[correction-map-align(relative)|correction-map]] ( local with global convoluted )  

  - **--dif correction-map-file**
  > set [[correction-map-dif(relative)|correction-map]] ( local only )  

  - **--view view-step view-overlap**  
    **--view view-list-file-name**
  > set view step and overlap by value or by [[view-list-file|view-list]]  
  > (*) 1st line of a view-list-file is used for global alignment.  

  - --search-mode 0/1/2 \[ fname-correction-map-i \]
  > set search mode ( default = 2 )  
  > 0 : independent search. use global alignment result as a basis in each process-view  
  > 1 : begin search in each process-view using fname-correction-map-i specified in this option.  
  > 2 : begin search in each prcess-view using closest process-view's result.  

  - --offset-xy offset-i offset-x offset-y
  > modify GlobalAlign::PositionWindow as  
  > when offset_i = 1 &rArr; xmin-offset_x, xmax-offset_x, ymin-offset_y, ymax-offset_y  
  > when offset_i = 2 &rArr; xmin+offset_x, xmax+offset_x, ymin+offset_y, ymax+offset_y  

  - --[filter-list](filter-list) [filter-list](filter-list) +1/-1
  > include (+1) or exclude (-1) [[[filter-list](filter-list)|[filter-list](filter-list)]]  

  - --filter-max-track-per-view value
  > avoid process in views having max-track-per-view track or more

  - --global xc yc
  > By default, first view is used for global alignment.  
  > This option forces a view which includes (xc,yc) to be used for global alignment.  
  > It is not applied when view-list-file-name is given.  

+ runcard template( m:/prg/netscan/ver-2011-03-01/rc/align-0.rc )

```
[GlobalAlign]
AngleCut        = 0 -0.40 0.40 -0.40 0.40   # First column controls how this region is parsed ... 0(disabled)/1(included)/-1(excluded)
PhCut           = 18
VolCut          = 3
MinimumTracks   = 10                # multiple threshold for faster search is not needed from this version on, for MaPeaks = 1. 
PositionWindow  = 2500.0 2500.0 2500.0 2500.0   # xmin xmax ymin ymax 
RotationWindow  = 0.020 0.002
PositionError   = 3.0               # 接続判定で許容する位置ズレは PositionError + AngleError x プレート間隔 /2 。
AngleError      = 0.020
ClusterSize     = 30.0              # When MaxPeaks > 1, obtained peaks which has distance < ClusterSize are clustered.  
IsBaseTrack     = 1
MaxPeaks        = 1                 # number of peaks to be searched. use 1 for usual cases. 
Dz              = -10.0 10.0 10.0   # 探索範囲の下限値、上限値、ステップ
ShiftAx         = -0.000 0.000 0.020
ShiftAy         = -0.000 0.000 0.020
FitMode         = 1                 # 1:offset＋rotation / 2:offset＋shrink / 3:offset＋rotation＋shrink
HistogramFileTemplate   =           # Dump histogram data if specified like hist%02d.dmp ( %02d is for found peaks index, see below for format spec ).  
Zproj           = 0.5               # 最近接ベース面間距離を Zproj : 1-Zproj に内分する z で alignment 探索を行う。default = 0.5

[LocalAlign]
AngleCut        = 0 -0.40 0.40 -0.40 0.40   # First column controls how this region is parsed ... 0(disabled)/1(included)/-1(excluded)
Enable_Affine   = 1
Enable_GapTune  = 1
SearchArea      = 50.0              # +/-SearchAreaの範囲内のピークを探す。
SearchAngle     = 0.020				# projectin angle difference for tracks to be connected
Significance    = 5.0
PHCUT           = 1 0.1
PHSUMCUT        = 0 0.1 0
VolCut          = 1 0.1 3
GhostFilter     = 1.0 0.007
BinWidth        = 6.0 0.015
PeakSizeMin     = 1.0 0.000         # minimum peak size ( $1+$2*gap ) to be searched 
PeakSizeMax     = 50.0              # maximum peak size to be serarched 
Gap             = 0.01 50.0 1.0     # 探索範囲 +/- ( 50.0 + 0.01xnominal-dz ) を 1.0micron ステップ
IsVerbose       = 0
AffineFitMode   = 1                 # 0 = full affine fit / 1 = shrink+rotation+shift
Zproj           = 0.5               # 最近接ベース面間距離を Zproj : 1-Zproj に内分する z で alignment 探索を行う。default = 0.5
```

  > 上記パラメータでのビン設定法  
  > w = sqrt( BinWidth::$1^2 + (BinWidth::$2x(0.5xnominal-gap))^2 );  
  > Nbin = (int)( (SearchArea+PeakSizeMax)*2.0 / w )+1;  
  > Nbin x Nbin のヒストグラムに詰めている。  
  
  > File specified by HistogramFileTemplate has [[dump-file format|ali-histogram]].  
