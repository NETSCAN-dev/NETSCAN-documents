[NETSCANのビルド方法](NETSCAN "wikilink")。 2018-10-20更新版 ver-2016-09-01 に対応。

## 準備

Windowsの場合

  - cygwin (x64版Windowsでは、64bit版でも可)
  - make (cygwin版)
  - perl (windows版、cygwin版のどちらでも可)
  - Visual C++ 2015 またはそれ以降 ( Visual C++ 2017 の最新Updateを推奨 )
  - Windows SDK ( Visual Studio 付属のものを推奨 )
  - [ROOT](ROOT "wikilink") ( 32bit版のビルドのみ )
  - boostライブラリ 1.61.0以降 ( 1.65.0以降推奨 )
  - jemallocライブラリ 5.0.0以降 ( 5.1.0以降推奨 )

あらかじめ、cygwin の bin ディレクトリへのパスを通しておいてください。

cygwin のバージョンが混ざらないように注意

## ビルド手順

Windows で Visual C++ 2017 を用いて64bitでビルドする場合

1.  boost, jemalloc ライブラリのファイル群を Z:\\usr からローカルにコピー
2.  config\\local ディレクトリに移動
3.  ファイル header-win-msvc-x64-15-build.mak を以下の内容で作成する

`WITH_BOOST = boostのコピー先パス`  
`WITH_JEMALLOC = jemallocのコピー先パス`

1.  "perl ../objdir.pl win-msvc-x64-15-build" を実行
2.  obj\\win-msvc-x64-15-build ディレクトリが生成されるので、そこに移動
3.  make を実行して、正常終了を確認
4.  "make install" を実行すると、obj\\win-msvc-x64-15-build\\bin に実行ファイルがコピーされる

他の場合は、win-msvc-x64-15 を対応するターゲット名に置き換えてください。

## ダウンロード

ソースコードの管理には[Mercurialを使用いています](Mercurial "wikilink")。
Windowsでは、cygwin版は推奨しません。

### Mercurialリポジトリからのダウンロード

ビルドツリーを置きたいディレクトリに中身がある場合は、あらかじめ削除しておいてください。

    hg clone http://192.168.0.129/repos/hg/netscan ディレクトリのパス [-u ブランチ名]

これで指定されたパス以下にビルドツリーができます。ディレクトリが無い場合は、新しく作られます。

### ユーザーの認証

書き込み(push)時のみ認証をかけています。 読み込み(ダウンロード)には認証をかけていません。

## ターゲット名

ターゲット名の解説です。

### プラットフォーム別のデフォルトターゲット名

  - Windows 32bit用
      - ~~win-msvc-12 - Visual Studio 2013 向け~~ 対応打ち切り
      - win-msvc-14 - Visual Studio 2015 ( Update 3 以降 ) 向け
      - win-msvc-15 - Visual Studio 2017 ( Update 7 以降 ) 向け ( boost
        1.65.0以降 )
  - Windows 64bit(x64)用
      - ~~win-msvc-x64-12 - Visual Studio 2013 向け~~ 対応打ち切り
      - win-msvc-x64-14 - Visual Studio 2015 ( Update 3 以降 ) 向け
      - win-msvc-x64-15 - Visual Studio 2017 ( Update 7 以降 ) 向け ( boost
        1.65.0以降 )
      - win-msvc-x64-16 - Visual Studio 2019 向け (予約済み)
  - GNU/Linux x86\_64用
      - linux-gcc-centos7dts7 - CentOS 7 devtoolset-7 向け
  - HPC用
      - hpc-fcc - fcc 向け
      - hpc-gcc - gcc 向け

### 特定の目的に予約されたターゲット名

上記のデフォルトターゲット名を増やした場合に、名が被らないようにするための措置です。

'\*'は0文字または英数字1文字、'\*\*'は任意の長さの'-'または英数字、を表します。

  - \*\*-example\* - サンプルまたは解説専用ターゲット名
  - \*\*-build\*\* - ローカルでのビルド用ターゲット名
  - \*\*-local-\*\* - ローカルでのビルド用ターゲット名

## ビルドツリー生成用スクリプト

ビルドツリー生成用スクリプト objdir.pl の解説。

### 使用方法

    objdir.pl [ターゲット名 [ターゲット名2 ...] ]

実行すると最初に、info.txtをカレントディレクトリから親ディレクトリを順にたどって探しに行きます。 見つからないとエラーになります。

### 動作

すでに作成してある場合、上書きするとき以外は何もしません。

