# Hackintosh record

## 硬件配置
Component | Brank
-|-
CPU | Intel i7 8700
主板 | 技嘉 Z390M Gaming
内存 | 金士顿骇客神条 16G DDR4 3200 × 2
显卡 | 蓝宝石 RX590 超白金极光特别版
SSD | 三星 970evo 1T
电源 | 鑫谷 GP700P 白金
显示器 | 明基 PD2700u

## 引导
> 基于 Clover

## 系统安装
 1. 在 AppStore 上下好最新版的操作系统
 2. 使用 tonymacx86.com 的 UniBeast 工具制作安装盘
 3. 按一般安装操作系统的流程完成安装
 4. 完成安装后需要使用 Clover Configuration 工具将安装盘的 EFI 拷贝到本机的 EFI 分区
 5. 完成安装，再根据硬件驱动有所异常情况来进行调试

## 驱动安装以及调试

 - **ps2 键盘支持**

 - **BIOS 支持 cfg**
 > "CFG Lock"，其说明内容为关闭或者开启MSR 0xe2，当需要使用黑苹果时，必须解锁 MSR 0xe2，否则无法使用原生电源管理。

参考文档: [https://blog.xjn819.com/?p=317](https://blog.xjn819.com/?p=317)

 - **Intel HD630 集显与 Rx590独显双解**

 - **AppleALC 音频支持**

 - **AppleALC 可能造成休眠重启的问题**
 > 目前的 applealc 可能对 10.15.× 的支持不是那么好，休眠或者关机过程中，可能由于音频崩溃会造成系统崩溃，从而造成重启

```shell
# 打开 Clover Configuration 在 “kernel and kext patches” 选项的 “KernelToPatch”选项里添加相对应的值
find: 63 6F 6D 2E 61 70 70 6C 65 00 5F 5F 6B 65 72 6E 65 6C 5F 5F 00
replace: 6E 6F 74 2E 61 70 70 6C 65 00 5F 5F 6B 65 72 6E 65 6C 5F 5F 00
```

 - **USB 定制**
 > 网上查询到黑苹果的 usb 有端口数量限定，虽然有相关不定，但是对休眠会有影响，如休眠过程中移动鼠标，可能造成唤醒，通过 usb 定制，将鼠标和键盘端口设定为内置，就可以解决相关问题

 参考文档：[https://post.m.smzdm.com/p/a5k6q06x/](https://post.m.smzdm.com/p/a5k6q06x/)

 - **z390 原生 nvram 支持**

 > 基于 opencore 的 SSDT-PMC.dsl，使用 maciasl 编译后放入 /EFI/CLOVER/ACPI/patched 当中即可

 - **关于OsxAptioFix2Drv-free2000.efi**
 

## 相关链接

 - [Opencore](https://github.com/acidanthera/OpenCorePkg)
 - [MaciASL](https://github.com/acidanthera/MaciASL)
 - [VoodooPS2](https://github.com/acidanthera/VoodooPS2)
 - [IntelMausi](https://github.com/acidanthera/IntelMausi)
 - [VirtualSMC](https://github.com/acidanthera/VirtualSMC)
 - [Lilu](https://github.com/acidanthera/Lilu)
 - [AppleALC](https://github.com/acidanthera/AppleALC)
 - [WhateverGreen](https://github.com/acidanthera/WhateverGreen)
