---
#### dc
---

+ description : search shrink and distortion. no correction tried for FulcrumDz in runcard
+ usage : dc pl[-zone] [options]
+ options ( those are in **bold** must be given )
  - **--descriptor event-descriptor**
  > set event descriptor  

  - --io fname-io
  > override event descriptor entries  

  - **--rc fname-runcard**
  > set runcard  

  - **--o corrmap-file [w/a]**
  > set [[corrmap-dc(absolute)|correction-map]]  
  > result is over-written (w) or appended (a) to this file ( default is to over-write ).  

  - **--view view-step view-overlap**  
    **--view view-list-file-name**
  > set view step and overlap by value or by [[process-view-list-file|view-list]]  
  > see [[view-step and view-overlap definitions|mk_views]].  

  - --c corrmap-file
  > set corrmap file used as a starting point for each view  

  - --search-mode 0/1/2
  > set search mode ( default = 0 )  
  > 0 : independent search. use first values of ErrDist and ErrShur  
  > 1 : begin search in each view using correction-map given in --c option  
  > 2 : begin search in each view using closest view's result. use second values of ErrDist and ErrShur if they exists  

  - --filter-angle +1/-1 axmin axmax aymin aymax
  > use angle regions (+1) or exclude angle regions(-1) in parameter search  
  > multiple entries are accepted and includes(+1) are first evaluated and then excludes(-1)  

  - --filter-list filter-list +1/-1
  > include (+1) or exclude (-1) list-filter  

  - --filter-phmin phmin
  > phmin filter  

  - --filter-max-track-per-view value
  > avoid process in views having max-track-per-view track or more  

  - --ang-shr ang-shr-base ang-shr-shift  
  > abs(&theta;) &le; ang-shr-base &rArr; &theta; += &theta; &times; (ang-shr-shift/ang-shr-base)  
  > &theta; > +ang-shr-base &rArr; &theta; += ang-shr-base  
  > &theta; < -ang-shr-base &rArr; &theta; -= ang-shr-base  
  > (*) implemented by Komatani probably fot HTS specific needs.  

  - --num-threads num1 num2
  > num1 : # of threads used to process views  
  > num2 : # of threads used in scan parameter ( not valid for current open-mp )  

  - --debug 1 
  > set verbose console output  

+ runcard template( m:/prg/netscan/ver-2011-03-01/rc/dc.rc )
  ```
[DC]
#
# FACE1,FACE2 で探索開始位置 dx dy dz dax day shrink_inv dz_fulcrum を指定する。
# dx dy dz dz_fulcrum は dc 内では探索しない。
# 座標系はプレート座標系。
# dz_fulcrum はベース表面（不動面）の補正値であり、現状のアルゴリズム ( ConnectBase ) では下記の通り補正する。
#   FACE1 MicroTrack::z2 -> MicroTrack::z2+FACE1::dz_fulcrum
#   FACE2 MicroTrack::z1 -> MicroTrack::z1+FACE2::dz_fulcrum
# 現状では opera 標準に合わせたプレート座標系であり、対物レンズ側が FACE2、Z 軸は上向き正である。
#
FACE1           = 0.0 0.0 0.0 0.000 0.000 1.00 0.0  # dx dy dz dax day shrink_inv dz_fulcrum
FACE2           = 0.0 0.0 0.0 0.000 0.000 1.00 0.0  #
ErrPos          = 10.0          # ベース中央での micro-track の位置ズレ ( r1074 以降は無用 )
ErrAng          = 0.040         # ベース中央での micro-track の角度ズレ ( r1074 以降は無用 )
ErrDist         = 0.100         # Distortion の探索範囲
ErrShur         = 0.200         # Shrink の探索範囲
ErrorAngleX1    = 0.040 0.000   # error-constant error-slope <= ErrAng の後継
ErrorAngleY1    = 0.040 0.000   #
ErrorAngleX2    = 0.040 0.000   #
ErrorAngleY2    = 0.040 0.000   #
PHCUT           = 0 0.1 0       #
PHSUMCUT        = 1 0.1 21      #
VolCut          = 1 0.1 3       #
AngleStep       = 0.050 0.010 0.002     # distortion 探索 step ( 絞込み探索対応 2015-10-04 より ) 
ShrinkStep      = 0.020 0.010 0.002     # shrink 探索 step
                                          # For second and subsequent AngleStep(ShrinkStep),  
                                          # a search area is defined as 
                                          # +/- previous value of AngleStep(ShrinkStep).  
MaxTracks       = 1000          # 探索に使用する１面トラック数の上限。これを超える場合、間引きを行う。
Significance    = 3.0           # significance がこの値以上の区画を探索成功と判断。失敗区画は出力しない。
MaxAng          = 0.8           # 探索に使用するトラックの最大角度
DumpDebugInfoFileName = xxx.xxx # Enable debug-info output to a file "xxx.xxx",  
                                  # when non-null value for this entry is specified.  
                                  # Format of this file is ...  
                                  # $1,$2  : base-track ax,ay  
                                  # $3,$4  : face 1 micro-track ax,ay corrected  
                                  # $5,$6  : face 2 micro-track ax,ay corrected  
                                  # $7,$8  : face 1 micro-track ax,ay raw  
                                  # $9,$10 : face 2 micro-track ax,ay raw  
  ```
