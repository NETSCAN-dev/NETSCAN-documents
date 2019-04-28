### New basetrack file format

最低限必要な要素
* RawID機能
  - HTS1号機の場合は、row col zone rawIDを含む
  - row colはHTSの視野数(shot)単位
  - rawIDは32bit int
* C++ nativeの機能だけで読めること
  - pythonでも読める
  - ROOTからも読める
* データの容量は最小限
  -不要なメンバーは省略できる
* Alignmentの性能を落とさない

あれば良い機能
* 場所を指定したランダムアクセス機能
