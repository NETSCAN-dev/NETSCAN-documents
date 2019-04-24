---
#### regulate_correction-map
---

+ description : evalute global component of correction-map
  > global-correction-map is evaluated by fitting position displacement at a center of each area  
  > using affine transformation ( shift + rotation + shrink ).  
  > global-correction-map should be applied first and then local-correction-map.  
  > global angle correction is treated from r1062 onwards.  

+ usage : regulate_correction-map correction-map(in) local-correction-map(out) global-correction-map(out)
  > correction-map-file-names ( i.e. arguments ) can be '-' and it means that those files are not written.  
