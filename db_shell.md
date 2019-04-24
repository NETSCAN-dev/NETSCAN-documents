---
#### db-shell
---

+ description : data-base-like access tool for NETSCAN data  

+ start db_server process
  
  ```db_shell --descriptor eventdescriptor.ini --geom 0 --c correction-map-align.lst distortion.lst```

  - **--descriptor [event-descriptor](event-descriptor.md)**
  > set [event-descriptor](event-descriptor.md)  

  - --io fname-io
  > override [event-descriptor](event-descriptor.md) entries  

  - --geom geom
  > set geometry 0/1/... ( default 0 )  

  - --c correction-map-file [ correction-map-dist ]  
    --c - correction-map-dist
  > set relative or absolute correction-map and correction-map-dist.  
  > **relative correction-map is in effect when you specify plate pair by 'c' command**  

  - --dir event-data-folder
  > set current folder of db_server to work with an eventdescriptor written in relavie path.  

  - --server id
  > specify server id (0,1...) to distinguish multiple db_server processes. default is 0  

  - --reset
  > reset resources used to communicate with db_server process.  
  > use when db_server can not be started properly  

+ execute commands
  
  ```db_shell --server 0 [ command ]```

  - r m pos xmin xmax ymin ymax [phmin] [zone]  
    r m pos rawid[-rawid] [zone] [;]
  > read micro-tracks. see dump_fvxx for output format. 
  > second commands can be stacked and executed when terminated with ";".  

  - r b pl xmin xmax ymin ymax [phmin] [zone]  
    r b pl rawid[-rawid] [zone] [;]
  > read base-tracks. see dump_fvxx for output format.  
  > second commands can be stacked and executed when terminated with ";".  

  - r l pos0 pos1 xmin xmax ymin ymax
  > read linklet  

  - r pos zone xmin xmax ymin ymax
  > read view header info  

  - r g [pos [zone]]
  > read geometry info **without corrections**. all pos are listed without arguments  

  - r key ( key = pos col row zone isg )
  > read micro track using key. un-specified member of a key can be '!'  
  
  - r key0 key1
  > read base track using key pair  
  
  - r key0 key1 key2 key3
  > read linklet using keys  
  
  - r pos level col row
  > read correction map **when correction-map vxx is used**  
  
  - r c pl x y
  > get z coordinate with correction-map applied as shown below.  
  > $1 : plate number  
  > $2 : \# of surfaces ( now it should be 2 )  
  > $3 : z of face 0 ( i.e. base track )  
  > $4 : z of face 1 ( 1st face micro track )  
  > $5 : z of face 2 ( 2nd face micro track )  
  > $6 : z of face 1 film surface  
  > $7 : z of face 1 base surface  
  > $8 : z of face 2 base surface  
  > $9 : z of face 2 film surface  

  - r a pl x y is_corrected
  > get x,y corrdinates either not-corrected when is_corrected = 1, or corrected when is_corrected = 0  

  ---
  - c pl0[-zone0] pl1[-zone1] [fname_correction-map]

    > set correction pos1[-zone1] relative to pos0[-zone0] using relative correction-map either given as the 3rd argument or specified in --c argument  

  ---
  - f m pos max_tracks_per_view [ list_file_name +1/-1 ]
  > set micro-track filter  

  - f b pl  max_tracks_per_view [ list_file_name +1/-1 ]
  > set base-track filter

+ stop db_server process
  
  ```db_shell --server 0 exit```
