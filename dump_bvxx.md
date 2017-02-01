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
  >     $08-$13 -> ph(microtrack), ax,ay(microtrack-corrected), x,y,z(basetrack-at-base-surface) ... for face-2  
  >     $26-$31 -> pos, col, row, zone, isg, rawid ... for face-2 microtrack  
  > 1 : binary output  ( see [[class description|netscan-data-types-ui]] or m:/prg/netscan/sample/2/binary-io/ )  
  > 2 : pos( with face=0 ) rawid rawid0 rawid1 0 ( to create aux file for this base-track-block )  
  > 3 : same as --format 0 but with one more digits for positions and angles.  

  - --micro 0/1
  > display (1) or hide (0) constituent micro track information  

  - --c correction-map-file-absolute
  - --ph phmin

  - --affine a b c d p q
  > apply affine transformation ( after correction-map-absolute if specified )  
