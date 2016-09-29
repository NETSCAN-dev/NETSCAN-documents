---
#### 高速化を図るために 全部（できる限り）の vxx をメモリーに保持するための --cache option の設定法
---

```
t2l, ali, ali-g, ali-l　の場合
  (1) base-track <-> base-track の場合
    --cache 0 2048 0 0 2 1
      第2パラメータ > base-track-vxx-file-size-in-Mbyte x 2
      ex. 上記例は、各 base-track-vxx が 1Gbyte の場合
  (2) base-track <-> micro-track の場合
    --cache 1024 1024 1 1 1
      第1パラメータ > micro-track-vxx-file-size-in-Mbyte / 4
      第2パラメータ > base-track-vxx-file-size-in-Mbyte
      ex. 上記例は micro-track-vxx が 4Gbyte, base-track-vxx が 1Gbyte の場合
  (*) linklet-vxx は書出しのみなのでキャッシュ不要

dump_linklet の場合
  (1) base-track <-> base-track の場合
    --cache 0 2048 1024 0 2 1
      第2パラメータ > base-track-vxx-file-size-in-Mbyte x 2
      第3パラメータ > linklet-vxx-file-size-in-Mbyte
      ex. 上記例は、各 base-track-vxx が 1Gbyte, linklet-vxx が 1Gbyte の場合
  (2) base-track <-> micro-track の場合
    --cache 1024 1024 1024 1 1
      第1パラメータ > micro-track-vxx-file-size-in-Mbyte / 4
      第2パラメータ > base-track-vxx-file-size-in-Mbyte
      第3パラメータ > linklet-vxx-file-size-in-Mbyte
      ex. 上記例は micro-track-vxx が 4Gbyte, base-track-vxx が 1Gbyte, 
          linklet-vxx が 1Gbyte の場合

--cache 設定妥当性の判断法
  プログラム実行時の画面出力末尾に下記が複数行出力されます。
  
  BlockManager status : 
    BlockType = 3, RecordSize = 109, Size = 7629312, PageListSize = 795, 
    CacheSize = 1024, LockedCacheSize = 0, PageRead = 794
  
  BlockType = 2 ( MicroTrackBlock ) / 3 ( BaseTrackBlock) / 6 ( LinkletBlock ) で、
  CacheSize >= PageRead ( 読み込んだ延べページ数 ) であれば再読み込みなしです。
```
