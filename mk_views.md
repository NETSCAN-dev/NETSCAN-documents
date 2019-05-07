#### mk_views

+ description : create [view-list](#view-list)
+ usage : mk_views *pos0* *pos1* [*options*]  
> specify same pos for pos0 and pos1 to make a list for one plate  

+ options ( those are in **bold** must be given )
  - **--descriptor [event-descriptor](event-descriptor.md)**
  > set [event-descriptor](event-descriptor.md)  

  - --io fname-io
  > override [event-descriptor](event-descriptor.md) entries  

  - **--view view-step view-overlap**
  > set view step and overlap by value  
  > - view-size = view-step + view-overlap x 2 and it steps by view-step.  
  > - out-most areas might be enlarged to include remainng edges of area to be processed.  

  - --o fname-view
  > set output [view-list](#view-list) filename  

  - --rc fname-rc
  > set runcard filename in [view-list](#view-list) file  

  - --sort 0/1
  > sort views by line(0) or by spiral-around-center(1). default = 0  

[dc](dc.md)、[ali-g](ali-g.md)、[ali-l](ali-l.md)などのアライメントツールの引数 `--view` では、視野サイズとオーバーラップを指定することができるが、ユーザーが計算する内容を直接管理するという設計思想の観点から、視野リスト[view-list](#view-list)を作成し、アライメントツールに渡すことを推奨している。

PL01のFACE1(11)とFACE2(12)のマイクロトラックを使う場合の例は以下の通り。

```
mk_views 11 12 –descriptor ./eventdescriptor.ini --view 2000 1000 --o ./PL001/dc-views.lst
```

PL01のベーストラック(10)とPL02のベーストラック(20)を使う場合の例は以下の通り。

```
mk_views 10 20 –descriptor ./eventdescriptor.ini --view 2000 0 --o ./PL001/ali-002-views.lst
```

#### view-list

+ description
  > This is a process-view-list which can be given as an argument for option --view in core-1 programs.  
  > One can use [mk_views](#mk_views) to create for each data-set.  
  > 作成方法については[mk_views](#mk_views)参照。

+ view-list-file
```
  column  description
  01      id
  02      ix x-index 0 ...  
  03      iy y-index 0 ...  
  04-07   xmin, xmax, ymin, ymax  
          This defines an area on the second pos and is normally an outer area of the view,  
          which is overlapped with neigbouring views.  
  08-11   xmin_i, xmax_i, ymin_i, ymax_i  
          This defines an area on the first pos and is normally an inner area of the view,  
          which is NOT overlapped with neigbouring views.  
  12      fname_rc runcard file name ( use '-' not to specify runcard file )  
```

08-11の定義は、1番目の位置(dc時はFACE1のベース面、ali時は1番目のベーストラックのFACE1のベース面)の領域を定義し、通常は視野の内側の領域(=アライメントの基準側なので、04-07よりも視野サイズは小さくする)です。
隣接する視野と重なり合ってはいけない。

04-07の定義は、2番目の位置(dc時はFACE2のベース面、ali時は2番目のベーストラックのFACE1のベース面)の領域を定義し、通常は視野の外側の領域(=アライメントを探索される側なので、08-11よりも視野サイズは大きい方がよい)です。
隣接する視野と重なり合ってもよい。

* Q. 08-11の定義における重なるとどうなるの?<br>
  A. 重なった領域は複数回接続に使われるため、ゴーストが発生する。
* Q. 境界はどうなっているの?<br>
  A. XX
* Q. 角度空間をtanθ<1まで計算させるときの、04-07の視野のサイズ(=外側の領域)は、08-11の視野サイズ(=内側の領域)よりもどの程度広げるべきなの?<br>
  A. dcの場合はベース厚分は広げたほうが良い。aliの場合はベーストラック間の距離は少なくとも広げた方が良い。ベーストラック間の距離とはエマルション間のGapではなく、ベーストラックの基準面の間隔分である。これらの値は、[mk_views](#mk_views)の引数であるview-overlapそのものである。
