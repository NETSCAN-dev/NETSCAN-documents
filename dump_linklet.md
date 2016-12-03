---
#### dump_linklet - dump linklet as an input for connect -
---

+ usage : dump_linklet [ options ] > ../[geom]/linklet/linklet-dump-[pos1]-[pos2].lst  
> To reproduce corrdinate system used in linklet connection by t2l, use the same options as of t2l.  
> Be careful that there could be missing linklets from dump-output, when "--view" option values are different from those given to t2l.  

+ options ( those are in **bold** must be given )  
  - **--pos pos0 pos1**
  > limit dump linnklets between pos0 - pos1, otherwise all  

  - **--descriptor event-descriptor**
  > set event descriptor  

  - --io fname-io
  > override event descriptor entries  

  - **--geom geom**
  > set geometry 0/1/...  

  - **--format 0/1/2**
  ```
    columns description
    01-02   pos0,rawid0
    03-04   pos1,rawid1
    05-09   ph0,ax0,ay0,x0,y0 for 1st micro/base track
    10-14   ph1,ax1,ay1,x1,y1 for 2nd micro/base track
    15-16   z0,z1 z for 1st and 2nd tracks
    17      proj z where dr was evaluated
    18-19   xc,yc relative xy from process-view center ( to be obsoleted )
    20-24   pos,col,row,zone,isg for 1st micro track
    25-29   pos,col,row,zone,isg for 2nd micro track ( all 0 for micro-any linklet )
    30-34   pos,col,row,zone,isg for 3rd micro track
    35-39   pos,col,row,zone,isg for 4th micro track ( all 0 for any-micro linklet )
    40-41   0,0 obsoleted
    42-45   rawid,ph,ax,ay for 1st micro track
    46-49   rawid,ph,ax,ay for 2nd micro track
    50-53   rawid,ph,ax,ay for 3rd micro track
    54-57   rawid,ph,ax,ay for 4th micro track
    58-59   dx,dy (dx,dy) at zproj
    (*) columns 42 - 59 are when format=2
  ```
  > 0 is for binary output ( see [[class description|netscan-data-types-ui]] or m:/prg/netscan/sample/2/binary-io/ )  

  - **--rc runcard-file**
  > set runcard used when linklets are built  

  - **--c corrmap-file [ corrmap-dist ]**  
    **--c - corrmap-dist**
  > set corrmap-align ( relative corrmap ) and corrmap-dist ( required to connect micro tracks ),  
  > used when linklets are built  

  - **--view view-step view-overlap**  
    **--view view-list-file-name**  
  > set view step and overlap by value or by [[process-view-list-file|view-list]]  

  - --window xmin xmax ymin ymax
  > limit position of tracks to be connected  

  - --filter-list file-name mode-0 mode-1 mode-x
  > mode-0 : include(+1) / exclude(-1) for pos0  
  > mode-1 : include(+1) / exclude(-1) for pos1  
  > mode-x : both-tracks(1) / at-least-one-track(2) must be 'accepted'.  
  > caution **'accepted' means 'not in the list' for exclude mode.**  

  - --warning-level 0/1
  > set 0 to suppress non-fatal warning messages ( old versions may have default value 1 ).  

  - --cache $1 $2 $3 $4 $5 $6
  > set cache size ( values set to 0 are remain unchanged ).  
  > $1 : high cache pages for micro-vxx  
  > $2 : high cache pages for base-vxx  
  > $3 : high cache pages for linklet-vxx  
  > S4 : high cache blocks for micro-vxx  
  > $5 : high cache blocks for base-vxx  
  > $6 : high cache blocks for linklet-vxx  

  ```options --v and --c-view are obsoleted ( use --format 1 instead of --v ).```
