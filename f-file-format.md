---
#### f-file format
---

> This file foramt is originally used in emulsion data handling in CHORUS experiment.  
> Columns quoted as 'void' are not used to convert track information into vxx file.  

+ header part ( first 4 lines )
  ```
  line-#1 ( $7 is used. others are void )
    $1-$4 run,event,stack,area
    $5 plate
    $6 module
    $7 pos ( plate x 10 + face )
    $8-$9 pver,stage
  
  line-#2 ( all columns are void )
    $1-$4 inter-plate-gap,base,emulsion,shrink
  
  line-#3 ( all colums are void )
    $1-$2 xc,yc
    $3-$4 dx,dy
    $5-$6 axc,ayc
    $7-$8 dax,day
  
  line-#4 ( $4,$5,$8 are used and others are void )
    $1-$2 pver%100,run-event
    $3 scan-mode
    $4-$5 xc,yc
    $6-$7 dx,dy
    $8 # of tracks ( -1 means upto EOF )
    $9 distance to closest fd-mark
    $10-$12 fd-1,fd-2,fd-3
  
  ```
+ track part ( from line-#5 )
  > $1 isg  
  > $2 jtk ( void )  
  > $3 ph  
  > $4-$5 ax,ay  
  > $6-$7 x,y  
  > $8-$9 z1,z2 ( see 'face definition' in [[geometry descriptor files|geometry-descriptor]] )  

+ example
  ```
  0 0 0 0 19 0 190 0 0
  0 0 0 0
  0 0 0 0 0 0 0 0
  0 0 0 0 0 0 0 -1 0 0 0 0
  1 0 140047 0.07510 0.11300 -468802.20 -308284.90 -609.00 -52.70
  2 0 110001 0.32020 0.09870 -468165.70 -308284.70 27.50 -52.50
  3 0 110003 0.24980 0.07600 -468162.90 -308283.70 30.30 -51.50
  ```
