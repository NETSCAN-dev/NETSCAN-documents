---
##### known bugs
---
+ dc の runcard で MaxTracks は明示して下さい。 現状で、未指定の場合 MaxTracks = 200 として扱われます。 この値では小さすぎる場合があると思います。

---
##### todo list
---
+ vxx-file 内部の hash entry に含まれる区画情報のバグ修正
+ runcard consistency の check program

---
##### change log ( 未リリース分のみ )
---
+ dc で使っている calc_bg のバグ修正 ( r1066, r1067 )
+ regulate_corrmap で角度補正の affine に回転以外の成分が含まれるバグ修正 ( r1068 )
+ linklet_window の更新 ( r1069 )
+ dc ( DistortionShrinkEvaluator ) で BG 評価できない場合、BG = -1 / significance を 0 に変更 ( r1070 )
+ linklet_window を t2l 変更に合わせて update ( r1071 )
+ dc ( DistorionShrinkEvaluator ) の MaxTracks のデフォルト値を 0 とし、間引きを無効にしておく ( r1072 )
+ dc ( DistorionShrinkEvaluator ) で PHSUMCUT/2 で filter していたのをやめ、 PHCUT にする ( r1073 )
+ dc での ErrAng, ErrPos を自動設定する（ runcard に指定した値は常に上書きされる ） ( r1074 )
+ db_server 読出しエラー後に復旧しないバグの修正 ( r1075 )
+ NetScanDataFilter 読出し区画の定義を変更し、上限値は含まない様にする ( r1076 )
+ 運動量下限値から接続窓を計算するコード mk_mom_window の追加 ( r1077 )
+ dump_linklet の warning message を抑制。表示させるには --warning-level 1 を与える ( r1078 )
+ CorrectionMap::GetView で空の補正マップに対するチェックを追加 ( r1079 )
+ ali.cpp ali-g.cpp ali-l.cpp の typo bug 修正 ( r1080 r1081 )
+ dc ( DistortionShrinkEvaluator ) 内部で使用している histogram の overflow 対策 ( r1082 )
+ dc ( DistortionShrinkEvaluator ) nested ループ解消による高速化 ( r1083 )
+ CorrectionMap::GetView bugfix ( CorrmapDistorition 未指定での MicroTrack 読出しで例外発生 ) ( r1084, r1085 )
+ ali-l ali.cpp 接続可能飛跡対の作成時の ErrShur(LocalAlign::SearchAngle$2), ErrGap(LocalAlign::Gap$2+nominal-dz*Gap$1) 指定追加 ( r1086 )
+ db_server 例外発生時のエラー回復追加 ( r1087 )
+ t2l ( ConnectLinket ) 高速化 ( r1088 r1089 r1090 )
+ dump_fvxx binary output 追加 ( r1091 )
+ ConnectBase.cpp から std::max(a,b,c,d) を排除 ( r1092 )
+ mkmf_p3 高速化 ( r1093 r1094 )
+ t2l 高速化 ( r1095 ... r1123 )
+ dc の出力ファイルにピークの幅情報を付加 ( r1126, r1147 )
+ RunCard constrctor でのエラー対策、ver-2016-09-01 での const 付加を merge ( r1145 )
+ dc の出力ファイルでのピーク幅情報の変更 ( r1148, r1150 )
+ ali 系で --search-mode 未指定ならエラー終了 ( r1149, r1151 )
+ mkmf_p3 の出力ファイルサイズ削減 ( r1152, r1153 )
+ regulate_corrmap 引数解釈のバグ修正 ( r1154, r1155 )
+ ali-g と ali-l で pl-zone 指定対応 ( r1156, r1157 )
+ dc の出力ファイルでのピーク幅情報の再変更 ( r1162, r1163 )
