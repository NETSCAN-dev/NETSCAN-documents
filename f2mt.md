---
#### f2mt
---

+ description : build micro track vxx file from text-file ( f-file, $e, $res )  
  > The f-file format and its minimal example is described [[here|f-file-format]].
 
+ usage : f2mt *input-file* *output-vxx-file* *pos* [*options*]
+ options
  - --append
  > append to vxx-file
  
  - --z z0
  > set z0 ( required for $res only )  
  
  - --page-size page-size
  > to be obsoleted ( internal parameter )  

```
Now f2mt re-assigns row-col internally, which might cause problem afterwards.  
```
