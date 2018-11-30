---
#### ali-l
---

+ description : search alignment in smaller scan area ( local ) in given process views  
  > Rotation correction in position obtained for global alignment is also applied to angle correction.  
+ usage : ali-l pl0[-zone0 pl1[-zone1] [options]  
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

  - **--view view-step view-overlap**  
    **--view view-list-file-name**
  > set view step and overlap by value or by [[process-view-list-file|view-list]]  
  > see [[view-step and view-overlap definitions|mk_views]].  

  - --search-mode 0/1 \[ fname-corrmap-i \]
  > This option is to give fname-corrmap-i as a starting point for alignment search.  
  > There is  no difference between mode 0 and 1 and  
  > They are remained just for backward compatibility of this option.  
  > Without fname-corrmap-i specified, search starts from unit affine transform.  
  > Mode = 2 was discarded.  

  - --filter-list filter-list +1/-1
  > include (+1) or exclude (-1) [[list-filter|filter-list]]  

  - --filter-max-track-per-view value
  > avoid process in views having max-track-per-view track or more

+ runcard template( m:/prg/netscan/ver-2011-03-01/rc/align-0.rc )

```
[LocalAlign]
AngleCut        = 0 -0.40 0.40 -0.40 0.40   # First column controls how this region is parsed ... 0(disabled)/1(included)/-1(excluded)
Enable_Affine   = 1
Enable_GapTune  = 1
SearchArea      = 50.0              # +/-SearchAreaの範囲内のピークを探す。
SearchAngle     = 0.020				# projection angle difference for tracks to be connected
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
AffineFitMode   = 1                 # 0 = full affine fit / 1 = shrink+rot+shift / 2 = shirnk+rot
Zproj           = 0.5               # 最近接ベース面間距離を Zproj : 1-Zproj に内分する z で alignment 探索を行う。default = 0.5
```

  > 上記パラメータでのビン設定法  
  > w = sqrt( BinWidth::$1^2 + (BinWidth::$2x(0.5xnominal-gap))^2 );  
  > Nbin = (int)( (SearchArea+PeakSizeMax)*2.0 / w )+1;  
  > Nbin x Nbin のヒストグラムに詰めている。  
