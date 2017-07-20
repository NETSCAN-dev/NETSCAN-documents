---
#### l2c
---

+ usage : l2c [ options ] ( *linklet-dump-file-1* *linklet-dump-file-2* ... | *@linklet-dump-file-list* )

  > This code is an updated copy of connect.cpp written by K.Hamada and by Shiraishi.  
  > Algorithm to build chain is not modified from the original.  
  > Be careful that the original version does not work with input file larger than 4Gbytes.  
  >  
  > Input linklet-dump-files can be specified in ...  
  > 1. Multiple file-names in sequence like linklet-dump-file-1 linklet-dump-file-2 ...  
  > 1. A list-file like @list-file  
  > .
  >  
  > l2c-x ( Komatani's version of l2c ) has restrictions below.  
  > 1. 2 segment chains ( isolated linklets ) are rejected.  
  > 2. --binary-input is not available.
  > 3. EndPos and UnUsePos can not be used in runcard. One should use UsePos and plates should be sequential.
  > 4. specify --debug to enable out_gr.txt output.  
  > .
  >  
  > Group means a set of chains, which have at least one track-segments in common.  
  > Be careful that this does not mean that any two chains in a group have track-segments in common.  
  > For example, chain A and chain B have a common segment S1 and chain B and chain C have a common segment S2.  
  > In this case, all chains A,B,C are in a group but chain A and C have no segments in common.  
  >  

+ options
  - --rc chain.rc
  > set runcard file  

  - --binary-input [ netscan_data_types_ui.h -- ]
  > tell l2c that input linklet-dump-files are in binary format ( i.e. created with option '--format 0' )  
  > you need to give 2nd and 3rd ('--') arguments to read customized-binary-linklet-dump-file correctly.  

+ runcard ( m:/prg/netscan/ver-2016-09-01/rc/chain.rc )
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

+ output file format

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
DATAの例（new format)  
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
$3=Groupのstart plate（最先端segのplate番号）
$4=Groupのend plate（最終端segのplate番号）
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
$7=1ペケの数　　　　この辺は、許すペケの数で変わる。
$8=2ペケの数
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
