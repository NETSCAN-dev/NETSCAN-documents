---
#### dump_bvxx
---

+ description : dump base track vxx file
> 出力する情報の詳細は [こちらを参照](dump_bvxx_columns.md) の事。

+ usage : dump_bvxx *vxx-file* *pl* *zone* [*options*]
+ options
  - --index i0 i1
  > limit dump for i0 &le; rawid &le; i1  

  - --a xmin xmax ymin ymax  
    --a list-file
  > select on xy  
  > list-file contains xmin,xmax,ymin,ymax,axmin,axmax,aymin,aymax each line and can have multile lines  

  - --format 0/1/2/3
  > 0 : text output  
  >     $01-$07 -> rawid, pl, isg, ax, ay, x, y  
  >     $08-$13 -> ph(microtrack), ax,ay(microtrack-corrected), x,y,z(basetrack-at-base-surface) ... for face-1  
  >     $14-$19 -> pos, col, row, zone, isg, rawid ... for face-1 microtrack   
  >     $20-$25 -> ph(microtrack), ax,ay(microtrack-corrected), x,y,z(basetrack-at-base-surface) ... for face-2  
  >     $26-$31 -> pos, col, row, zone, isg, rawid ... for face-2 microtrack  
  > 1 : binary output  ( see [[class description|[netscan-data-types-ui](netscan-data-types-ui.md)] or m:/prg/netscan/sample/2/binary-io/ )  
  > 2 : pos( with face=0 ) rawid rawid0 rawid1 0 ( to create aux file for this base-track-block )  
  > 3 : same as --format 0 but with one more digits for positions and angles.  
  > 一般的に飛跡情報をバイナリ形式でdumpする時に使われているのは1なので他のは無視しても良い。

  - --micro 0/1
  > display (1) or hide (0) constituent micro track information  

  - --c correction-map-file-absolute
  - --ph phmin
  - --z z-corrdinate ( 未指定の場合は z=0 になる )

#### FAQ
ベーストラックに関するFAQは[m2bのFAQ](m2b.md#FAQ)を見よ。
