---
#### FAQ
---

+ micro-track, base-track の xyz の定義  

  - micro_track_t の xy は (z1+z2)/2 での値 ( vxx 作成時に保証されるべき )。  
  > micro-track-vxx ファイルでは micro_track_t::z = 0 としてある。  

  - base-track の xyz は z = m[0].z1 での値 ( i.e. face=1 の micro_track_subset_t の xy )  
    micro_track_subset の xyz は
    - face=1 の場合 z2 に投影 ( 式中の m は face=1 の micro_track_t )  
		x = m.x + m.ax x (m.z2-m.z1)/2
		y = m.y + m.ay x (m.z2-m.z1)/2
		z = m.z2
    -  face=2 の場合 z1 に投影 ( 式中の m は face=2 の micro_track_t )  
		x = m.x + m.ax x (m.z1-m.z2)/2
		y = m.y + m.ay x (m.z1-m.z2)/2
		z = m.z1
   > 但し、 base-track-vxx には micro_track_subset_t の xy は書込んでいないので  
   > dump_bvxx で出力する micro_track_subset_t の xy は base_track_t のメンバから再計算  

+ b_filter の xy 座標での filter には base_track_t::x,y を使用している。  
