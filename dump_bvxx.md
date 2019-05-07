---
#### dump_bvxx
---

+ description : dump base track vxx file
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

  - --affine a b c d p q
  > apply affine transformation ( after correction-map-absolute if specified )  

#### FAQ
* Q. VXXの座標(base_track_tクラス内におけるx y)のZ面はどこ? <br>
  A. FACE1のマイクロトラックの中央の座標。なおbase_track_tクラス内におけるzもやはりFACE1のzと同じ
* Q. ベーストラックの中央の座標(cx, cy)はどうやって求めれば良い?<br>
  A. `cx = x + ax * (m[1].z-m[0].z)*0.5`
* Q. マイクロトラックの角度は、correction-mapによる補正後? 補正前?<br>
  A. distortionとshrink両方補正した値。
