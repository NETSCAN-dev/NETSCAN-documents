---
#### m2b - build base track - </h4>
---

+ description : build base track using ConnectBase2 / ConnectLinklet / IConnectLinklet
+ usage : m2b pl[-zone] [ options ]
+ options ( those are in **bold** must be given )
  - **--descriptor event-descriptor**
  > set event descriptor  

  - --io fname-io
  > override event descriptor entries  

  - **--rc runcard-file**
  > set runcard  

  - **--c corrmap-file**
  > set corrmap ( output of dc )  

  - **--view view-step view-overlap**  
    **--view view-list-file-name**  
  > set view step and overlap by value or by [[process-view-list-file|view-list]]  

  - --exclude xmin xmax ymin ymax
  > register a region to be excluded for process. multiple regions can be registerd  

  - --position xmin xmax ymin ymax
  > limit base-tracks in position  

  - --angle axmin axmax aymin aymax
  > limit base-tracks in angle  

  - --filter-list filter-list +1/-1
  > include (+1) or exclude (-1) [[list-filter|filter-list]]  

  - --filter-phmin phmin
  > phmin filter  

  - --filter-max-track-per-view value
  > avoid process in views having max-track-per-view track or more  

  - --global xc yc
  >  

  - --ang-shr ang-shr-base ang-shr-shift
  > &theta; += &theta; &times; (ang-shr-shift/ang-shr-base) for abs(&theta;)≤ang-shr-base  
  > &theta; += ang-shr-base for &theta; > +ang-shr-base  
  > &theta; -= ang-shr-base for &theta; < -ang-shr-base  
  > (*) implemented by Komatani probably fot HTS specific needs.  

  - --hist
  > enable histogram ( obsoleted )  

  - --o prefix
  > set histogram file prefix ( obsoleted )  

+ memory usage
> To suppress memory usage in Corei7, try "set OMP_NUM_THREADS=4" or less. 

+ runcard ( m:/prg/netscan/ver-2011-03-01/rc/m2b.rc )
  ```
#
# ver-2006-11-05 の mt2bt を --exec-local-correction で動かす場合は、
# ErrDist と ErrShur はそれぞれの補正パラメータの探索範囲として使われ、
# トラックのつなぎの際には、それぞれの誤差は ErrDist = 0.020, ErrShur = 0.01 
# ( これらの値は、それぞれの補正パラメータを探索する際のステップ ) 
# としてつなぎを行っていた。
# これとは異なり m2b では ErrDist と ErrShur はつなぎの許容範囲計算での
# それぞれのパラメータの誤差として使われる。
#
[MT2BT]
Algorithm   = 0 # 0 => ConnectBase2 / 1 => ConnectLinklet / 2 => IConnectLinklet
FACE1       = 0.0 0.0 0.0 0.000 0.000 1.00
FACE2       = 0.0 0.0 0.0 0.000 0.000 1.00
ErrPos      = 10.0;
ErrAng      = 0.060
ErrDist     = 0.000
ErrShur     = 0.010
PHCUT       = 0 0.1 0
PHSUMCUT    = 0 0.1 0
VolCut      = 0 0.1 0
Algorithm   = 0/1/2 ConnectBase2/ConnectLinklet/IConnectLinklet
GhostFilter = 0 5 10 <- enable dr dt
  ```
