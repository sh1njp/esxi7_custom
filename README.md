# ESXi7.0u2で トランセンド TS1TMTE220Sを使う。

# 概要
標準のESXi7.0U2では、トランセンド TS1TMTE220S を認識しないため、
カスタマイズISOイメージを作成する。

# 方法
Windows10環境で、「ESXi-Customizer-PS」にコミュニティ版のNVMEドライバを組み込みISOイメージ作成

# VMWare PowerCLIの導入

管理者権限でPowerShellを起動
```
Install-Module -Name VMware.PowerCLI -AllowClobber
```

→ エラー出る場合は、PowerShell実行環境のセキュリティを緩和する。

# ESXiカスタマイザー(V2.8.1)のダウンロード

以下URLにアクセス
https://github.com/VFrontDe/ESXi-Customizer-PS

以下ファイルをダウンロード
https://raw.githubusercontent.com/VFrontDe/ESXi-Customizer-PS/master/ESXi-Customizer-PS.ps1
2021年7月26日時点で、Version2.8.1でした。

ファイルA：ESXi-Customizer-PS.ps1


# コミュニティ版のNVMEドライバのダウンロード
https://flings.vmware.com/community-nvme-driver-for-esxi

ファイルB : nvme-community-driver_1.0.1.0-2vmw.700.1.0.15843807-component-18290856.zip


# ディレクトリ C:esxi7-custom\pkg を作成、ファイルを格納。

ファイルA,Bを上記ディレクトリに格納。

```
PS C:\esxi7-custom\pkg> DIR
ディレクトリ: F:\esxi7-custom\pkg

Mode            LastWriteTime   Length Name
----            -------------   ------ ----
-a----    7/25/2021   6:14 PM    22154 ESXi-Customizer-PS.ps1
-a----    7/25/2021   5:59 PM    98340 nvme-community-driver_1.0.1.0-2vmw.700.1.0.15843807-component-18290856.zip

PS C:\esxi7-custom\pkg>
```


# パッケージ格納ディレクトリを指定して、カスタマイズ版のISOファイルを作成
```
PS C:esxi7-custom\pkg> .\ESXi-Customizer-PS.ps1 -v70 -pkgDir C:\esxi7-custom\pkg -NSC
```
実行結果・・・。
```
This is ESXi-Customizer-PS Version 2.8.1 (visit https://ESXi-Customizer-PS.v-front.de for more information!)
(Call with -help for instructions)

Logging to C:\Users\netadmin\AppData\Local\Temp\ESXi-Customizer-PS-320.log ...

Running with PowerShell version 5.1 and VMware PowerCLI version .. build

Connecting the VMware ESXi Software depot ... [OK]

Getting Imageprofiles, please wait ... [OK]

Using Imageprofile ESXi-7.0U2a-17867351-standard ...
(Dated 04/29/2021 00:00:00, AcceptanceLevel: PartnerSupported,
The general availability release of VMware ESXi Server 7.0U2a brings whole new levels of virtualization performance to datacenters and enterprises.)

Loading Offline bundles and VIB files from C:esxi7-custom\pkg ...
   Loading C:esxi7-custom\pkg\nvme-community-driver_1.0.1.0-2vmw.700.1.0.15843807-component-18290856.zip ... [OK]
      Add VIB nvme-community 1.0.1.0-2vmw.700.1.0.15843807 [OK, added]

Exporting the Imageprofile to 'C:esxi7-custom\pkg\ESXi-7.0U2a-17867351-standard-customized.iso'. Please be patient ...

All done.
```


# 以下ファイルが作成された。

ESXi-7.0U2a-17867351-standard-customized.iso

