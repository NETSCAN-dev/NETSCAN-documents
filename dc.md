---
#### dc
---

#### description
> #### 両面乳剤層の micro-track を使い、乳剤層の distortion と shrink を求める。
> <br>
> 各乳剤層の厚みとベース厚の nominal 値 ( [parts.kar](geometry-descriptor.md) 記載 ) との差も求めている。  
>  
> dc 内部での使用スレッド数のデフォルトは、処理区画のループでは `std::min<int>(ceil(0.61*GetProcessorInfo().nCore),6);` と、  
> 内部アルゴリズム ( DistorionShrinkEvaluator2 ) では `std:ceil(0.65*GetProcessorInfo().nCore)` としている。  
> --num-threads オプションで変更可。  
>

#### usage
> dc pl[-zone] [options]

#### options ( those are in **bold** must be given )
- **--descriptor [event-descriptor](event-descriptor.md)**
> set [event-descriptor](event-descriptor.md)  

- --io fname-io
> override [event-descriptor](event-descriptor.md) entries  

- **--rc fname-[runcard](#runcard)**
> set [runcard](#runcard)  

- **--o [correction-map](correction-map.md)-file [w/a]**
> 出力 [correction-map](correction-map.md) 名を指定する。w は上書きで a は追記指定。デフォルトは上書き。  
> ~~ここで指定するファイル名は、４文字以上かつ拡張子を .lst とする事を推奨。~~  
> ~~ファイル名が４文字未満 or 拡張子が .lst でない場合に .lst を付加するという、旧版互換のための仕様を回避するため。~~  
> この仕様は ver-2024-09-01 より削除。  

- **--view view-step view-overlap**  
  **--view view-list-file-name**
> [処理区画](view-list.md) を、view-step と view-overlap で指定するか、  
> [mk_views](mk_views.md) で作成した [区画リスト](view-list.md) のファイル名で指定する。  
> ファイル名での指定を推奨。  

- --c [correction-map](correction-map.md)-file
> set [correction-map](correction-map.md) file used as a starting point for each view  

- --search-mode 0/1/2
> set search mode ( default = 0 )  
> 0 : independent search. use first values of ErrDist and ErrShur  
> 1 : begin search in each view using [correction-map](correction-map.md) given in --c option  
> 2 : begin search in each view using closest view's result. use second values of ErrDist and ErrShur if they exists  

- --filter-angle +1/-1 axmin axmax aymin aymax
> use angle regions (+1) or exclude angle regions(-1) in parameter search  
> multiple entries are accepted and includes(+1) are first evaluated and then excludes(-1)  

- --[filter-list](filter-list.md) [filter-list](filter-list.md) +1/-1
> include (+1) or exclude (-1) [filter-list](filter-list.md)  

- --filter-phmin phmin
> phmin filter  

- --filter-max-track-per-view value
> avoid process in views having max-track-per-view track or more  

- --ang-shr ang-shr-base ang-shr-shift  
> abs(&theta;) &le; ang-shr-base &rArr; &theta; += &theta; &times; (ang-shr-shift/ang-shr-base)  
> &theta; > +ang-shr-base &rArr; &theta; += ang-shr-base  
> &theta; < -ang-shr-base &rArr; &theta; -= ang-shr-base  
> (*) implemented by Komatani probably fot HTS specific needs.  

- --num-threads num1 num2
> num1 : # of threads used to process views  
> num2 : # of threads used in scan parameter  

- --debug 1 
> set verbose console output  

#### FAQ
- **Q** 探索に使用する区画ごとの片面トラック数の上限は MaxTracks で指定できるが、実際に使われたトラック数はどうやって得られるのか ?  
  **A** 標準出力はされているが、ファイル出力されていないため現状得る方法はない。  
- **Q** 角度の小さい飛跡を除外して計算させたいが、どうすればよいか ?  
  **A** PHCUT を使えば space-angle での cut は可能。詳細は[cut](cut.md)を参照せよ。  
- **Q** FACE1, FACE2 の dx, dy, dz, dax, day は何か ?  
  **A** micro-track に与える位置・角度の offset だが、dc では全て 0 とする事を推奨。m2b ではこれを使って background の評価を行う事も可。  
- **Q** ある領域だけ dc を通したい場合はどうすればよいか ?  
  **A** [処理区画リストファイル](view-list.md) を使用し、--view に処理対象としたい領域内の区画だけを含むファイルを渡す。  
- **Q** `view-overlap` とは何か ?  
  **A** [mk_views](mk_views.md#description) 参照。  
- **Q** micro-tracl vxx のファイル名を直接指定できないか ?  
  **A** 現状ではできない。引数指定は要検討とする。  
- **Q** significance が低いなどで失敗した領域の場所を得るにはどうすればよいか ?  
  **A** [mk_views](mk_views.md) で作成した [処理区画リスト](view-list.md) を用いれば可能になる予定。  
  &emsp; 処理区画リストの各エントリ id (第1カラム) と出力 [correction-map](correction-map.md) の id (第1カラム) を同じ値にしておくべき所を実装していないだけなので。  
- **Q** dz はどうやって求めている ?  
  **A** [parts.kar](geometry-descriptor.md#parts.kar) で設定した値との差分である。m2bでは使っていない。  

#### runcard example
```
[DC]
#
# FACE1,FACE2 で探索開始位置 dx dy dz dax day shrink_inv dz_fulcrum を指定する。
# dx dy dz dz_fulcrum は dc 内では探索しない。
# 座標系はプレート座標系。
# dz_fulcrum はベース表面（不動面）の補正値であり、現状のアルゴリズム ( ConnectBase ) では下記の通り補正する。
#   FACE1 MicroTrack::z2 -> MicroTrack::z2+FACE1::dz_fulcrum
#   FACE2 MicroTrack::z1 -> MicroTrack::z1+FACE2::dz_fulcrum
# 現状では opera 標準に合わせたプレート座標系であり、対物レンズ側が FACE2、Z 軸は上向き正である。
#
FACE1           = 0.0 0.0 0.0 0.000 0.000 1.00 0.0  # dx dy dz dax day shrink_inv dz_fulcrum
FACE2           = 0.0 0.0 0.0 0.000 0.000 1.00 0.0  #
ErrPos          = 10.0          # ベース中央での micro-track の位置ズレ ( r1074 以降は無用 )
ErrAng          = 0.040         # ベース中央での micro-track の角度ズレ ( r1074 以降は無用 )
ErrDist         = 0.100         # Distortion の探索範囲
ErrShur         = 0.200         # Shrink の探索範囲
ErrorAngleX1    = 0.040 0.000   # error-constant error-slope <= ErrAng の後継
ErrorAngleY1    = 0.040 0.000   #
ErrorAngleX2    = 0.040 0.000   #
ErrorAngleY2    = 0.040 0.000   #
PHCUT           = 0 0.1 0       #
PHSUMCUT        = 1 0.1 21      #
VolCut          = 1 0.1 3       #
AngleStep       = 0.050 0.010 0.002 # distortion 探索 step ( 絞込み探索対応 2015-10-04 より ) 
ShrinkStep      = 0.020 0.010 0.002 # shrink 探索 step
                                    # For second and subsequent AngleStep(ShrinkStep),  
                                    # a search area is defined as 
                                    # +/- previous value of AngleStep(ShrinkStep).  
Significance    = 3.0           # significance がこの値以上の区画を探索成功と判断。失敗区画は出力しない。
MaxAng          = 0.8           # 探索に使用するトラックの最大角度
DumpDebugInfoFileName = xxx.xxx # Enable debug-info output to a file "xxx.xxx",  
                                # when non-null value for this entry is specified.  
                                # Format of this file is ...  
                                # $1,$2  : base-track ax,ay  
                                # $3,$4  : face-1 micro-track ax,ay corrected  
                                # $5,$6  : face-2 micro-track ax,ay corrected  
                                # $7,$8  : face-1 micro-track ax,ay raw  
                                # $9,$10 : face-2 micro-track ax,ay raw  
PatchMe         = 1             # 0 = 従来動作(デフォルト) / 1 = 新規(こちらに統合したい。詳細は notes の algorithm change in dc,ali-i を参照の事。)
MaxTracks       = 1000          # 探索に使用する face-1 トラック数の上限。これを超える場合、floor(face-1トラック数/MaxTracks) の剰余で間引きを行う
```
