---
#### 隣接するスキャンエリアどうしのアライメント  (2017/11/1 大島)
---

```
ex.) PL23(Area3)を基準としてPL23(Area1)のアライメントを取る場合


(1) EventDescriptor.iniへの記述
 [BaseTrackBlock]
  23 0 = T:\T60\run6\ECC2\Area3\PL23\b023.sel.vxx 1000000
  23 1 = T:\T60\run6\ECC2\Area1\PL23\b023.sel.vxx 1000000
 のように、2列目のzoneの値を変えてvxxを登録しておく。

(2) view list作成(処理区画1cmx1cm)
 m:\prg\netscan\ver-2016-09-01\obj\win-msvc-x64-12\bin\mk_views.exe 230 230 --descriptor ..\EventDescriptor.ini --view 10000 0 --o view.lst --rc ..\..\rc\align.rc --sort 0

(3) シフト量の計算
 Area1のスキャン原点(-4mm, -4mm), Area3のスキャン原点(-4mm, 76mm)
 x shift = -4mm - (-4mm) =   0mm =      0um
 y shift = -4mm - 76mm   = -80mm = -80000um

(4) global alignment
 m:\prg\netscan\ver-2016-09-01\obj\win-msvc-x64-12\bin\ali-g.exe 23-0 23-1 --descriptor ..\EventDescriptor.ini --id-geom 0 --view view.lst --rc ..\..\rc\align.rc --search-mode 0 --c .\align\correction-map-g-23-23-1.lst --offset-xy 1 0 -80000
	(23-0はzone=0のPL23, 23-1はzone=1のPL23という意味)
	(--offset-xy offset-i offset-x offset-y　でスキャン原点のシフト量を入力する。)

注意点
 * --offset-xy が有効なのは、ver-2016-09-01 のみ。
 * 作成した correction-map の中には zone は埋め込めないので、ファイル名などでユーザーが管理する。

参考資料
 NETSCAN manual の [mk_views](mk_views),ali-g のページ http://heplab3.physics.aichi-edu.ac.jp/kodama/netscan/docs.md/index.html
```

