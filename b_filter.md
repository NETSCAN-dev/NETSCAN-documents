---
#### b_filter
---

+ description : filter base track vxx file  
+ usage : b_filter *pl* *vxx-file* [*options*]
+ options ( those are in **bold** must be given )
  - **--o output-vxx-file**
  > set output vxx-file  

  - --page-size val
  > set vxx page-size ( default is that of input vxx file )  

  - --view-width val
  > set view witdh for fiter-processing  

  - --xy xmin xmax ymin ymax
  > select on xy  

  - --filter-list filter-file-name +1/-1
  > specify [[filter-file-name|filter-list]] to include (+1) or to exclude (-1)  

  - --filter-max-track-per-view val
  > reject view which has a given number of tracks or more  

  - --density val
  > reject views which has a track density > val tracks/cm2 ( to be replaced with --filter-max-track-per-view )  

  - --pl new-pl
  > change pl  

  - --afp a b c d p q
  > apply affine transform on position x-y  

  - --aft a b c d p q
  > apply affine transform on angle ax-ay  

  - --ph nbin wbin val ...
  > apply phmin cut  

  - --vol nbin wbin val ...
  > apply volmin cut  

  - --append
  > append base tracks to output vxx file  

  - --ghost dr dt
  > ghost filter  

  - --edit fname-edit
  > edit limited properties of each base track ( x,y,col,row are not accepted ).  
  > 'fname-edit' format is that of dump_bvxx output with --format 0/3.  
