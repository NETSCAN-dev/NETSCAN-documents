---
#### mk_views
---

+ description : create [[process-view-list|view-list]]  
+ usage : mk_views *pos0* *pos1* [*options*]  
> specify same pos for pos0 and pos1 to make a list for one plate  

+ options ( those are in **bold** must be given )
  - **--descriptor [event-descriptor](event-descriptor)**
  > set [event-descriptor](event-descriptor)  

  - --io fname-io
  > override [event-descriptor](event-descriptor) entries  

  - **--view view-step view-overlap**
  > set view step and overlap by value  
  > - view-size = view-step + view-overlap x 2 and it steps by view-step.  
  > - out-most areas might be enlarged to include remainng edges of area to be processed.  

  - --o fname-view
  > set output view-list filename  

  - --rc fname-rc
  > set runcard filename in view-list file  

  - --sort 0/1
  > sort views by line(0) or by spiral-around-center(1). default = 0  
