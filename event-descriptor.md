---
#### event descriptor
---

各pathは、event descriptorを置いたdirectoryからの相対path または 絶対pathで指定して下さい。プログラムを実行しているcurrent directoryからの相対pathではありません。

#### eventdescriptor example
  ```
  #
  # RunCard の雛型です。このままでは使えません。
  #

  #
  # Chamber 構造の記述
  #
  [ChamberGeometry]
    PartsFile           = ..\st\parts.kar   # parts.kar を指定します。
    ChamberStructure 0  = ..\st\chamber.kar # geometry = 0 の chamber.kar を指定します。
    
  #
  # MicroTrack の vxx ファイルを指定します。
  # 左辺は pos zone です。
  # 右辺は file-name block-size です。通常は block-size = 1000000 として下さい。
  #
  [MicroTrackBlock]
    11 0 = ..\PL01\f0011.vxx 1000000
    12 0 = ..\PL01\f0012.vxx 1000000
    21 0 = ..\PL02\f0021.vxx 1000000
    22 0 = ..\PL02\f0022.vxx 1000000
    11 1 = ..\PL01\i0011.vxx 1000000
    12 1 = ..\PL01\i0012.vxx 1000000
    21 1 = ..\PL02\i0021.vxx 1000000
    22 1 = ..\PL02\i0022.vxx 1000000

  #
  # BaseTrack の vxx ファイルを指定します。
  # 左辺は pl zone です。
  # 右辺は file-name block-size です。通常は block-size = 1000000 として下さい。
  #
  [BaseTrackBlock]
    1 0 = ..\PL01\b001.vxx 1000000
    2 0 = ..\PL02\b002.vxx 1000000

  #
  # Linklet の vxx ファイルを指定します。
  # 左辺は pos0 pos1 geometry zone です。
  # 右辺は file-name block-size です。通常は block-size = 1000000 として下さい。
  #   pos0-pos1 対毎に異なる vxx を使える様にしてあります。
  #   通常は pos0 = MAKEPOS(pl0,0), pos1 = MAKEPOS(pl1,0) と pl0-pl1 対毎に vxx を指定して下さい。
  #   異なる pos0-pos1 対に同じ vxx file name を指定しても良い（はず）です。
  #   pos0,pos1 共に * とワイルドカード表記すると、１行の記述で全ての pos0-pos1 対に同じ vxx を指定できます。
  #
  [LinkletBlock]
    10 20 0 0 = ..\linklet\l-0.vxx 1000000

  #
  # 補正マップの vxx ファイルを指定します。
  # 左辺は pl geometry level (0,1,2,3) です。
  # 右辺は file-name block-size です。通常は block-size = 1000000 として下さい。
  # (*) この section は補正データの管理に correction-map-vxx を使う場合 ( scripts\2 ) でのみ指定して下さい。それ以外で指定すると動作が遅くなります。
  #
  [CorrectionMapBlock]
    1 0 0 = ..\PL01\correction-map-0-0.vxx 1000000
    1 0 1 = ..\PL01\correction-map-0-1.vxx 1000000
    1 0 2 = ..\PL01\correction-map-0-2.vxx 1000000
    1 0 3 = ..\PL01\correction-map-0-3.vxx 1000000
    2 0 0 = ..\PL02\correction-map-0-0.vxx 1000000
    2 0 1 = ..\PL02\correction-map-0-1.vxx 1000000
    2 0 2 = ..\PL02\correction-map-0-2.vxx 1000000
    2 0 3 = ..\PL02\correction-map-0-3.vxx 1000000

  [Parameters]
    FastIoMode = 1          # 通常はこのまま使用して下さい。
    RelativePathMode = 0    # 0 : relative to current folder of the process ( default )
                            # 1 : relative to event-descriptor's folder
    CorrectionMode = 0      # 0 : single affine, calculated by weighted avarage is ( default )
                            # 1 : closest correction-maps, possibly multiple affines, are used for a read-out area
    MaxZone = 9             # 使える zone の最大値を指定する。この場合は 0-9 を使える。未指定の場合は MaxZone = 9 となる。

  ```
