---
#### integ_corr
---

#### description
> #### Integrate relative correction map and output absolute correction map.  
>  
> **BE CAREFUL that ...**  
> When a given relative correction map file has multiple areas,  
> this code calculates SIGNLE affine parameter for each plate by fitting all affine parameters in one plate first,  
> and then integrates these SINGLE affine parameters to obtain absolute correction map.  

#### usage
> #### integ_corr *correction-map-rel(in)* *correction-map-abs(out)* [*options*]  

#### options ( those are in **bold** must be given )
- **--descriptor [event-descriptor](event-descriptor.md)**
> set [event-descriptor](event-descriptor.md)  

- --geom geom
> set geometry 0/1/...  

- --global 0/1
> enable(1) or disable(0/defaule) global correction to fix end-plates  

- --split 0/1
> 1: correction-map-abs with plate(0) or pos(1) entries  

- --fix pos
> fix specified pos  
