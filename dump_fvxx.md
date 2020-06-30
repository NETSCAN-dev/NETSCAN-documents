---
#### dump_fvxx
---

#### description
> micro-track vxx ファイルをダンプする。  
> 出力する情報の詳細は [こちらを参照](dump_fvxx_columns.md) の事。  

#### usage
> #### dump_fvxx *vxx-file* *pos* *zone* [*options*]

#### options
- --index i0 i1
> limit dump for i0 &le; rawid < i1  

- --a xmin xmax ymin ymax  
  --a list-file
> select on xy  
> list-file contains ```xmin xmax ymin ymax axmin axmax aymin aymax``` in each line and can have multile lines  

- --format 0/1/2/3/4
> 0 : rawid isg ph ax ay z1 z2 ( NF=7, so-called f-file format with 4 header lines )  
> 1 : pos zone rawid isg ph ax ay x y z z1 z2 px py col row f ( NF=17, f is obsoleted )  
> 2 : pos rawid 0 ( NF=3, to create aux-file for this micro-track-block )  
> 3 : same as --format 2 but with one more digits for positions and angles.  
> 4 : binary output ( see [[class description|netscan-data-types-ui]] or m:/prg/netscan/sample/2/binary-io/ ).  
>  
> - xy position of micro-track is at z-center of an emulsion layer  
> - HTSから出力するときのxy座標は 0-15層中の7層目のxy座標で、micro-trackの真の中心座標ではない。  
>   ベース面に外挿する際、レンズ側乳剤層では8層、ステージ側乳剤層では7層分外挿すべきはず（吉本）。  
  
- --c correction-map-file-absolute  

- --ph phmin  

- --affine a b c d p q  
> apply affine transformation ( after correction-map-absolute if specified )  
