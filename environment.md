---
#### environment setting to run netscan programs
---

+ boost ( 1.65.0 for ver-2016-09-01 and 1.59.0 for earlier versions )
  - add Z:\usr\boost\boost_1_65_0\stage\lib64 and Z:\usr\boost\boost_1_65_0\stage\lib32 to PATH
  - add Z:\usr\boost\boost_1_59_0\stage\lib64 and Z:\usr\boost\boost_1_59_0\stage\lib32 to PATH
+ jemalloc ( Komatani patch is required )
  - add Z:\usr\jemalloc\bin64 and Z:\usr\jemalloc\bin to PATH
+ root ( required for 32bit only )
  - set ROOTSYS=z:\usr\root\root_v5.34.32_vc10
  - add %ROOTSYS%\bin to PATH
+ appropreate version of visual c/c++ runtime

  > boost, jemallocは必須です。これらの Path 設定は 32bit と 64bit を同時に指定して良いです。  
  > また、上記 root 設定は l_pic, cpic_rel など本ドキュメント記載のツールを使う場合の設定です。  
  > HTS 処理で使っている root ツールの環境設定とは別です。  
  
  > C/C++ runtime というのは Visual C++ 20XX 再頒布可能パッケージと呼ばれているもので、ウェブから最新版をダウンロードできます。  
  > どれが必要か分からない場合、```C++ 2010 x86, C++ 2010 x64, C++ 2013 x86, C++ 2013 x64, C++ 2015 x86, C++ 2015 x64``` の  
  > 6つを入れておくことをお勧めします。
