---
#### t2l
---

#### description
> プレート間で micro/base-track をつないで linklet をつくる。  

#### usage
> #### t2l [options]

#### options ( those are in **bold** must be given )
- **--pos pos0[-zone0] pos1[-zone1]**
> set pos ( plate + face ) pair to connect. default for zone = 0.  

- **--descriptor [event-descriptor](event-descriptor.md)**
> set [event-descriptor](event-descriptor.md)  

- --io fname-io
> override [event-descriptor](event-descriptor.md) entries  

- **--rc runcard-file**
> set runcard  

- **--geom geom**
> chamber 構造を番号 ( 0,1,... ) で指定する。  

- **--c correction-map-file [ correction-map-dist ]**  
  **--c - correction-map-dist**
> set correction-map-align ( relative correction-map ) and correction-map-dist ( required to connect micro tracks )  

- **--view view-step view-overlap**  
  **--view view-list-file-name**
> [処理区画リスト](view-list.md) を、view-step と view-overlap で指定して自動生成するか、  
> [mk_views](mk_views.md) で作成した [処理区画リスト](view-list.md) のファイル名で指定する。  
> ファイル名での指定を推奨。  
  
- --append
> add built linklets to vxx, otherwise vxx is overwritten  

- --[filter-list](filter-list.md) [filter-list](filter-list.md) +1/-1
> include (+1) or exclude (-1) [[[filter-list](filter-list.md)|[filter-list](filter-list.md)]]  

- --filter-max-track-per-view value
> avoid process in views having max-track-per-view track or more  

- --window xmin xmax ymin ymax
> limit position of tracks to be connected  

- --offset-xy offset-i offset-x offset-y
> ex. --offset-xy 1 100 0 means &rArr; 1st plate is shifted by (100,0) when connecting tracks, after applying correction-maps.  

- --cache $1 $2 $3 $4 $5 $6
> set cache size ( values set to 0 are remain unchanged ).  
> $1 : high cache pages for micro-vxx  
> $2 : high cache pages for base-vxx  
> $3 : high cache pages for linklet-vxx  
> $4 : high cache blocks for micro-vxx  
> $5 : high cache blocks for base-vxx  
> $6 : high cache blocks for linklet-vxx  

- --dump-linklet output-file-name 0/1/2
> output dump_linklet equivalent format to  output-file-name  
> 0/1/2 is values given in --format 0/1/2 option to dump_linklet  

- --hts-a
> parse (col,row) according to HTS alpha format and print (view-id,imager-id) in text-mode output.  

- --hts-b
> parse (col,row) according to HTS beta format and print (shot-id,0) in text-mode output.  

- --log ( obsoleted )
>  

- --max-write-views max-views-in-write-queue
> limit # of views in write queue. use this option when you faced memory usage problem.  

#### runcard example
```
[Linklet]
Mode        = 1         # connection widow is set by errors (0) / by minimum momentun and sigma (1)
MemoryLimit = 100000    # max number of linklets created in each view
MinimumMomentum = 0.500 2.432
Errors1     = 1.0 1.0 0.007 0.007 0.0 0.0 0.000 0.000
Errors2     = 1.0 1.0 0.007 0.007 0.0 0.0 0.000 0.000
PHCUT1      = 0 1.0 0
PHCUT2      = 0 1.0 0
PHSUMCUT    = 0 1.0 0
CircleCut   = 1
RadialCut   = 0     # errors and windows are in X-Y (0) / Radial-Lateral (1)
MaxAng      = 0.8   # 
Zproj       = f     # connection is done at z = (1-f)*z0 + f*z1 when Mode = 0 ( default is 0.5 )
Offset1     = 100 0 # equivalent to --offset-xy 1 100 0
Offset2     = 100 0 # equivalent to --offset-xy 2 100 0
UseAngleErrorForPositionWindow = 1  # default = 1. when set 0, angle-errors in Errors1/2 are not used to calculate PositionWindow at zproj.
ConnectionWindow-0 = xmin xmax ymin ymax axmin axmax aymin aymax # multiple connection window can be specified using ConnectionWindow-0/1/2 ... 
ConnectionWindow-1 = xmin xmax ymin ymax axmin axmax aymin aymax # 
ConnectionWindow-X = xmin xmax ymin ymax axmin axmax aymin aymax # This is a replacement of WindowMin and WindowMax, and they will be obsoleted.
WindowMin   = wxmin wymin waxmin waymin # asymmetric fixed connection window. default values are 0.  
WindowMax   = wxmax wymax waxmax waymax # this parameter assumes Mode = 0 and CircleCut = 0
```
> To set connection window, do not use ErrPos and ErrAng anymore and use Errors1 and Errors2 instead.  
> Please refer to **[[inside description|t2l-inside]]** below for their usage. 
