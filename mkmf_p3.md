---
#### mkmf_p3
---

+ description : convert chain to m-file  
+ usage : mkmf_p3 *event-descriptor* *chain.dat* [options]
+ options
  - --geom geom
  > set geometry ( default is 0 ).  

  - --zone zone
  > set zone for all tracks in chain.dat ( default is 0 ).  
  > This option should be temporal to pass zone information from linklet which is not treated properly in l2c.  

  - --buffer-size size  
  > set buffer-size, which is a number of tracks ( default is 1000000 ).  
