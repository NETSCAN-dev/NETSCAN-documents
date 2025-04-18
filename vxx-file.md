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
>  | 拡張子 | 説明 |
>  |----|----|
>  | vxx-2 | 従来の vxx |
>  | vxx-3 | 従来の vxx + ファイル圧縮（内部ハッシュ単位） |
>  | vxx-4 | HTS beta-file (*) |  
>
> (*) beta_raw.dat を beta_raw.vxx-4 と拡張子を変更する必要あり。同一フォルダに hash-1.vxx と hash-2.vxx を作成する。


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

