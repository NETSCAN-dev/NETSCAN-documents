---
#### **Required tools to build NETSCAN programs**
---

+ visual c/c++ 2015
+ gnu make ( included in cygwin )
+ perl ( included in cygwin )
+ mercurial ( https://www.mercurial-scm.org/wiki/Download )
+ boost 1.59.0 as described above
+ jemalloc as described above

  > Boost and jemalloc are also required to run NETSCAN applications.  
  > They are better to be installed like in Z:, because their path are hard-coded in Makefile settings.  
  > Minimum set of 64bit cygwin package is avaliable [here](cygwin.zip).  

---
#### **Build steps for NETSCAN software**
---

+ Prepare youre-repository and your-working-copy from the master-repository.
  - mkdir your-repository
  - cd your-repository
  - hg clone --branch ver-2016-09-01 http://192.168.0.129/repos/hg/netscan .
  - mkdir obj
  - perl ..\config\objdir.pl win-msvc-x64-12


+ Build NETSCAN application.
  - cd your-repositoryobj\win-msvc-x64-12
  - make update
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
