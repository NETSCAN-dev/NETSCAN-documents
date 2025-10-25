<style>
table { border-collapse: collapse; }
th,td { border: 1px solid #fff; padding: 5px; }
</style>

---
#### vxx ファイルに関するメモ
+ ver-2024-09-01 で、複数版 vxx の混在をやり易くすべく、コード整理を行った。vxx の版は拡張子で判別する仕様。  
+ 従来の vxx では、位置は 1/100 micron 単位の、角度は 1/100 mrad 単位の、ともに整数値として保持している。整数化は四捨五入。  
---
#### 以下は ver-2024-09-01 からの実装であり、ver-2016-09-01 には無い。
---
#### 実装している vxx の種類

##### MicroTrack
>
>  | 拡張子 | 説明 | isg |  
>  |----|----|----|  
>  | vxx-2 | 従来の vxx (1) | hts_beta_fvxx で設定した値を保持 |  
>  | vxx-3 | 従来の vxx (1) に加えて内部ハッシュ単位での zstd 圧縮 | hts_beta_fvxx で設定した値を保持 |  
>  | vxx-4 | HTS beta-file を直接読込む (2) col,row,px,py は未設定 | ShotID 埋込 (3) |  
>
> ```
> (1) 位置は 0.01micron 単位で、角度は 0.01mrad 単位で整数化し保存している。
> (2) f###.vxx-4 は、下記の様な json ファイルで、beta_raw.dat と各面のハッシュパスを記述する。
>     event-descriptor の [MicroTrack] には pos=1,2 とも同じ f###.vxx-4 を指定すれば良い。
> {
>         "beta-file-path": "./hts/beta_raw.dat",
>         "eachshotparam-path": "./hts/Beta_EachShotParam.json", <- 未指定でデフォルト値を使う事も可
>         "eachviewparam-path": "./hts/Beta_EachViewParam.json", <- 未指定でデフォルト値を使う事も可
>         "eachimagerparam-path": "./hts/Beta_EachImagerParam.json", <- 未指定でデフォルト値を使う事も可
>         "hash-file-path-1": "./hts/hash-1.vxx-4",
>         "hash-file-path-2": "./hts/hash-2.vxx-4",
>         "apply-round-calc": false <- ture で位置・角度に対し vxx-2/3 と同じ桁丸め処理を行う。デフォルトは false
> }
> (3) 64bit isg の使い方 ( vxx-4 にはコード埋込済だが、他は hts_beta_fvxx 依存 )
>   0-byte : shot 内での通番(0~)
>   1-byte : shot 内での通番(0~)
>   2-byte : shot 内での通番(0~)
>   3-byte : ShotID[0] = col[0]
>   4-byte : ShotID[1] = col[1]
>   5-byte : ShotID[2] = row[0]
>   6-byte : ShotID[3] = row[1]
>   7-byte : join 時に zone を埋め込み uniqueness を担保するために予約
> (4) HTS1 の ShotID, ImagerID, ViewID の定義メモ for beta-file ( HTS2 は未確認 )
>   uint32_t NumberOfImager = 72;
>   uint32_t ViewID = ;
>   uint32_t ImagerID = CameraID * 12 + SensorID; -> ImagerID(0-71), CameraID(0-5), SensorID(0-11)
>   uint32_t ShotID = ViewID * NumberOfImager + ImagerID;
> ```
> MEMO : HTS 関連コードは NetScanData/hts_wrapper.hpp に集約。TrackFilter.h BetaStruct.hpp htstools.hpp の部分コピー借用。  

##### BaseTrack
>
>  | 拡張子 | 説明 |
>  |----|----|
>  | vxx-2 | 従来の vxx |
>  | vxx-3 | 従来の vxx + ファイル圧縮（内部ハッシュ単位） |

---

#### 新規 vxx の追加方法 

##### MicroTrack
 1. PMicroTrackBlock.cpp 内の get_Version() と CreateMicroTrackBlock() に新規 vxx を追記。　
 2. ファイル PMicroTrackBlock_ver_##.h を作成し、PMicroTrackBlock ( PMicroTrackBlock.h で定義 ) の継承クラス PMicroTrackBlock_## を記述。
 3. 作成した PMicroTrackBlock_ver_##.h を PMicroTrackBlock.cpp で include する。


##### BaseTrack
 1. PBaseTrackBlock.cpp 内の get_Version() と CreateBaseTrackBlock() に新規 vxx を追記。
 2. ファイル PBaseTrackBlock_ver_##.h を作成し、PBaseTrackBlock ( PBaseTrackBlock.h で定義 ) の継承クラス PBaseTrackBlock_## を記述。
 3. 作成した PBaseTrackBlock_ver_##.h を PBaseTrackBlock.cpp で include する

