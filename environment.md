---
#### environment setting to run netscan programs
---

+ boost ( 1.65.0 for ver-2016-09-01 and 1.59.0 for earlier versions )  
  - add Z:\usr\boost_1_65_0\stage\lib64 and Z:\usr\boost_1_65_0\stage\lib32 to PATH  
  - add Z:\usr\boost_1_59_0\stage\lib64 and Z:\usr\boost_1_59_0\stage\lib32 to PATH  
  > those path are for f-lab intranet and depends on where you installed boost.  
+ jemalloc ( 5.1.0 for ver-2016-09-01 and Komatani-patch for earlier versions )
  - add Z:\usr\jemalloc-5.1.0\dll64 and Z:\usr\jemalloc-5.1.0\dll to PATH
+ root ( required for 32bit only )
  - set ROOTSYS=z:\usr\root\root_v5.34.32_vc10
  - add %ROOTSYS%\bin to PATH
+ appropreate version of visual c/c++ runtime
  - 対応している Visual C/C++ は vc-2017 です。( vc-2013 以前は対応打ち切り )  
  - C/C++ runtime というのは Visual C++ 20XX 再頒布可能パッケージと呼ばれているもので、ウェブから最新版をダウンロードできます。  


  > boost, jemallocは必須です。これらの Path 設定は 32bit と 64bit を同時に指定して良いです。  
  > また、上記 root 設定は l_pic, cpic_rel など本ドキュメント記載のツールを使う場合の設定です。  
  > HTS 処理で使っている root ツールの環境設定とは別です。  
