---
#### m2b
---
#### 重要 : base-track メンバの isg は vxx ファイル内で一意ではないので track ID としては使わないで下さい。バグですが放置します。
---
#### description
>
> #### プレート両面の micro-track に distortion と shrink 補正を適用し base-track を作成する  
> <br>  
>  
> #### 接続判定  
>
> micro-track の接続判定は abs( &theta;xy<sub>micro</sub> - &theta;xy<sub>base</sub> ) < ErrAng + ErrDist + ErrShur &times; &theta;xy<sub>base</sub> で行う。  
> runcard での接続条件の設定には、固定項には ErrAng を、角度依存項には ErrShur を使い、ErrDist は常に 0 とする事を推奨。  
> runcard のパラメータ名などは要整理ではあるが、今後の課題とする。  
> 
> Algorithm = 2 で radial-lateral 空間での角度差によるカットが可能 ( [下記参照](#runcard-example) )。  
> これは、通常の xy 空間での角度差による接続判定との論理積として実装しており、  
> lateral 方向の角度差カットによるノイズ除去を想定したもの。  
>
> #### track 位置の z 面  
>
> **base-track の xy は face=1 乳剤層ベース表面での値。**  
> z は [eventdescriptor](event-descriptor.md) を使って読み出す場合は chamber 構造内での z に設定しており、  
> それ以外の場合 ( dump_bvxx 等 ) は z=0 としている。  
> dump_bvxx の場合は option --z で明示的に指定もできる。  
> なお base-track メンバの micro-track の角度は、distortion と shrink 補正を適用した後の値である。  
>
> **micro-track の xy は、基本的に読み出し系依存だが、概ね乳剤層中央での値。**  
> z は base-track 同様に、[eventdescriptor](event-descriptor.md) を使って読み出す場合は chamber 構造内での z に設定しており、  
> それ以外の場合は z=0 となる ( micro-track vxx を作成する際に z=0 としている )。  
> z1,z2 はステージ座標そのままの値なので注意。  
>> base-track の xy は、m2b 内部で micro-track の位置が乳剤層中央のものであると仮定し、それをベース表面に延長して求めている。  
>> 実際には face=1 micro-track の場合は z2 に、face=2 micro-track の場合は z1 に延長している ( [geometry](geometry-descriptor.md) の face definition 参照 ) ため、  
>> **厚型乳剤層の場合など、乳剤層全体を scan しない場合には対応できていない。**(忘備)  
>
> #### その他  
>
> m2b では、指定された [correction-map](correction-map.md) 記載のパラメータの中で、distortion と shirnk ( 角度 affine パラメータの a,p,q ) だけを使う。  
> dc が生成する [correction-map](correction-map.md) には、各乳剤層とベースの厚み各々の nominal 値 ( [parts.kar](geometry-descriptor.md) に記載 ) からの差も記録されているが、  
> これらは eventdescriptor 経由での micro-track 読み出しの際に、micro-track の位置 xyz の値を正しく設定するのに使っている。  
>   
> 使用する [correction-map](correction-map.md) の中で、ある significance 以上のエントリを使う等、  
> フィルターをかけたい場合は、予めフィルタ済の [correction-map](correction-map.md) を用意して下さい。  
> フィルタの記録にもなるし、テキストファイルなので簡単だと思います。  
>
> m2b 内部での使用スレッド数は、処理区画のループでは `std::min(GetProcessorInfo().nCore,6)` 、  
> 内部アルゴリズム ( ConnectBase2 ) では `0.65*GetProcessorInfo().nCore` としている。  
>   
> For view size larger than 32.5mm,  
> micro-track's position resolution in connection calculation  
> becomes x2 worse than basic resolution of 0.015micron.  
>

#### usage
> #### m2b pl[-zone] [options]
>

#### options ( those are in **bold** must be given )
  - **--descriptor [event-descriptor](event-descriptor.md)**
  > set [event-descriptor](event-descriptor.md)  

  - --io fname-io
  > override [event-descriptor](event-descriptor.md) entries  

  - **--rc [runcard](#runcard-example)-file**
  > runcard を指定する。下記の [runcard example](#runcard-example) を参照。  

  - **--c [correction-map](correction-map.md)-file**
  > set [correction-map](correction-map.md) ( output of dc )  

  - **--view view-step view-overlap**  
    **--view view-list-file-name**  
  > [処理区画](view-list.md) を、view-step と view-overlap で指定するか、  
  > [mk_views](mk_views.md) で作成した [区画リスト](view-list.md) のファイル名で指定する。  
  > ファイル名での指定を推奨。  

  - --[filter-list](filter-list.md) [filter-list](filter-list.md) +1/-1
  > include (+1) or exclude (-1) [[[filter-list](filter-list.md)|[filter-list](filter-list.md)]]  

  - --filter-phmin phmin
  > phmin filter  

  - --filter-max-track-per-view value
  > avoid process in views having max-track-per-view track or more  

  - --ang-shr ang-shr-base ang-shr-shift
  > &theta; += &theta; &times; (ang-shr-shift/ang-shr-base) for abs(&theta;)≤ang-shr-base  
  > &theta; += ang-shr-base for &theta; > +ang-shr-base  
  > &theta; -= ang-shr-base for &theta; < -ang-shr-base  
  > (*) implemented by Komatani probably fot HTS specific needs.  

#### FAQ
- **Q** runcard の、ErrAng と ErrDist の違いは ?  
  **A** 接続条件の指定では等価。[ErrDist=0 での使用を推奨](#接続判定) する。  
- **Q** runcard の ErrShur は何か ?  
  **A** micro-track 対の接続で、許容する角度差に角度依存を持たせる場合に使う。[接続判定参照](#接続判定) 。  
- **Q** radial-lateral空間で接続できないのか ?  
  **A** 可能。[接続判定参照](#接続判定) 。  
- **Q** ある significance 以上の [correction-map](correction-map.md) のみを使う方法はあるか ?  
  **A** フィルタ済み [correction-map](correction-map.md) の使用を推奨。  
- **Q** どの [correction-map](correction-map.md) が使われたかの記録は残る ?  
  **A** 現状では残らない。[new_basetrack_format](new_basetrack_format.md) で挙げられている通り今後取得できるようにすべき。
- **Q** base-track のベース中央での座標 (cx, cy) はどうやって求めれば良い ?  
  **A** `cx=x+ax*(m[1].z-m[0].z)*0.5; cy=y+ay*(m[1].z-m[0].z)*0.5;`  

#### 以下は間違っていると思います。上の description に追記しましたが確認願います。
- Q. ベーストラック=bvxxのz1とz2が、マイクロトラック=fvxxのz座標と異なるのはなぜ?  
  A. 確認中。同じであるべき。  
- Q. VXXの座標(base_track_tクラス内におけるx y)のZ面はどこ?  
  A. FACE1のマイクロトラックの中央の座標。なおbase_track_tクラス内におけるzもやはりFACE1のzと同じ  
- Q. マイクロトラックの中央の座標 (cx1, cy1), (cx2, cy2)はどうやって求めれば良い?  
  A. その方法はない。理由1 Distortion補正済みなので、理由2 マイクロトラックのdz情報はbvxxには保存されていないので。  

#### runcard example
```
[MT2BT]
Algorithm   = 0
  # 0 : ConnectBase2
  # 1 : ConnectLinklet
  # 2 : ConnectBase2 に radial-lateral での角度差カット機能を追加
FACE1       = 0.0 0.0 0.0 0.000 0.000 1.00
FACE2       = 0.0 0.0 0.0 0.000 0.000 1.00
ErrAng      = 0.060
ErrDist     = 0.000
ErrShur     = 0.010
PHCUT       = 0 0.1 0
PHSUMCUT    = 0 0.1 0
VolCut      = 0 0.1 0
ErrorsAngleRL = face1-radial face2-radial face1-lateral face2-lateral
(*) 角度ズレ許容範囲 = ErrAng + ErrDist + ErrShur x ベース角
(*) ConnectLinklet を使う際の runcard は t2l 用の Mode = 0 を参照のこと
```
