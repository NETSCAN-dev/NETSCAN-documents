---
#### netscan のシステム要件 2017年08月更新時点
---
```
OS:
  必須: Windows 7SP1/8以降 x64,
        Windows Server 2008 R2 SP1以降 x64
  推奨: Windows 7SP1 x64, 8.1 x64, 10 x64 CB/CBB
  注1: Home版の使用は推奨しません
  注2: Windows10 CBBのバージョンは、Windows Update で配信される最新のものを推奨します。

CPU ( ver-2011-03-01, ver-2016-09-01互換性重視ビルド ):
  必須: Intel Core i3/i5/i7 第2世代以降,
        Intel Xeon E3/E5,
        Intel Xeon E7 第2世代以降,
        AMD FX-4xxx/6xxx/7xxx/8xxx/9xxx,
        AMD Ryzen 3/5/7,
        その他x86_64,AVXに対応したCPU/APU
  推奨: デスクトップ向け Intel Core i5/i7 第4世代以降,
        Haswell-E (Core i7-58xx/59xx),
        Broadwell-E (Core i7-68xx/69xx),
        Intel Xeon E3/E5 第3世代以降

CPU ( ver-2016-09-01速度重視ビルド ):
  必須: Intel Core i3/i5/i7 第3世代以降,
        Intel Xeon E3/E5/E7 第3世代以降,
        AMD Ryzen 3/5/7,
        その他x86_64,AVX2,FMA3,BMI2に対応したCPU/APU
  推奨: デスクトップ向け Intel Core i5/i7 第4世代以降,
        Haswell-E (Core i7-58xx/59xx),
        Broadwell-E (Core i7-68xx/69xx),
        Intel Xeon E3/E5 第3世代以降

メモリー:
  必須: 16GB (通常時), 4GB (省メモリ動作時)
  推奨: 32GB 以上 かつ 物理CPUコア数×8GB以上
  注: on memory で処理するには更に多く必要になります

作業用ストレージ:
  必須: 無し(出力先と共有)
  推奨: HDD 2物理CPUコアあたり1台 または
        SSD 物理CPUコアあたり読み込み60MB/s以上,書き込み30MB/s以上
```

+ CPU 世代判別
  > Corei#-N* ( N が世代番号 )  
