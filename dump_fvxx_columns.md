---
#### マイクロトラック情報
---

# Member variables
+ pos
+ zone
+ rawid
+ isg
+ ph
+ ax, ay
+ x, y
+ z
+ z1, z2
+ px, py
+ col, row

## pos
プレート番号 x 10 + 乳剤面番号 ( face = 1 or 2 ) の値。  
HTSの場合、face=2 がレンズ側乳剤層で face=1 がステージ側乳剤層になっている。

## zone
[event-descriptor](event-descriptor.md) に同じプレートの vxx-file を複数登録する際にそれらを区別するための変数。  
[event-descriptor](event-descriptor.md) で指定する値であり vxx-file には埋め込んでいないため、これを使用する `t2l --dump-linklet` や `dump_linklet` の出力でのみ意味ある値になるが、  
dump_fvxx では引数に指定した値がそのまま設定される。  
使い方は、例えば [note の inter-zone alignment](inter-zone-alignment.md) を参照の事。

## rawid
vxx形式で保存するときの順番(0始まり)で割り当てられているID。  
ベーストラック情報にマイクロトラックの rawid が含まれていると、当該マイクロトラックを探し出すことができる。

## isg
HTSの場合、ある視野、あるセンサでユニークに割り当てられるshotIDの飛跡の中で、HTSから出力されたときに保存されるときの順番 ( 0 始まり ) で割り当てられている ID。  
fvxx の col, row, isgがあれば、HTSのベータファイル形式からマイクロトラックを探し出すことができる。

## ph
ここでは便宜上16枚の画像のうち飛跡の上にグレインがあった画像の数を `PH(Pulse Height)` 、  
`PH` が最大となる飛跡を中心として、同じ角度で似た位置に存在する全ての飛跡のピクセル座標での位置と `PH` 空間の体積が `VOL` である。  
`VOL` の詳細な定義は飛跡認識を行うプログラムの仕様に依存する。

``` cpp
PH  = ph / 10000;
VOL = ph % 10000; 
```

## ax, ay
飛跡の始点から終点への変位ベクトル `dx` `dy` `dz` を用いて次のように表される ( tanθ である )。  
fvxx での角度 ax, ay には [dc](dc.md) で求めた補正は適用されていない。

``` cpp
ax = dx / dz;
ay = dy / dz;
```

## x, y
HTS では、飛跡認識に用いる 16 枚の画像のうち、レンズ側 ( Z 軸正方向 ) の一番端の画像を 1 枚目としたとき 8 枚目の位置での xy 座標である。

## z
[event-descriptor](event-descriptor.md) 経由でチェンバー構造を記述する parts.kar, chamber.kar を使う場合は、そのチェンバー構造を反映した z の値。  
チェンバー構造を考慮しない [dump_fvxx](dump_fvxx.md) では常に z=0 となる。単位はミクロン。

+ db_shell で micro-track を読み出す時には chamber 構造を反映した z になり、その値は常に nominal なものになる。 
+ t2l で micro-track と base-track の linklet を作成する場合に face=2 の micro-track の場合に z が nominal な値になる。

## z1, z2
飛跡認識に使った乳剤層の両端の z 座標[micron] であり、基本的に相対値のみ意味があるが、  
現状では [dc](dc.md), [m2b](m2b.md) において、両面乳剤層の同時測定を前提として、両面乳剤層のベース面の差をベース厚として使っている。  
HTS では、飛跡認識に用いる 16 枚の画像のうち、  
ステージ側 ( Z 軸負方向 ) の一番端の画像を撮像したときのZ座標が `z1`、レンズ側 ( Z 軸正方向 ) の一番端の画像を撮像したときのZ座標が `z2` である。

## col, row
HTS info embedded in `col` and in `row` at Alpha files
  ``` c
  ImagerID = CameraID * 12 + SensorID;
  col = (int16_t)("ViewID"&0x0000ffff);
  row = (int16_t)((("ImagerID")&0x000000ff)|(("ViewID"&0x00ff0000)>>8));
  ```
HTS info embedded in `col` and in `row` at Beta files (仕様変更の可能性あり)
  ``` c
  uint32_t ImagerID = CameraID * 12 + SensorID;
  uint32_t ShotID = ViewID * NumberOfImager + ImagerID;
  col = (int16_t)("ShotID"&0x0000ffff);
  row = (int16_t)(("ShotID"&0xffff0000)>>16);

  //もとに戻す時は
  uint32_t ShotID = ((uint32_t)(uint16_t)row << 16) | ((uint32_t)(uint16_t)col);
  uint32_t NumberOfImager = 72;
  uint32_t ViewID = ShotID / NumberOfImager;
  uint32_t ImagerID = ShotID % NumberOfImager;
  ```
