---
#### integ_corr
---

+ description : integrate relative correction map to absolute 
  > Integrate relative correction map and output absolute correction map.  
  > This code works for relative correction map with single area for each plate.  

+ usage : integ_corr corrmap-rel(in) corrmap-abs(out) [options]
+ options ( those are in **bold** must be given )
  - **--descriptor event-descriptor**
  > set event descriptor  

  - --geom geom
  > set geometry 0/1/...  

  - --global 0/1
  > enable(1) or disable(0/defaule) global correction to fix end-plates  

  - --split 0/1
  > 1: corrmap-abs with plate(0) or pos(1) entries  

  - --fix pos
  > fix specified pos  
