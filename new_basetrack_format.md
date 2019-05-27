### New basetrack file format

最低限必要な要素
* RawID機能
  - HTS1号機の場合は、row col zone rawIDを含む
  - row colはHTSの視野数(shot)単位
  - rawIDは32bit int
* データの容量は最小限
  -不要なメンバーは省略できる
  -ハッシュテーブルを使って圧縮できる
* Alignmentの性能を落とさない
* 場所を指定したランダムアクセス機能

あれば良い機能
* C++ nativeの機能だけで読めること
  - pythonでも読める
  - ROOTからも読める
* どの[correction-map](correction-map.md)を用いて接続したかを取得できる
* ベーストラックの基準面を再利用可能なように定義する
