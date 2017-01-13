---
##### known bugs
---
+ dc の runcard で MaxTracks は明示して下さい。 現状で、未指定の場合 MaxTracks = 200 として扱われます。 この値では小さすぎる場合があると思います。

---
##### todo list
---
+ runcard consistency の check program
+ eventdescriptor 生成用 script  

---
##### change log
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
+ dump_bvxx 出力での micro-track xy 座標計算バグ修正 ( hg 1193,1194 )
+ ここまで ver-2011-03-02 でリリース済
+ t2l に --dump-linklet option 追加 ( hg 1203,1205 )
+ 上記で混入した t2l.cpp のバグ修正 ( hg 1204,1206 )
+ zone で複数領域を区別する機能の強化 ( hg 1207,1209 )
+ db_shell に補正マップ適用コマンド、xy 座標変換コマンド追加 ( hg 1208,1210 )
+ event-descriptor に追加した Parameters::MaxZone の定義を、zone 数から最大 zone 番号に修正 ( 1211:b2d70a8c4943,1213:393703d2c0c9 )
+ f_filter で --append 指定の際に view-header を上書きするバグを修正 ( 1213:393703d2c0c9,1214:33f1045bbc39 )
+ ali-g ali-l t2l で zone 対応化に伴う補正マップ適用のバグを修正 ( 1219:860aa172dc90,1220:f2e1fdea3862 )
+ l_pic, dump_s のバグ修正 ( 1221:7a010a2ba6a0,1222:0bb8286f58a9 )
+ 不要なアプリ削除 ( 1223:a27ad3cb55a8,1224:40e7fdd488de )
+ zone 対応の書き出し時のバグ修正 ( 1225:ce9e5c7a127b,1226:201cada63dfb,1227:711fec3d05f1,1228:cb0367d56135 )
+ NetScanDataFilter::GhostFilter のバグ修正 ( 1229:6317639f4de1,1230:d49332624a04 )
+ t2l の thread 間で mutable_ptr が不正に解放されるバグの対策 ( 1231:d9eb28f11cf0,1232:c9ce3c2055e0 )
+ mk_views の zone 対応 ( 1233:c5f141969df2,1234:8e6fc157cc5c )
+ 不要なアプリ削除 ( 1235:ec4bb8dbf625,1236:2e237985fed4 ) ``` これ以降は ver-2016-09-01 のみ適用 ```
+ ViewDescriptor への m_id 追加と不要 method 削除 ( 1237:6b1a13bb284a )
+ ali-g で view-list の id を corrmap の区画 id にコピーする ( 1238:fd3a38b18c66 )
