---
#### Release Note on 2016-03-09
---

```
t2l dump_linklet
  MicroTrack 使用の linklet 生成が異様に遅い件の対処
  --cache のバグ未修正 
    => --cache 0 512 4096 0 2 1 ( bb linlet ) or 
       --cache 128 64 1024 2 2 1 ( bm linklet ) であれば動く。
  
eventdescriptor の Parameter::CorrectionMode 新設
  読出し領域が複数の補正マップに跨っている場合に、
  これまでは、それら補正マップの占有面積加重平均による
  単一の補正マップ適用を適用していた ( CorrectionMode = 0 ) が、
  トラック毎に再近接の補正マップを使用するモード ( CorrectionMode = 1 ) を新設。
```
