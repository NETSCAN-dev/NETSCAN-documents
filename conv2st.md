---
#### conv2st
---

+ description : parse parts.kar and chamber.kar to dump chamber structure in st.dat format  
+ usage : conv2st [ options ] > st.dat
> if eventdescriptor is not specified, use parts.kar and chamber.kar in current folder  
+ options
  - --desriptor eventdescriptor
  > set [event-descriptor](event-descriptor)  

  - --geom 0/1/2/...
  > set geometry in eventdescriptor ( default = 0 )  
  
  - --parts parts.kar --chamber chamber.kar
  > set parts.kar and chamber.kar directory ( see [[geometry|geometry-descriptor]] )  
  
  - --format 0/1
  > set output format ( 0:st-file / 1:detailed-dump )  
  
  - --c correction-map-absolute.lst
  > apply correction in z by correction-map-absolute.lst  
  
  - --dir current-folder
  > set current folder to parse correctly a given eventdesriptor written using relative path ( to be obsoleted )  
