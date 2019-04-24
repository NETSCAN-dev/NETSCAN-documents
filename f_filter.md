---
#### f_filter
---

+ description : filter micro track vxx file  
+ usage : f_filter *pos* *vxx-file* [*options*]
+ options ( those are in **bold** must be given )
  - **--o output-vxx-file**
  > set output vxx-file

  - --page-size val
  > set vxx page-size ( default is that of input vxx file )  

  - --view-width val
  > set view witdh for fiter-processing  

  - --xy xmin xmax ymin ymax
  > select on xy  

  - --[filter-list](filter-list) filter-file-name +1/-1
  > specify [[filter-file-name|[filter-list](filter-list)]] to include (+1) or to exclude (-1)  

  - --filter-max-track-per-view val
  > reject view which has a given number of tracks or more  

  - --density val
  > reject views which has a track density > val tracks/cm2 ( to be replaced with --filter-max-track-per-view )  

  - --pos new-pos
  > change pos  

  - --afp a b c d p q
  > apply affine transform on position x-y  

  - --aft a b c d p q
  > apply affine transform on angle ax-ay  

  - --ph nbin wbin val ...
  > apply phmin cut  

  - --vol nbin wbin val ...
  > apply volmin cut  

  - --phmax nbin wbin val ...
  > apply phmax cut  

  - --volmax nbin wbin val ...
  > apply volmax cut  

  - --append
  > append micro tracks to output vxx file  

  - --ghost dr dt
  > ghost filter in position (dr micron) and angle (dt rad).  
  > For each pair of micro-tracks, choose high ph, if position-diff < dr and angle-dif < dt.  
