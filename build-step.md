---
## ビルド環境の準備
---

### Windows + MSYS2 の場合

- MSYS2
- git ( MSYS2版は非推奨, Git for Windowsを推奨 )
- GNU make ( MSYS2版 )
- perl ( MSYS2版 )
- Visual C++ 2017 またはそれ以降
- Windows SDK ( Visual Studio 付属のものを推奨 )

あらかじめ、MSYS2 の usr\\bin ディレクトリへのパスを通しておいてください。

> コマンドプロンプトで MSYS2 のツールを使うには、  
> MSYS2 を Z:\usr\msys64 にインストールした場合、  
> PATH に Z:\usr\msys64\usr\bin を追加し、  
> 環境変数 MSYSTEM を設定する ( ex. set MSYSTEM=UCRY64 )。  
> MSYSTEM を設定しないと jemalloc のビルドでエラーが発生する。  
>

### Windows + Cygwin の場合

- Cygwin
- git ( Cygwin版は非推奨, Git for Windowsを推奨 )
- GNU make ( Cygwin版 )
- perl ( Cygwin版 )
- Visual C++ 2017 またはそれ以降
- Windows SDK ( Visual Studio 付属のものを推奨 )

あらかじめ、cygwin の bin ディレクトリへのパスを通しておいてください。

---
## ライブラリの準備
---

ビルド済みのライブラリは Z:\\usr 以下に置いてあります。

- boost
- jemalloc ( 5.2.1以降を推奨 )
- zstd

---
## リポジトリのダウンロード
---

ver-2016-09-01 を M:\prg\netscan\ver-2016-09-01 にダウンロードする場合  
- cd M:¥prg¥netscan  
- git clone --branch ver-2016-09-01 https://github.com/NETSCAN-dev/NETSCAN2.0 ver-2016-09-01  

ver-2024-09-01 を M:\prg\netscan\ver-2024-09-01 にダウンロードする場合  
- cd M:¥prg¥netscan  
- git clone --branch ver-2024-09-01 https://github.com/NETSCAN-dev/NETSCAN2.0 ver-2024-09-01  

---
## ビルド手順
---

Windows で Visual C++ 2017 を用いて64bitでビルドする場合

1. boost, jemalloc, zstd ライブラリのファイル群を Z:\\usr からローカルにコピー
2. リポジトリの config\\local ディレクトリに移動
3. ファイル header\-win\-msvc\-x64\-15\-build.mak を以下の内容で作成する

	WITH_BOOST = boostのコピー先パス  
	WITH_JEMALLOC = jemallocのコピー先パス  
	WITH_ZSTD = zstdのコピー先パス  

4. "perl ../objdir.pl win\-msvc\-x64\-15\-build" を実行
5. リポジトリに obj\\win\-msvc\-x64\-15\-build ディレクトリが生成されるので、そこに移動
6. "make" を実行して、正常終了を確認
7. "make install" を実行すると、obj\\win\-msvc\-x64\-15\-build\\bin に実行ファイルがコピーされる

ディレクトリのパスに空白文字は使えません。また、ディレクトリの区切り文字は \\ ではなく / です。  
他の場合は、win\-msvc\-x64\-15 の部分を対応するデフォルトターゲット名に置き換えてください。

---
## ターゲット名
---

ターゲット名の解説です。

### ターゲット名に使用できる文字

ターゲット名に使用できる文字は、数字、英小文字、\_、\- です。  
このうち、\- は区切り文字で、前後に他の種類の文字がある必要があります。
ただし、ターゲット名に \-\_\- 、 \_\_ を含んではいけません。

### ターゲット名の派生

ターゲット名 *TARGETNAME* について、 *TARGETNAME*\-*SUFFIX* として *SUFFIX* を追加して、派生したターゲット名を作ることができます。

### プラットフォーム別のデフォルトターゲット名

デフォルトターゲット名をそのまま使用することは推奨しません。
上記のビルド手順のように、デフォルトターゲットから派生したターゲット名を作ることを推奨します。

- Windows 32bitバイナリ用
  - ~~win\-msvc\-12 \- Visual Studio 2013 向け~~ 対応打ち切り
  - ~~win\-msvc\-14 \- Visual Studio 2015 向け~~ 対応打ち切り
  - win\-msvc\-15 \- Visual Studio 2017 向け
- Windows 64bitバイナリ用
  - ~~win\-msvc\-x64\-12 \- Visual Studio 2013 向け~~ 対応打ち切り
  - ~~win\-msvc\-x64\-14 \- Visual Studio 2015 向け~~ 対応打ち切り
  - win\-msvc\-x64\-15 \- Visual Studio 2017 向け
  - win\-msvc\-x64\-16 \- Visual Studio 2019 向け
  - win\-msvc\-x64\-17 \- Visual Studio 2022 向け
- GNU/Linux x86\_64用
  - linux\-gcc\-centos7dts7 \- CentOS 7 devtoolset\-7 向け

### 特定の目的に予約されたターゲット名

ターゲット名が被らないようにするための措置です。  
文字 / で挟んだ部分はターゲット名に含まれる正規表現です。  
これらから派生したターゲット名も該当します。

- /\-example\\d?$/ \- 解説専用のターゲット名
- /\-build\\d\*$/ \- ローカルでのビルド用ターゲット名
- /\-local\\d\*$/ \- ローカルでのビルド用ターゲット名
- /\-user\-\[\\w\_\]\+$/ \- ユーザー毎のビルド用ターゲット名( \-user\- の直後にユーザー名)
- /\-exp\-\[\\w\_\]\+$/ \- 実験毎のビルド用ターゲット名( \-exp\- の直後に実験名)


