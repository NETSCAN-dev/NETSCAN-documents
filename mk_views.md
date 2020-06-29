---
#### mk_views
---

#### description
>
> #### [処理区画リスト](view-list.md) を作成する
> <br>  
>
> pos0 ( plate &times; 10 + face ) 上の track が存在する領域を、--view で指定した view-step と view-overlap で区割りして、[処理区画リスト](view-list.md) を作成する。  
> 具体的には、まず view-step 単位で区割りした区画を作成しておき、これらを view-overlap だけ全方向に拡大する。  
> view-step 単位での区割りの際に生じた余りは、最外周の区画に繰入れる。  
>
> ここで作成した [処理区画リスト](view-list.md) は、[dc](dc.md), [m2b](m2b.md), [ali-g](ali-g.md), [ali-l](ali-l.md), [t2l](t2l.md) などの NETSCAN ツールで使用する。  
> これらのツールでは、--view オプションを使って処理区画を dynamic に作成する事も可能だが、  
> 処理内容をユーザーが直接管理できるべきという設計思想の観点から、[処理区画リスト](view-list.md) をファイルとして作成し、各ツールに渡す事を推奨する。
>

#### usage
> #### mk_views *pos0* *pos1* [*options*]  
>
> 使用例  
> pl01 の face=1 ( pos=11 ) と face=2 ( pos=12 ) の micro-track を使う場合  
> ```
> mk_views 11 12 –descriptor ./eventdescriptor.ini --view 2000 1000 --o ./pl001/dc-views.lst
> ```
> pl01 の base-track ( pos=10 ) と pl02 の base-track ( pos=20 ) を使う場合  
> ```
> mk_views 10 20 –descriptor ./eventdescriptor.ini --view 2000 0 --o ./pl001/ali-002-views.lst
> ```

#### options ( those are in **bold** must be given )
- **--descriptor [event-descriptor](event-descriptor.md)**
> set [event-descriptor](event-descriptor.md)  

- --io fname-io
> override [event-descriptor](event-descriptor.md) entries  

- **--view view-step view-overlap**
> 処理区画リストを作成する際の view-step と view-overlap を指定する。  
> パラメータの意味は description を参照の事。  

- --o fname-view
> set output [view-list](view-list.md) filename  

- --rc fname-rc
> set runcard filename in [view-list](view-list.md) file  

- --sort 0/1
> sort views by line(0) or by spiral-around-center(1). default = 0  
