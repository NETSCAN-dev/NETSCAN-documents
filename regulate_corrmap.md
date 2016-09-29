---
#### regulate_corrmap - extract global affine transformation in corrmap -
---

+ description : evalute global component of correction-map
  > global-corrmap is evaluated by fitting position displacement at a center of each area  
  > using affine transformation ( shift + rotation + shrink ).  
  > global-corrmap should be applied first and then local-corrmap.  
  > global angle correction is treated from r1062 onwards.  

+ usage : regulate_corrmap corrmap(in) local-corrmap(out) global-corrmap(out)
