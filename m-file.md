---
#### m-file format
---

+ example  
```
% Created by l2m.awk
% on 2017/08/18 14:17:13 (JST)
 0 0 1 0 0.0 0.0000
 2
31 21
459905 2 31 21 <--(1)
31 0 4982314 160000 -0.9081 -0.5725  58885.6  30089.6   6574.1 0 0 0 0 0.0 0.0 <--(2)
21 0 4690554 160000 -0.9155 -0.5876  61754.8  31913.9   3430.0 0 0 0 0 0.0 0.0 <--(2)
```


+ First 5 lines can be ignored.  
+ (1) is a track header line ( 4 cols ) ... itk( chain-id ) / # of tracks / pos0 / pos1  
+ (2) is a each track's info ( 15 cols ) ... pos / group-id / rawid / ph / ax / ay / x / y / 0 / 0 / 0 / 0 / 0.0 / 0.0  
