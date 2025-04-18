---
#### **NETSCAN 作成に必要なツール**
---

+ visual c/c++ 2017 ( 以降のバージョンも対応あり )
+ make
+ perl
+ git
+ boost ( ver-2016-09-01 では boost_1_65_0 を ver-2024-09-01 では boost_1_83_0 を使用 )
+ jemalloc ( ver-2016-09-01 では jemalloc-5.2.1 を ver-2024-09-01 では jemalloc-5.3.0 を使用 )

  > boost and jemalloc は Z: にインストールされているフォルダ構成を標準としている。  
  > それ以外のフォルダ構成の場合は config\local 内に header-win-msvc-x64-15.mak 等を作成してカスタマイズ可能。  
  > Minimum set of 64bit cygwin package is avaliable [here](cygwin.zip).  

---
#### **NETSCAN のビルド**
---

+ リポジトリからの複製 ( M:\prg\netscan 以下に ver-2024-09-01 を複製する場合 )
  - cd M:¥prg¥netscan  
  - git clone --branch ver-2024-09-01 https://github.com/NETSCAN-dev/NETSCAN2.0 ver-2024-09-01  

+ ビルド環境作成 ( win-msvc-x64-15 - visual c/c++ 2017 64bit 版 - の場合 )
  - cd M:\prg\netscan\ver-2024-09-01\config
  - perl objdir.pl win-msvc-x64-15

+ ビルド
  - cd M:\prg\netscan\ver-2024-09-01\obj\win-msvc-x64-15
  - make clean all install clean-obj

+ リポジトリにコミットされた更新履歴のインポート
  - cd M:\prg\netscan\ver-2024-09-01
  - git pull
