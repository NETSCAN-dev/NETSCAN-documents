---
#### f_filter
---

#### description
> micro-track vxx ファイルに対するフィルター  

#### usage
> #### f_filter *pos* *vxx-file* [*options*]  
> <br>
> 使用例 : pl01 の face=1 micro-track vxx に dr&lt;5&mu;m、d&theta;&lt;50mrad でゴーストフィルターをかける。  
> f_filter 11 f0011.vxx --o filtered_f0011.vxx --ghost 5 0.05  

#### options ( those are in **bold** must be given )
- **--o output-vxx-file**
> set output vxx-file

- --page-size val
> set vxx page-size ( default is that of input vxx file )  

- --view view-width view-overlap
> 処理区画の指定。出力の際に view-width 単位でハッシュしなおす。  
> このため view-width を後段処理での処理区画程度に留めないと読込に無駄な時間を要する。  
> また ghost-filter を使う場合には、区画境界に十分な重なり ( view-overlap ) を指定する事。  

- --xy xmin xmax ymin ymax
> select on xy  

- --[filter-list](filter-list.md) filter-file-name +1/-1
> specify [[filter-file-name|[filter-list](filter-list.md)]] to include (+1) or to exclude (-1)  

- --filter-max-track-per-view val
> reject view which has a given number of tracks or more  

- --density val
> reject views which has a track density > val tracks/cm2 ( to be replaced with --filter-max-track-per-view )  

- --pos new-pos
> change pos  

- --afp a b c d p q
> apply affine transform on position x-y  

- --aft a b c d p q
> apply affine transform on angle ax-ay  

- --ph nbin wbin val ...
> apply phmin cut  

- --vol nbin wbin val ...
> apply volmin cut  

- --phmax nbin wbin val ...
> apply phmax cut  

- --volmax nbin wbin val ...
> apply volmax cut  

- --append
> `-o` で既存のファイルがあったとき、vxxファイルにマイクロトラックを追加する。 
> 既存ファイルがあり、このオプションを使わなかったとき、元ファイルは削除して新規作成される

- --ghost dr dt
> x-y でのゴーストフィルタ   
> 各飛跡 a に対して、位置差 sqrt(dx<sup>2</sup>+dy<sup>2</sup>) &lt; dr かつ、角度差 sqrt(d&theta;x<sup>2</sup>+d&theta;y<sup>2</sup>) &lt; dt の領域に飛跡 b が存在し、  
> それらが ph-vol のより小さな、あるいは ph-vol が等しく rawid がより小さな、飛跡である場合、b をゴーストとして排除する。  

- --ghost-rl dr_lat dr_rad dt_lat dt_rad cr_rad ct_rad
> radal-lateral でのゴーストフィルタ   
> 各飛跡 a に対して、`自身の角度`で radial-lateral 方向を決め、下記の領域に飛跡 b が存在し、  
> それらが ph-vol のより小さな、あるいは ph-vol が等しく rawid がより小さな、飛跡である場合、b をゴーストとして排除する。  
> + lateral 方向での位置差 &lt; dr_lat かつ、角度差 &lt; dt_lat  
> + radial 方向での位置差 &lt; dr_rad + cr_rad &times; th かつ、角度差 &lt; dt_rad + ct_rad &times; th ( th は自身の space-angle )  

- --isg-overwite  
> isg を 0 始まりの通番に変更する。  

- --isg-embed-col-row  
> isg の上位バイトに col,row に埋め込まれた shotID を埋め込む。  
> 詳細は[ここを参照](vxx-file.md)して下さい。  

##### 上記 2 つのゴーストフィルタに関する注意書き
> + 位置は micron、角度は rad 単位での標記です。これは NETSCAN コードでのデフォルトです。  
> + micro-track ( vxx-file ) の位置 xy は各乳剤層の中央の z 面での値です。  
> + 内部での計算に、位置は double、角度は float を用いているため、dump 出力を用いた double での計算とは若干のズレはあり得ます。  
> + フィルター後に出力される飛跡は`元の rawid ではソートされません`。出力順序を前提としないで下さい。  
