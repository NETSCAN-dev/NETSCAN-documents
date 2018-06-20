---
#### netscanのシステム要件 ( 2018年04月更新 )
---
```
OS:
  必須: Windows 7 SP1/8以降 x64, Windows Server 2008 R2 SP1以降 x64
  推奨: Windows 7 SP1 x64, 8.1 x64, 10 x64 SAC
    注1: OSが、そのハードウェアに対応している必要があります
    注2: サポート期間が終了したOSの使用は推奨しません
    注3: Home版の使用は推奨しません
    注4: Windows10 SAC(Semi-Annual Channel)のバージョンは、 Windows Update で配信される、最新もしくは最新の1つ前のものを推奨します。

CPU ( ver-2011-03-01, ver-2016-09-01互換性重視ビルド ):
  必須: Intel Core i3/i5/i7 第2世代以降,
        Intel Xeon E3/E5,
        Intel Xeon E7 第2世代以降,
        AMD FX-4xxx/6xxx/7xxx/8xxx/9xxx,
        AMD Ryzen, AMD APYC,
        その他x86_64,AVXに対応したCPU/APU
  推奨: デスクトップ向け Intel Core i5/i7 第4世代以降,
        Haswell-E (Core i7-5xxx X-series),
        Broadwell-E (Core i7-6xxx X-series),
        Skylake-X (Core i7-78xx X-series, Core i9-79xx),
        Intel Xeon E3/E5 第3世代以降,
        Intel Xeon Bronze/Silver/Gold

CPU (ver-2016-09-01速度重視ビルド, ver-2018-05-01 ):
  必須: Intel Core i3/i5/i7 第3世代以降,
        Intel Xeon E3/E5/E7 第3世代以降,
        AMD Ryzen, AMD APYC,
        その他 x86_64,AVX2,FMA3,BMI2 に対応したCPU/APU
  推奨: デスクトップ向け Intel Core i5/i7 第4世代以降,
        Haswell-E (Core i7-5xxx X-series),
        Broadwell-E (Core i7-6xxx X-series),
        Skylake-X (Core i7-78xx X-series, Core i9-79xx),
        Intel Xeon E3/E5 第3世代以降,
        Intel Xeon Bronze/Silver/Gold

メモリー:
  必須: 16GB (通常時), 4GB (省メモリ動作時)
  推奨: 32GB以上 かつ 物理CPUコア数×8GB以上
    注: on memory で処理するには更に多く必要になります

作業用ストレージ:
  必須: 無し(出力先と共有)
  推奨: HDD 2物理CPUコアあたり1台、または、
        SSD 物理CPUコアあたり読み込み60MB/s以上,書き込み30MB/s以上
```

+ CPU 世代判別
  > Corei#-N* ( N が世代番号 )  
