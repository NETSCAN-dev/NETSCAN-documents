---
#### l_pic - make plots of linklets -
---

+ usage : l_pic [ options ] < linklet-dump-file
  > This program assumes output file created by dump_linklet is to be given from stdin.  
+ options ( those are in **bold** must be given )
  - **--descriptor event-descriptor**
  > set event descriptor  

  - --io fname-io
  > override event descriptor entries  

  - --geom 0/1/...
  > set geometry 0/1/... ( default = 0 )  

  - --pos pos1 pos2
  > set pos pair to make plots  
  
  - --rc runcard-file
  > set runcard used by t2l. 
  > this is used to know max values of errpos and errang to determine plot are size.  
  > --err option can set these error values directly instead  
  
  - --err errpos errang
  > set plot area size ( &plusmn; errpos &times; 2 and &plusmn; errang &times; 2 ) directly.  
  
  - --c-view fname-correction-map
  > set correction-map file used by t2l. this is used to know a view size to determine plot area size.  
  
  - --o prefix
  > set output file prefix. prefix.root and prefix.ps are created ( default is l_pic.root ).  
  
  - --num num_val
  > use first num_val linklets for plot.  
  
  - --mabiki mabiki_val
  > reduce \# of linklets by using every mabiki_val linklets only.  
  
  - --cache size_m size_b page_m page_b
  > to be obsoleted  
