---
#### classes used in binary dump ( dump_fvxx, dump_bvxx, dump_linklet )
---

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
	micro_track_t m[4];
};

```
  > They are defined in namespace netscan.  
  > Constructores are now show in code below.  
