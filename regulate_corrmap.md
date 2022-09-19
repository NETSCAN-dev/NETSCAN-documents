---
#### regulate_corrmap
---

#### description : 相対補正マップファイルからグローバル(pc)成分を抽出する。  
  > global-correction-map is evaluated by fitting position displacement at a center of each area  
  > using affine transformation ( shift + rotation + shrink ).  
  > global-correction-map should be applied first and then local-correction-map.  
  > global angle correction is evaluated from r1062 onwards.  

#### usage : regulate_corrmap correction-map-relative(in) correction-map-relative-diff(out) correction-map-relative-global(out) [options]
  > file-names of correction-map in arguments can be '-' and then it means that those files are not written.  

#### options
- --v
> correction-map-relative-diff の各区画中心での位置ズレを表示する。  

- --fit-mode 0/1/2/3
> correction-map-relateive-global を求める際のフィット法指定。デフォルトは 3。  
> 0:平行移動 / 1:平行移動＋回転 / 2:平行移動＋縮尺 / 3:平行移動＋回転＋縮尺 でフィットする。  

