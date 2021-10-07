---
#### l2c ( l2c-x )
---

#### description
> このコードは、濱田要氏と白石卓也氏によって書かれたconnect.cppの更新版です。  
> チェーンを構築するためのアルゴリズムは、オリジナルから変更されていません。  
> 4Gバイトを超える入力ファイルではオリジナルバージョンが機能しないことに注意してください。
>  
> 入力linklet-dump-filesは次のいずれかの方法で指定できます
> - 複数のファイル名 例: linklet-dump-file-1.txt linklet-dump-file-2.txt のように引数を並べて書く
> - リストファイルを @ 付きで 例: @list-file.txt リストファイルにはファイル名を1行ずつ書く
>  
> l2c-x（Komataniのバージョンのl2c）には以下の制限があります。
> - --binary-inputは利用できません。
> - UsePos のみを使い、プレートの物理的な並び順に合わせて記述する事。EndPos と UnUsePos は使用できません。  
> - 全 group リスト ( out_gr.txt ) を出力するには、--debugを指定します。
> - PmPeke は利用できません。渡した linklet は全て接続するので、例えば PmPeke=1 としたい場合は隣接 linklet のみを渡して下さい。  
> - 3-skip 以上 ( i.e. PmPeke &ge; 4 ) の linklet に対する畳み込みは不完全です。コード内並列処理の順序に依存して畳み込まれない linklet が残り得ます。  
>  
> グループとは、少なくとも1つのトラックセグメントを共通に持つチェーンの集合です。  
> これは、グループ内の2つのチェーンがトラックセグメントを共有しているという意味ではないことに注意してください。  
> 例えば、チェーンAとチェーンBが共通のセグメントS1を有し、チェーンBとチェーンCは共通のセグメントS2を有する場合、  
> チェーンA,B,Cは同一グループ内にありますが、チェーンAとCには共通のセグメントはありません。  
>

#### usage
> #### l2c [ options ] ( *linklet-dump-file-1* *linklet-dump-file-2* ... | *@linklet-dump-file-list* )
>   

#### options
- --rc chain.rc
> set runcard file  

- --binary-input [ record-size ]
> tell l2c that input linklet-dump-files are in binary format ( i.e. created with option '--format 0' )  
> you need to give record-size in byte as a 2nd argument,  
> if your binary file is a subset or superset of linklet_t and has different record-size.  

- --output-isolated-linklet
> *This option is for t2l-x only.*  
> output isolated linklets as chains ( i.e. output 2 segment chains ). 

- --o fname-out
> *This option is for l2c-x only.*  
> specify output file. default output is to fname-out = "chain_dat.txt".  
> fname-out = "-" is to stdout.  
 
- --over fname-over
> *This option is for l2c only.*  
> specify output file for groups having more than Chain::UpperLim chains.  
> default is "over_upperlim.dat".  

#### runcard example
```
[Chain]
#UsePos = 10	10 20 30 40 50 60 70 80 90 100
	# 使う pos を指定したいときに。１つ目の数字は使う pos の数で、それより後ろは使う pos の羅列
#UnUsePos = 12	10 20 30 40 50 70 80 120 130 150 170 180
	# 使わない pos を指定したいときに。書式は UsePos と同じ
EndsPos = 10 100
	# どの pos からどの pos までのように指定したいときに。両端の pos を指定する
PmPeke = 2  # chainをつくるときの許容ペケ
UpperLim = 1000000
	# group 内の chain 数がここで指定した数を上回ればその group の chain は出力しない。
	# chain 数、group 数のカウントにも入れない。
	# そのかわり、over_upperlim.dat というファイルにその group に属する linklet を出力。
Format = 0
	# 0 なら new format、1 なら int 型 ("-1"と"-2")、2 ならchar型("*******"と"-------")で出力
```
> これらを指定しなければ、全ての pos、linklet の許容ペケ、UpperLim なし、new format で chain をつくります。  
> UsePos, UnUsePos, EndsPos はどれか１つしか指定できません。  
> コメントアウトは前に # をつけてください。  

