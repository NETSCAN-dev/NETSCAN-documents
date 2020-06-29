---
#### dump_linklet
---

#### description
> dump linklet info, which should be used as input to make chain  

#### usage
> #### dump_linklet [options] > *../0/linklet/linklet-dump-01-02.lst*  
> <br>
> To reproduce corrdinate system used in linklet connection by t2l, use the same options as of t2l.  
> Be careful that there could be missing linklets from dump-output, when "--view" option values are different from those given to t2l.  

#### options ( those are in **bold** must be given )  
- **--pos pos0 pos1**
> limit dump linnklets between pos0 - pos1, otherwise all  

- **--descriptor [event-descriptor](event-descriptor.md)**
> set [event-descriptor](event-descriptor.md)  

- --io fname-io
> override [event-descriptor](event-descriptor.md) entries  

- **--geom geom**
> set geometry 0/1/...  

- **--format 0/1/2 [customized_netscan_data_types_ui.h]**
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
    60-69   ph,ax,ay,x,y,z,z1,z2,px,py for 1st micro track ( z1,z2,px,py are valid when --access-micro )
    70-79   ph,ax,ay,x,y,z,z1,z2,px,py for 2nd micro track
    80-89   ph,ax,ay,x,y,z,z1,z2,px,py for 3rd micro track
    90-99   ph,ax,ay,x,y,z,z1,z2,px,py for 4th micro track
    (*) columns 42 - 99 are when format=2.
    (*) columns 60 - 99 appears when option --access-micro is specified.
```
> --format 0 is for binary output ( see [[netscan_data_types_ui.h|netscan-data-types-ui]] or m:/prg/netscan/sample/2/binary-io/ )  
> each record is linklet_t, but, with option --access-micro, micro_track_t[4] is appended.  

- --access-micro
> force to read micro-track-vxx file to obtain z1,z2,px,py which are only available in micro-track-vxx.  
> this option enables columns 60 - 99 in text mode output and adds micro_track_t[4] in binary mode output.  

- **--rc runcard-file**
> set runcard used when linklets are built  

- **--c correction-map-file [ correction-map-dist ]**  
  **--c - correction-map-dist**
> set correction-map-align ( relative correction-map ) and correction-map-dist ( required to connect micro tracks ),  
> used when linklets are built  

- **--view view-step view-overlap**  
  **--view view-list-file-name**  
> [処理区画リスト](view-list.md) を、view-step と view-overlap で指定して自動生成するか、  
> [mk_views](mk_views.md) で作成した [処理区画リスト](view-list.md) のファイル名で指定する。  
> ファイル名での指定を推奨。  

- --window xmin xmax ymin ymax
> limit position of tracks to be connected  

- --[filter-list](filter-list.md) file-name mode-0 mode-1 mode-x
> mode-0 : include(+1) / exclude(-1) for pos0  
> mode-1 : include(+1) / exclude(-1) for pos1  
> mode-x : at-least-one-track(1) / both-tracks(2) must be 'accepted'.  
> caution **'accepted' means 'not in the list' for exclude mode.**  

- --hts-a
> parse (col,row) according to HTS alpha format and print (view-id,imager-id) in text-mode output.  

- --hts-b
> parse (col,row) according to HTS beta format and print (shot-id,0) in text-mode output.  

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

- --max-write-views max-views-in-write-queue
> limit # of views in write queue. use this option when you faced memory usage problem.  
