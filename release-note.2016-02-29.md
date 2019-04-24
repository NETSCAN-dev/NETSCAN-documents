---
#### Release Note on 2016-02-28
---

```
dc
  thread 間干渉のバグ修正 => これまでのコードでの処理で、このバグ起因の処理失敗があり得ます。
  --o correction-map-o 
    => --o correction-map-o [w/a] ( a = append / w or none = over-write ) 
    => 出力補正マップへの追記は引数で指定する。
	
ali
  --dif correction-map-dif を廃止
  --c correction-map-o => --c correction-map-o [w/a] ( a = append / w or none = over-write ) 
    => 出力補正マップへの追記は引数で指定する。
  --view-list view-list-file 追加 ( view-select-mode は未実装 ) 
	
ali-g
  --pc corrma-pc を廃止
  --dif correction-map-dif を廃止
  --search-mode 1/2 correction-map-i => search-mode = 2 でも correction-map-i 指定可能、上記 --pc の代替
  --view-list view-list-file [ 0/1/2 ] 追加 
    ( view-select-mode 0 = all / 1 = one view in center / 5 views in center and corners ) 

ali-l
  --pc corrma-pc を廃止
  --dif correction-map-dif を廃止
  --search-mode 1/2 correction-map-i => search-mode = 2 でも correction-map-i 指定可能、上記 --pc の代替
  --view-list view-list-file 追加 ( view-select-mode は未実装 ) 

internal
  vxx 書込み時の整数化 ( position => 0.01micron / angle => 0.01mrad ) を切捨てから四捨五入に変更
```
