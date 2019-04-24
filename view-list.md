---
#### view list for processing
---

+ description
  > This is a process-view-list which can be given as an argument for option --view in core-1 programs.  
  > One can use [[[mk_views](mk_views.md)|[mk_views](mk_views.md)]] to create for each data-set.  

+ view-list-file
  ```
  column  description
  01      id
  02      ix x-index 0 ...  
  03      iy y-index 0 ...  
  04-07   xmin, xmax, ymin, ymax  
          This defines an area on the second pos and is normally an outer area of the view,  
          which is overlapped with neigbouring views.  
  08-11   xmin_i, xmax_i, ymin_i, ymax_i  
          This defines an area on the first pos and is normally an inner area of the view,  
          which is NOT overlapped with neigbouring views.  
  12      fname_rc runcard file name ( use '-' not to specify runcard file )  
  ```
