---
#### ali-g
---

#### description
> プレート間の位置関係を求める。[ali-l](ali-l.md) とは、探索範囲を総当たりで探す点が、異なる。  
> 探索パラメータは、位置のシフトと回転、角度のシフト（位置の回転分補正後）、回転、ギャップである。  

#### usage
> #### ali-g pl0[-zone0] pl1[-zone1] [options]  
> <br>
> default value for zone0/1 are 0  

#### options ( those are in **bold** must be given )
- **--descriptor [event-descriptor](event-descriptor.md)**
> set [event-descriptor](event-descriptor.md)  

- --io fname-io  
> override [event-descriptor](event-descriptor.md) entries  

- **--rc runcard-file**
> set runcard  

- **--id-geom geom**
> set geometry 0/1/...  

- **--c [correction-map-file](correction-map.md) [w/a]**
> 探索結果である [相対補正マップ](correction-map.md) のファイル名を指定する。  
> 上書き(w) か追記(a) を指定可。デフォルトは上書き。  

- **--view view-step view-overlap [view-select-mode]**  
  **--view view-list-file-name [view-select-mode]**
> [処理区画リスト](view-list.md) を、view-step と view-overlap で指定して自動生成するか、  
> [mk_views](mk_views.md) で作成した [処理区画リスト](view-list.md) のファイル名で指定する。  
> ファイル名での指定を推奨。  
> view-select-mode は、処理区画リストの一部だけを処理させたい場合に使う。  
> view-select-mode = 1 は中央の 1 区画だけを、view-select-mode = 2 は中央と四隅の計 5 区画だけを処理する。  

- --search-mode 1/2 \[ [correction-map-i](correction-map.md) \]
> このオプションで探索基点の決定法 (search-mode) と、基点の決定に使う [correction-map-i](correction-map.md) を指定する。  
> search-mode = 1 では、常に [correction-map-i](correction-map.md) 中の最近接区画を基点とする。  
> search-mode = 2 では、探索に成功した区画リスト中の最近接区画を基点とするが、  
> 探索に成功した区画が得られる迄は [correction-map-i](correction-map.md) から基点を選ぶ。  
> [correction-map-i](correction-map.md) が未指定の場合は無変換パラメータを基点とする。  
> search-mode = 0 は search-mode = 1 と同じ動作をする ( 削除予定 )。  

- --offset-xy offset-i offset-x offset-y
> modify GlobalAlign::PositionWindow as  
> when offset_i = 1 &rArr; xmin-offset_x, xmax-offset_x, ymin-offset_y, ymax-offset_y  
> when offset_i = 2 &rArr; xmin+offset_x, xmax+offset_x, ymin+offset_y, ymax+offset_y  

- --[filter-list](filter-list.md) [filter-list](filter-list.md) +1/-1
> include (+1) or exclude (-1) [[[filter-list](filter-list.md)|[filter-list](filter-list.md)]]  

- --filter-max-track-per-view value
> avoid process in views having max-track-per-view track or more

- --filter-area area-list-file
> For each view, use tracks in areas given in area-list-file only.  
> This option will be useful for whole plate (pc) alignment.  
> First 4 columns of each line in area-list-file should be xmin, xmax, ymin and ymax.  

#### FAQ
- **Q** 最初ビームトラックを使って位置と角度ずれを補正、次に角度の大きな飛跡を使ってギャップを補正みたいなことはできる ?  
  **A** 原理的にできるはずだが、実用上難しい
- **Q** ベーストラックとマイクロトラックの角度差が小さい飛跡だけ使うことはできるのか ?  
  **A** --filter-list で探索に使う track の pl,rawid を選択すれば可。  
- **Q** 位置ズレの初期値を与えるにはどうしたらいい ?  
  **A** --search-mode 1 を使い、初期値を correction-map-i で渡す。  
- **Q** ベーストラック間のギャップの値の設計値が既知であれば、AngleErrorは設定する必要はないのでは ?  
  **A** そのとおり。0でよい ???。 
- **Q** Significance の計算方は ?  
  **A** 工事中  
- **Q** ClusterSize って何 ?  
  **A** 廃止予定のパラメータ。使用しない事。  
- **Q** 乾板の伸び縮みは計算できる ?  
  **A** プレート全体の結果を使い [regulate_corrmap](regulate_corrmap.md) で affine パラメータの global 成分 ( プレーｔ全体に共通の成分 ) を抽出すれば、それから求められる。  
