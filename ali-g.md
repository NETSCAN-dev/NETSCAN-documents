---
#### ali-g
---

+ description : search alignment in large scan area ( global ) in given process-views  
  > For MaxPeaks=1, MinimumTracks are auto-incremented  
  > while scanning alignment parameter to suppress junk peaks.  
  > Rotation correction in position obtained is also applied to angle correction.  
+ usage : ali-g pl0[-zone0] pl1[-zone1] [options]  
  > default value for zone0/1 are 0  
+ options ( those are in **bold** must be given )
  - **--descriptor event-descriptor**
  > set event descriptor  

  - --io fname-io  
  > override event descriptor entries  

  - **--rc runcard-file**
  > set runcard  

  - **--id-geom geom**
  > set geometry 0/1/...  

  - **--c corrmap-file [w/a]**
  > set [[corrmap-align(relative)|correction-map]]  
  > result is over-written (w) or appended (a) to this file ( default is to over-write ).  

  - **--view view-step view-overlap [view-select-mode]**  
    **--view view-list-file-name [view-select-mode]**
  > set view step and overlap by value or by [[process-view-list-file|view-list]]  
  > see [[view-step and view-overlap definitions|mk_views]].  
  > view-select-mode ( optional ) can be specified to limit views used for process as  
  > 1 = one view in center / 2 = one view in center and 4 views in corners.  

  - **--search-mode 0/1 \[ fname-corrmap-i \]**
  > This option is to give fname-corrmap-i as a starting point for alignment search.  
  > There is  no difference between mode 0 and 1 and  
  > They are remained just for backward compatibility of this option.  
  > Without fname-corrmap-i specified, search starts from unit affine transform.  
  > Mode = 2 was discarded.  

  - --offset-xy offset-i offset-x offset-y
  > modify GlobalAlign::PositionWindow as  
  > when offset_i = 1 &rArr; xmin-offset_x, xmax-offset_x, ymin-offset_y, ymax-offset_y  
  > when offset_i = 2 &rArr; xmin+offset_x, xmax+offset_x, ymin+offset_y, ymax+offset_y  

  - --filter-list filter-list +1/-1
  > include (+1) or exclude (-1) [[list-filter|filter-list]]  

  - --filter-max-track-per-view value
  > avoid process in views having max-track-per-view track or more

  - --filter-area area-list-file
  > For each view, use tracks in areas given in area-list-file only.  
  > This option will be useful for whole plate (pc) alignment.  
  > First 4 columns of each line in area-list-file should be xmin, xmax, ymin and ymax.  

+ runcard template( m:/prg/netscan/ver-2011-03-01/rc/align-0.rc )

```
[GlobalAlign]
AngleCut        = 0 -0.40 0.40 -0.40 0.40   # First column controls how this region is parsed ... 0(disabled)/1(included)/-1(excluded)
PhCut           = 18
VolCut          = 3
MinimumTracks   = 10                # multiple threshold for faster search is not needed from this version on, for MaPeaks = 1. 
PositionWindow  = -2500.0 +2500.0 -2500.0 +2500.0   # xmin xmax ymin ymax ( single value is treated as -value +value -value +value )
RotationWindow  = 0.020 0.002
PositionError   = 3.0               # 接続判定で許容する位置ズレは PositionError + AngleError x プレート間隔 /2 。
AngleError      = 0.020
Significance    = 10.0              # minimum significance for peak search. 
ClusterSize     = 30.0              # When MaxPeaks > 1, obtained peaks which has distance < ClusterSize are clustered.  
IsBaseTrack     = 1
MaxPeaks        = 1                 # number of peaks to be searched. use 1 for usual cases. 
Dz              = -10.0 10.0 10.0   # 探索範囲の下限値、上限値、ステップ
ShiftAx         = -0.000 0.000 0.020
ShiftAy         = -0.000 0.000 0.020
FitMode         = 1                 # 1:offset＋rotation / 2:offset＋shrink / 3:offset＋rotation＋shrink
HistogramFileTemplate   =           # dump histogram data if specified like hist-%02d-%02d-%05d-%d.dmp
                                    # ( format specifiers are for pl0,pl1,view-id,peak-id in the file-name. see below for format spec. )
Zproj           = 0.5               # 最近接ベース面間距離を Zproj : 1-Zproj に内分する z で alignment 探索を行う。default = 0.5
GhostFilter     = 1.0 0.007
InternalMode    = 2                 # 0:従来動作の内部計算 / 1:PositionError をビン幅とする / 2:PositonError+=nominal-dz*AngleError をビン幅とする。
                                    # InteranlMode=1/2 を推奨。0 は廃止予定。
```

  > File specified by HistogramFileTemplate has [[dump-file format|ali-histogram]].  