1.  obj ディレクトリを作成。
2.  obj\\common.mak を作成。
3.  設定ごとの作業。
    1.  obj\\*ターゲット名* ディレクトリを作成。
    2.  obj\\*ターゲット名* に include lib bin ディレクトリを作成。
    3.  obj\\*ターゲット名* に Makefile、header.mak、footer.mak を作成または上書き。
    4.  obj\\*ターゲット名* に src ディレクトリの各サブディレクトリと同名のディレクトリを作成し、それぞれの中に
        Makefile を作成または上書き。

### ファイル生成

ファイルの生成方法です。

  - obj\\common.mak - config\\common.mak から生成。
  - obj\\*ターゲット名*\\Makefile - config\\make.mak からコピー。
  - obj\\*ターゲット名*\\*ディレクトリ名*\\Makefile - src\\*ディレクトリ名*\\make.mak からコピー。
  - obj\\*ターゲット名*\\header.mak - config\\header\*.mak
    config\\local\\header\*.mak から生成。
      - 例えば、ターゲット名が win-msvc-8 の場合、以下の(存在しない場合は無視)ファイルを順に連結して生成する。
        1.  header.mak
        2.  header-win.mak
        3.  header-win-msvc.mak
        4.  header-win-msvc-8.mak
      - 出力メッセージ「creating 'header.mak'」の直後に、各生成元ファイルの情報が順に表示されます。
          - o - config ディレクトリにファイルが存在し、正常に完了。
          - u - config\\local ディレクトリにファイルが存在し、正常に完了。
          - . - ファイルが存在せず、無視。
          - x - ファイルの処理中にエラー発生。
  - obj\\*ターゲット名*\\footer.mak - config\\footer\*.mak から生成。生成方法などは
    header.mak と同様。

## カスタマイズ

header.mak のカスタマイズ方法の解説です。

### カスタマイズ方法

既存の設定の一部を変更する場合。

1.  新しいターゲット名を「*基となるターゲット名*-*追加文字列*」とします。
2.  config\\header-*新しいターゲット名*.mak を作成し、編集します。
      - 書式は一般のMakefileと同じです。
3.  「objdir.pl *新しいターゲット名*」 を実行すると、obj\\*新しいターゲット名*
    以下に新しい設定を基にしたディレクトリツリーが生成されます。
      - obj\\*新しいターゲット名*\\header.mak
        の生成時に、config\\header-*新しいターゲット名*.mak
        が使用されます。

### 内部変数

header.mak の内部で使われる変数の解説です。 環境によってはデフォルト値から変える必要があるかもしれません。

ここに載っていないものは、できるだけ変えないでください。 また、ディレクトリ名に空白文字は使用できません。

  - コンパイラ等
      - CXX - C++コンパイラの名前。Linuxではフルパスを使えます。
      - MT - マニフェストツールの名前。Windows専用。使わないようにするには、この値を /bin/true に変えてください。
  - [MPI関係](MPI "wikilink")
      - WITH\_MPI - MPIのあるディレクトリ。空にすると、MPIを使わずにビルドし、MPI必須のバイナリをビルドしません。
      - WITH\_MPI\_LIBNAME - MPI関係で使用するライブラリ名(複数可)
  - [ROOT関係](ROOT "wikilink")
      - ROOTSYS - ROOTのあるディレクトリ。空にすると、ROOTを使わずにビルドし、ROOT必須のバイナリをビルドしません。
      - WITH\_ROOT\_LIBNAME - ROOT関係で使用するライブラリ名(複数可)
  - boost関係
      - WITH\_BOOST - boostのあるディレクトリ
      - WITH\_BOOST\_LIBDIR - boostのライブラリのあるディレクトリ。Z:\\usr
        以下にあるデフォルト設定で使用するboostのファイル群をコピーして使用するなら、この値の変更は不要です。
  - jemalloc関係
      - WITH\_JEMALLOC - jemallocのあるディレクトリ
      - WITH\_JEMALLOC\_LIBNAME - ROOT関係で使用するライブラリ名(複数可)

## Makefileの特別なターゲット

obj\\*ターゲット名*\\Makefile に書かれている、特別なターゲットです。

### リビジョン情報ファイルの生成

    make revision

リビジョン情報ファイル revision.txt を生成します。 make から mercurial を呼んで生成しているので、'make
all' では生成しません。

生成に失敗した場合は、リビジョン情報ファイルが存在しない状態にします。

## 不具合

  - ~~make を -j オプションで並列に実行すると、予期しないエラーが起こる。make を再実行すると、再現しない。~~ 修正済み。

[Category:netscan](Category:netscan "wikilink")
