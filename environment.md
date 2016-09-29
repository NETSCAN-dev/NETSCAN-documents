---
#### environment setting to run netscan programs
---

+ boost 1.59.0
  - 32bit : add z:\usr\boost\boost_1_59_0\stage\lib32 to PATH
  - 64bit : add z:\usr\boost\boost_1_59_0\stage\lib64 to PATH
+ jemalloc ( Komatani patch is required )
  - 32bit : add z:\z:\usr\jemalloc\bin to PATH
  - 64bit : add z:\z:\usr\jemalloc\bin64 to PATH
+ root ( required for 32bit only )
  - set ROOTSYS=z:\usr\root\root_v5.34.32_vc10
  - add %ROOTSYS%\bin to PATH
+ appropreate version of visual c/c++ runtime

---
#### required tools to build netscan programs
---

+ visual c/c++ 2010 or 2015
+ gnu make ( included in cygwin )
+ perl ( included in cygwin )
+ svn version 1.8.8
+ boost 1.59.0 as described above
+ jemalloc as described above
  > boost and jemalloc are better to be installed as in Z:, since their path are hard-coded in Makefile settings.  
  > minimum set of 64bit cygwin package is avaliable [here](cygwin.zip).  
