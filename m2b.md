---
#### m2b
---

distortion と shrink の補正がされた Micro Track のデータから Base Track を作る

+ description : build base track  
>   
> In currect connection algorhtm, only distortion and shrink correction to micro-track-angle in correction-map-file are used.  
> Other values given in correction-map-file are NOT used in this program.  
> They are used to obtain correct micro-track x,y,z when micro-tracks are read-out.  
>   
> \# of threads in m2b is NOW FIXED to be  
> `std::min(GetProcessorInfo().nCore,6)` for process-view loop, and  
> `0.65*GetProcessorInfo().nCore` for internal connection algorithm.  
> ~~To suppress memory usage in Corei7, try "set OMP_NUM_THREADS=4" or less.~~  
>   
> For view size larger than 32.5mm,  
> micro-track's position resolution in connection calculation  
> becomes x2 worse than basic resolution of 0.015micron.  
>
> Angle torelance is calculated as ErrAng + ErrDist + ErrShur x base-track-angle for x and y projections.  
>

+ usage : m2b pl[-zone] [options]
+ options ( those are in **bold** must be given )
  - **--descriptor [event-descriptor](event-descriptor.md)**
  > set [event-descriptor](event-descriptor.md)  

  - --io fname-io
  > override [event-descriptor](event-descriptor.md) entries  

  - **--rc [runcard](#runcard)-file**
  > set [runcard](#runcard)  

  - **--c [correction-map](correction-map.md)-file**
  > set [correction-map](correction-map.md) ( output of dc )  

  - **--view view-step view-overlap**  
    **--view view-list-file-name**  
  > set view step and overlap by value or by [view-list-file](mk_views.md/#view-list) 
  > see [[view-step and view-overlap definitions|[mk_views](mk_views.md)]].  
  > distortion correction を行う区画サイズの指定(指定の仕方は 2 通り) ステップとオーバーラップを指定するか、区画サイズをファイルに書く方法。

  - --[filter-list](filter-list.md) [filter-list](filter-list.md) +1/-1
  > include (+1) or exclude (-1) [[[filter-list](filter-list.md)|[filter-list](filter-list.md)]]  

  - --filter-phmin phmin
  > phmin filter  

  - --filter-max-track-per-view value
  > avoid process in views having max-track-per-view track or more  

  - --ang-shr ang-shr-base ang-shr-shift
  > &theta; += &theta; &times; (ang-shr-shift/ang-shr-base) for abs(&theta;)≤ang-shr-base  
  > &theta; += ang-shr-base for &theta; > +ang-shr-base  
  > &theta; -= ang-shr-base for &theta; < -ang-shr-base  
  > (*) implemented by Komatani probably fot HTS specific needs.  

#### FAQ
* Q. runcardの、ErrAng と ErrDist の違いは?<br>
  A. 現状違いはないので、ErrDistは常に0でよい<br>
* Q. runcardの、ErrShurは何か?<br>
  A. dcでいうところの、ErrorAngleの1つめのパラメータがErrAng、2つめのパラメータがErrShur。この名前は非常に分かりにくいので、今後パラメータ名は整理されるだろう。 
* Q. Radial-Lateral空間でカットできないのか?<br>
  A. m2bではできない。
* Q. どの[correction-map](correction-map.md)が使われたか、どこを見れば良い?<br>
  A. 分からない。[new_basetrack_format](new_basetrack_format.md)で挙げられている通り今後取得できるようになるだろう。
* Q. Z座標はどのように計算される?<br>
  A. 

#### runcard
runcard ( m:/prg/netscan/ver-2011-03-01/rc/m2b.rc )
```
[MT2BT]
Algorithm   = 0 # 0 => ConnectBase2 / 1 => ConnectLinklet
FACE1       = 0.0 0.0 0.0 0.000 0.000 1.00
FACE2       = 0.0 0.0 0.0 0.000 0.000 1.00
ErrAng      = 0.060
ErrDist     = 0.000
ErrShur     = 0.010
PHCUT       = 0 0.1 0
PHSUMCUT    = 0 0.1 0
VolCut      = 0 0.1 0
GhostFilter = 0 5 10 <- enable dr dt <- obsoleted
(*) 角度ズレ許容範囲 = ErrAng + ErrDist + ErrShur x ベース角
(*) ConnectLinklet を使う際の runcard は t2l 用の Mode = 0 を参照のこと
```
