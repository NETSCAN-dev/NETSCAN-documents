---
#### regulate_corrmap
---

+ description : 相対補正マップファイルからグローバル(pc)成分を抽出する。  
  > global-correction-map is evaluated by fitting position displacement at a center of each area  
  > using affine transformation ( shift + rotation + shrink ).  
  > global-correction-map should be applied first and then local-correction-map.  
  > global angle correction is evaluated from r1062 onwards.  

+ usage : regulate_corrmap correction-map-relative(in) correction-map-relative-diff(out) correction-map-relative-global(out)
  > file-names of correction-map in arguments can be '-' and then it means that those files are not written.  
