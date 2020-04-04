# Member variables
## base_track_t
ベーストラック情報

+ ax, ay
+ x, y, z
+ pl
+ isg
+ zone
+ rawid
+ micro_track_subset_t m[2]

### ax, ay
ベーストラックの角度 [tanθ]

### x, y, z
HTSの場合、ステージ側乳剤層のベース境界面の座標 [micron]

### pl
何?

### isg
何?

### zone
詳しく知ってる人しか使えない。同じposで複数回スキャンしたり、複数箇所スキャンしたときに使うらしい。

### rawid
vxx形式で保存するときの順番(0始まり)で割り当てられているID。
リンクレット情報にベーストラックのrawidが含まれていると、lvxx形式から当該ベーストラックを探し出すことができる。

### micro_track_subset_t
HTSの場合、m[0]がステージ側乳剤層、m[1]がレンズ側乳剤層のマイクロトラック情報

## micro_track_subset_t

マイクロトラック情報

+ ax, ay
+ z
+ ph
+ pos, col, row, zone, isg
+ rawid

### ax, ay
Shrink, Distortion補正後のマイクロトラックの角度 [tanθ]

### z
ベース境界面のZ軸座標 [micron]

### ph
PH*10000+PHV

### pos, col, row, zone, isg, rawid
[dump_fvxx_columns](dump_fvxx_columns.md)を参照せよ。
