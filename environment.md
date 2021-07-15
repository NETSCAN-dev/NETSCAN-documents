---
#### environment setting to run netscan programs
---

+ boost
  - add Z:\usr\boost_1_65_0\stage\lib64 to PATH


+ jemalloc ( 5.2.0 はメモリリークする )
  - add Z:\usr\jemalloc-5.2.1\dll64 to PATH


+ appropreate version of visual c/c++ runtime
  - 対応している Visual C/C++ は vc-2017 です。( vc-2013 以前は対応打ち切り )
  - C/C++ runtime というのは Visual C++ 20XX 再頒布可能パッケージと呼ばれているもので、ウェブから最新版をダウンロードできます。

> 上記 boost, jemalloc の Path 設定は F 研内での一例であり、インストール環境に応じて適宜修正が必要です。  

