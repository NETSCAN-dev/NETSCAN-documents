---
#### **Required tools to build NETSCAN programs** ( updated 2018-11-01 )
---

+ visual c/c++ 2017
+ gnu make ( included in cygwin )
+ perl ( included in cygwin )
+ mercurial ( https://www.mercurial-scm.org/wiki/Download )
+ boost 1.65.0
+ jemalloc 5.1.0

  > Boost and jemalloc are also required to run NETSCAN applications.  
  > They are better to be installed like in Z:, because their path are hard-coded in Makefile settings.  
  > Minimum set of 64bit cygwin package is avaliable [here](cygwin.zip).  

---
#### **Build steps for NETSCAN software**
---

+ Prepare youre-repository and your-working-copy from the master-repository.
  - mkdir your-repository
  - cd your-repository
  - hg clone --branch ver-2016-09-01 http://192.168.0.129/repos/hg/netscan . ( do not forget last '.' which means current folder )  
  - mkdir obj
  - perl ..\config\objdir.pl win-msvc-x64-15


+ Build NETSCAN application.
  - cd your-repositoryobj\win-msvc-x64-15
  - make clean
  - make all
  - make install
  - make clean-obj

  > do not specify multiple targets in using make.  

+ Import all changes uploaded to the master repository to your-repository and your-working-copy.
  - cd your-repository
  - hg pull
  - hg update


+ Commit changes applied to your-working-copy to your-repository and then export to the master-repository.
  - hg status
  - hg commit -m "commit-message" files-to-be-committed
  - hg push

  > You need your account to push your changes to the master-repository.  
