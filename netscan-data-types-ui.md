---
#### classes used in binary dump ( dump_fvxx, dump_bvxx, dump_linklet )
---

> They are defined in namespace netscan.  
> Constructores are not shown here.  

```c
class micro_track_t {
public:
	double ax,ay;
	double x,y,z;
	double z1,z2;
	float px,py;
	int ph,pos,col,row,zone,isg;
	int64_t rawid;
};

class micro_track_subset_t {
public:
	double ax,ay;
	double z;
	int ph;
	int pos,col,row,zone,isg;
	int64_t rawid;
};

class base_track_t {
public:
	double ax,ay;
	double x,y,z;
	int pl;
	int isg,zone;
	int64_t rawid;
	micro_track_subset_t m[2];
};

class linklet_t {
public:
	int pos[2];
	double zproj;
	double dx,dy;
	double xc,yc;
	base_track_t b[2];

	// Below member is commented-out ( i.e. de-activated ) by default action.  
	// To access them, you need un-comment and give it to dump_linklet like --format 0 your-own-netscan_data-types-ui.
//	micro_track_t m[4];
};

```
