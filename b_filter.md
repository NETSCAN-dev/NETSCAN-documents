---
#### b_filter
---

#### description
> base-track vxx ファイルに対するフィルター  

#### usage
> #### b_filter *pl* *vxx-file* [*options*]
> <br>
> 使用例 : pl01 の base-track vxx に dr&lt;5&mu;m、d&theta;&lt;30mrad でゴーストフィルターをかける  
> b_filter 01 b001.vxx --o filtered_b001.vxx --ghost 5 0.03  

#### options ( those are in **bold** must be given )
- **--o output-vxx-file**
> set output vxx-file  

- --page-size val
> set vxx page-size ( default is that of input vxx file )  

- --view-width val
> set view witdh for fiter-processing  

- --xy xmin xmax ymin ymax
> select on xy  

- --[filter-list](filter-list.md) filter-file-name +1/-1
> specify [[filter-file-name|[filter-list](filter-list.md)]] to include (+1) or to exclude (-1)  

- --filter-max-track-per-view val
> reject view which has a given number of tracks or more  

- --density val
> reject views which has a track density > val tracks/cm2 ( to be replaced with --filter-max-track-per-view )  

- --pl new-pl
> change pl  

- --afp a b c d p q
> apply affine transform on position x-y  

- --aft a b c d p q
> apply affine transform on angle ax-ay  

- --ph nbin wbin val ...
> apply phmin cut. cut is on the sum of ph of micro-tracks.  

- --vol nbin wbin val ...
> apply volmin cut. cut is on the sum of vol of micro-tracks.  

- --append
> append base tracks to output vxx file  

- --ghost dr dt
> ghost filter in position (dr micron) and angle (dt rad).  
> `--ghost 5 0.05` で 5&mu;m 50mradでのゴースト消しになる。  

- --edit fname-edit
> edit limited properties of each base track ( x,y,col,row are not accepted ).  
> 'fname-edit' format is that of [dump_bvxx](dump_bvxx.md) output with --format 0/3.  

- --cut-rl face1-rad-0 face1-rad-1 face1-lat-0 face2-rad-0 face2-rad-1 face2-lat-0
> radial-lateral cut  
> face1/2-rad max difference = face1/2-rad-0 + face1/2-rad-1 x base-angle  
> face1/2-lat max difference = face1/2-lat-0  

- ---trapezoid a0 b0 c0 a1 b1 c1 a2 b2 c2 a3 b3 c3  
> apply projective transformation  
> `X = ( a0*x+b0*y+c0 )/( a1*x+b1*y+c1 )`  
> `Y = ( a2*x+b2*y+c2 )/( a3*x+b3*y+c3 )`  

#### FAQ
- **Q** --ghost で使われる xy 座標の基準 z面はどこ ?  
  **A** base-track の xy 座標。現状で face=1 のベース表面。即ちベーストラックにおけるz=z1、(x, y)を使っている。ベーストラックの対称性からするとベーストラックの中央のXY座標でクラスタリングすべきだが、そうはなっていない。  
- **Q** 角度ごとにベーストラックとマイクロトラックの角度差を使ったカットをかけることはできるか ?  
  **A** できない。  
- **Q** 自分が定義したカット条件を適用する方法は ?  
  **A** 現状では b_filter のソースをベースに自分でコードを書くのが現実的解か ...  
