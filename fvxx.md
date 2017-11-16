---
#### fvxx
---

A description of dump_fvxx to output fvxx members is in [dump_fvxx](dump_fvxx.md).

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

## ph
ここでは便宜上16枚の画像のうち飛跡の上にグレインがあった画像の数を `PH(Pulse Height)`  、
`PH` が最大となる飛跡を中心として、同じ角度で似た位置に存在する全ての飛跡のピクセル座標での位置と `PH` 空間の体積が `VOL` である。
`VOL` の詳細な定義は飛跡認識を行うプログラムの仕様に依存する。

``` cpp
PH  = ph / 10000;
VOL = ph % 10000; 
```

## ax and ay
角度 ax, ayは飛跡の始点から終点への変位ベクトル `dx` `dy` `dz` を用いて次のように表される。tanθである。
fvxxの角度 ax ay は[dc](dc.md)を行う前(補正前)の値である。

``` cpp
ax = dx / dz;
ay = dy / dz;
```

## x and y
position of micro-track is at z-center of an emulsion layer. The unit is micron.

HTSから出力するとき、飛跡認識に用いる16枚の画像のうちレンズ側(Z軸正方向)の一番端の画像を1枚目としたとき8枚目の位置でのxy座標でである。

## z
chamber構造 parts.kar を使ったときの、chamber構造を反映したzの値。chamber構造を考慮しない[dump_fvxx](dump_fvxx.md)では常にz=0となる。単位はミクロン。

+ db_shell で micro-track を読み出す時には chamber 構造を反映した z になり、その値は常に nominal なものになる。 
+ t2l で micro-track と base-track の linklet を作成する場合に face=2 の micro-track の場合に z が nominal な値になる。

## z1 and z2
[dc](dc.md)や[m2b](m2b.md)でディストーション・シュリンクコレクションやベーストラックを作成をするときに用いる、同じフィルムの中の相対的なz座標。単位はミクロン。

HTSから出力するとき、飛跡認識に用いる16枚の画像のうち、ステージ側(Z軸負方向)の一番端の画像を撮像したときのZ座標が `z1` 、レンズ側(Z軸正方向)の一番端の画像を撮像したときのZ座標が `z2` である。

## col and row
HTS info embedded in `col` and in `row` at Alpha files
  ``` c
  ImagerID = CameraID * 12 + SensorID;
  col = (int16_t)("ViewID"&0x0000ffff);
  row = (int16_t)((("ImagerID")&0x000000ff)|(("ViewID"&0x00ff0000)>>8));
  ```
HTS info embedded in `col` and in `row` at Beta files (仕様変更の可能性あり)
  ``` c
  ImagerID = CameraID * 12 + SensorID;
  ShotID = ViewID * NumberOfImager + ImagerID;
  col = (int16_t)("ShotID"&0x0000ffff);
  row = (int16_t)(("ShotID"&0xffff0000)>>8);
  ```