>  FitMode=2,3 では 区画毎に shirnk を許してフィットしているので原理的には求めているが、over-fit の可能性もあり、こちらからの抽出は非推奨。  

#### runcard example
```
[GlobalAlign]
AngleCut        = 0 -0.40 0.40 -0.40 0.40   # 後述
PhCut           = 18
VolCut          = 3
MinimumTracks   = 10                # MaPeaks = 1の場合、このバージョンから高速検索のための複数のしきい値は必要ありません。
PositionWindow  = -2500.0 +2500.0 -2500.0 +2500.0   # xmin xmax ymin ymax
                                                    # 位置ズレ探索範囲。パラメータが１個の場合は |dx|< value && |dy| < value に展開する。
RotationWindow  = 0.020 0.002       # 回転探索範囲(±)、ステップ
PositionError   = 3.0               # 接続判定での位置ズレ許容値の決定に使う（必ずしも許容値ではない）。下記「接続判定で許容する範囲」参照。
AngleError      = 0.020             # 接続判定での角度ズレ許容値。位置ズレ判定にも使用。下記「接続判定で許容する範囲」参照
Significance    = 10.0              # minimum significance for peak search. 
ClusterSize     = 30.0              # When MaxPeaks > 1, obtained peaks which has distance < ClusterSize are clustered.  
IsBaseTrack     = 1
MaxPeaks        = 1                 # number of peaks to be searched. use 1 for usual cases. 
Dz              = -10.0 10.0 10.0   # 探索範囲の下限値、上限値、ステップ
ShiftAx         = -0.000 0.000 0.020 # 探索範囲の下限値、上限値、ステップ この設定では探索しない
ShiftAy         = -0.000 0.000 0.020 # 探索範囲の下限値、上限値、ステップ この設定では探索しない
FitMode         = 1                 # 1:offset＋rotation / 2:offset＋shrink / 3:offset＋rotation＋shrink ( default=1 )
HistogramFileTemplate   =           # 後述
Zproj           = 0.5               # 最近接ベース面間距離を Zproj : 1-Zproj に内分する z で alignment 探索を行う。default = 0.5
GhostFilter     = 1.0 0.007         # [b_filter](b_filter.md)の *--ghost* と同じ機能。
InternalMode    = 2                 # InteranlMode=1/2 を推奨。0 は廃止予定。
```
#### 接続判定で許容する範囲
> ali-g での探索アルゴリズムは以下の通り。  
> 探索パラメータの中、角度ズレに関しては ShiftAx,ShiftAy に、ギャップに関しては Dz に、回転に関しては RotationWindow に、それぞれ指定された範囲とステップでループを回す。  
> そしてこのループ毎に、角度差 ErrorAngle 以内の track 対をに対して、位置ズレ histogram (dx,dy) を作成し、ヒストラム中のピークを探索している。  
> 位置ズレ histogram のビン幅は InternalMode により下記にしている。  
> - 0 : PositionError + AngleError &times; std::min(fabs(z0-zproj),fabs(z1-zproj)) (従来動作)  
> - 1 : PositionError  
> - 2 : PositonError += nominal-dz &times; AngleError
> InternalMode のデフォルトは互換性のために 0 としているが、今後の使用では 1 または 2 を推奨。  
>

#### AngleCut
> 第 1 カラムは、0(disabled)/1(include)/-1(exclude)  
> ( -0.1&le;tan&theta;x&le;+0.1 )&&( -0.1&le;tan&theta;y&le;+0.1 ) を使う場合は  
> `AngleCut = 1 -0.10 0.10 -0.10 0.10`
>
> ( -0.1&le;tan&theta;x&le;+0.1 )&&( -0.1&le;tan&theta;x&le;+0.1 ) を排除する、つまり、abs(tan&theta;x)&gt;0.1||abs(tan&theta;y)&gt;0.1 を使う場合は  
> `AngleCut = -1 -0.10 0.10 -0.10 0.10`
>

#### HistogramFileTemplate
> hist-％02d-％02d-％05d-％d.dmpのように指定した場合、ヒストグラムデータをダンプ出力する。  
> フォーマット指定子はファイル名の中の pl0、pl1、view-id、peak-id をファイル名に埋め込むため。  
> このファイルの書式は [下記](ali-histogram.md) を参照の事。  
>