#### output file format
```
start_plate と end_plate の番号は、ヘッダー2行目の pos の順につけた通し番号 (1～) になっています。
chain 番号、group 番号はすべて通し番号で順に並んでいるので、
chain 数や group 数を知りたければ chain ファイルの一番下の chain を見ればわかります。 
header の例（old format、new format）  
#	N_pos = 5
#	UsePos = 50 60 70 80 90
#	PmPeke = 2
#
上から、
・plate枚数
・plate ID
・許すpeke数
（headerの行の先頭には#が付いています。行数は10です。）
```

```
DATAの例（old format） 
     1015       1010        941        893   1   5   3   0   1     286035     325435         -1         -1     280943
     1016       1011        942        894   1   5   3   0   1     286094         -1         -1     313508     281671
     1017       1012        943        894   3   5   3   0   0         -2         -2     316423     313508     281671
$1=chain ID
$2=start(root)が同じchain達につけたグループID
$3=start(root)とend(leaf)が同じchain達につけたグループID
$4=同じネットワークに属するchain達につけたグループID（いわゆるGroup ID）
$5=start plate
$6=end plate
$7=nseg
$8=1ペケの数　　　　この辺は、許すペケの数で変わる。
$9=2ペケの数
$10～ segment ID 
```

```
DATAの例（new format=format 0)  
893	1	1	5	3	1	1	1.000000	0.000000
	1015	1010	941	1	5	3	0	1
		286035	325435	-1	-1	280943
894	2	1	5	4	2	1	1.200000	0.447214
 	1016	1011	942	1	5	3	0	1
		286094	-1	-1	313508	281671
  	1017	1012	943	3	5	3	0	0
		316423	313508	281671
Groupのheader、chainのheader、chainに対して段をつけています。
```
```
Groupのheader（段が下がってない行）
$1=Group ID
$2=Groupに属するchain数
$3=Groupのstart plate（最先端segの、UsePosで設定した最初のplate番号を1としたplate番号。Pos/10 ではない）
$4=Groupのend plate（最終端segの、UsePosで設定した最初のplate番号を1としたplate番号。Pos/10 ではない）
$5=Groupが持つsegment数
$6=rootの数（先端のseg数）
$7=leafの数（終端のseg数）
$8=1plate内のsegment数の平均値（ペケもsegmentとしてカウント）
$9=$8のσ
```

```
chainのheader（段が1段下がっている行）
$1=chain ID
$2=root と leaf が同じ chain に対して同じ値になる ID
$3=root が同じ chain に対して同じ値になる ID
$4=start plate (rootのplate)
$5=end plate（leafのplate）
$6=nseg
$7=1ペケの数　　　　許すペケの数 PmPeke = 1 以上の指定で出力される
$8=2ペケの数        許すペケの数 PmPeke = 2 以上の指定で出力される
```

```
chain（段が2段下がっている行）
$1～ segment ID
・old formatでは"-------"や"-2"となっていたものをなくしました。
　（なのでstart plateをみて対応させなければなりません）
・ペケの書式はint型
```

```
footerの例（new format） 
#	N_linklet = 2597
#	N_folded-linklet = 434
#	N_chain = 1788
#	N_gcomm = 1576
#	N_groot = 1664
#	N_gends = 1774
上から、
・linkletの総数（同じlinkletがあった場合はカウントしていない）
・linklet総数のうち、たたみ込まれたLinklet数
・chain数
・Group数
・「rootが同じ」グループ数
・「rootとleafが同じ」グループ数
```

```
l2c の group-file ( UpperLim を越える group の linklet リスト )
# no_group n_chains pl-start pl-end n_seg n_root n_leaf mean_n_seg sigma_n_seg
	pos0 id0 pos1 id1 <-- n_chain 繰返し
```

```
l2c-x の group-file ( --debug で出力。全 group のリスト )
G #-of-tracks #-of-linklets
	T pos id <-- #-of-tracks 繰返し
	L pos0 id0 pos1 id1 <-- #-of-linklets 繰返し
```

#### l2c-x の処理工程
> 早川氏のメモより
> 1. Linkletの読み込み (読み込みと解釈でマルチスレッド)
> 2. Groupの生成(BaseTrackとLinkletのグループ化のみ、Chainはつくらない)
> 3. Chainを生成しつつテキスト出力
