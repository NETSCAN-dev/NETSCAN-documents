---
#### ベーストラック情報
---

# Member variables
> ここでは [base_track_t](netscan_data_types_ui.md) のメンバ名を使って記述する。  
> 他の出力形式 ( ex. dump_bvxx のテキスト出力 ) の場合は適宜読みかえる事。

## base_track_t
+ ax, ay
+ x, y, z
+ pl
+ isg
+ zone
+ rawid
+ [micro_track_subset_t](#micro_track_subset_t) m[2]

## ax, ay
ベーストラックの角度 [tanθ]

## x, y
1 面乳剤層のベース境界面での座標。  
HTSの場合、ステージ側乳剤層のベース境界面の座標 [micron]

## z
dump_bvxx では z=0 ( --c で補正を適用した場合は z=0 に補正値が加算される )。  
dump_linklet ( t2l --dump-linklet ) ではリンクレット作成時に使われた z の値 [micron] になる。

### pl
プレート番号

### isg
使用しないで下さい。

### zone
[event-descriptor](event-descriptor.md) に同じプレートの vxx-file を複数登録する際にそれらを区別するための変数。  
[event-descriptor](event-descriptor.md) で指定する値であり vxx-file には埋め込んでいないため、これを使用する `t2l --dump-linklet` や `dump_linklet` の出力でのみ意味ある値になが、  
dump_bvxx では引数に指定した値がそのまま設定される。  
使い方は、例えば [note の inter-zone alignment](inter-zone-alignment.md) を参照の事。

### rawid
vxx-file に保存するときの順番 ( 0 始まり ) で割り当てられている ID。  
リンクレット情報にベーストラックの rawid が含まれていると、当該ベーストラックを探し出すことができる。

## micro_track_subset_t
m[0] は face=1 乳剤層の、m[1] は face=2 乳剤層のマイクロトラック情報。  
HTSの場合、m[0] がステージ側、m[1] がレンズ側である。  
micro_track_subset_t の持つメンバは下記の通り。

+ ax, ay
+ z
+ ph
+ pos, col, row, zone, isg
+ rawid

## ax, ay
shrink+distortion 補正後のマイクロトラック角度 [tanθ]

## z
ベース境界面の Z 座標 [micron]

## ph,pos, col, row, zone, isg, rawid
[dump_fvxx_columns](dump_fvxx_columns.md)を参照せよ。
