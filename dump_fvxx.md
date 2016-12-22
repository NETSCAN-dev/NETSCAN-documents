---
#### dump_fvxx - dump micro track vxx file -
---

+ usage : dump_fvxx vxx-file pos zone [ options ]
+ options
  - --index i0 i1
  > limit dump for i0 &le; rawid < i1  

  - --a xmin xmax ymin ymax  
    --a list-file
  > select on xy  
  > list-file contains ```xmin xmax ymin ymax axmin axmax aymin aymax``` in each line and can have multile lines  

  - --format 0/1/2/3/4
  > 0 : rawid isg ph ax ay z1 z2 ( NF=7, so-called f-file format with 4 header lines )  
  > 1 : pos zone rawid isg ph ax ay x y z z1 z2 px py col row f ( NF=17, f is obsoleted )  
  > 2 : pos rawid 0 ( NF=3, to create aux-file for this micro-track-block )  
  > 3 : same as --format 2 but with one more digits for positions and angles.  
  > 4 : binary output ( see [[class description|netscan-data-types-ui]] or m:/prg/netscan/sample/2/binary-io/ ).  
  > (*) xy position of micro-track is at z-center of an emulsion layer  
  >    
  - --c correction-map-file-absolute
  - --ph phmin
  - --affine a b c d p q
  > apply affine transformation ( after correction-map-absolute if specified )  

+ HTS info embedded in 'col' and in 'row' at Alpha files
  ``` c
  ImagerID = CameraID * 12 + SensorID;
  col = (int16_t)("ViewID"&0x0000ffff);
  row = (int16_t)((("ImagerID")&0x000000ff)|(("ViewID"&0x00ff0000)>>8));
  ```
+ HTS info embedded in 'col' and in 'row' at Beta files (仕様変更の可能性あり)
  ``` c
  ImagerID = CameraID * 12 + SensorID;
  ShotID = ViewID * ImagerID + ImagerID;
  col = (int16_t)("ShotID"&0x0000ffff);
  row = (int16_t)(("ShotID"&0xffff0000)>>8);
  ```
