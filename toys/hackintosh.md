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

## BIOS 选项设置
> 网上有很多教程，而且也列举了相关的选项，但我在我的主板中只找到一些，还有一些没有找到，罗列出来
 - 我的主板设置

Option|Value
-|-
Extreme Memory Profile (X.M.P.)|**Profile 1**
Windows 8/10 Features|**Other OS**
CSM Support|**Disable**
Initial Display Output|**PCIe Slot 1**
Intel Platform Trust Technology (PTT)|**Disabled**
Legacy USB Support|**Enabled**
XHCI Hand-off|**Enabled**
Network Stack|**Disabled**
Vt-d|**Disabled**
Internal Graphics|**Enabled**
Audio Controller|**Enabled**
Above 4G Decoding|**Enabled**
ErP|**Disabled**
RC6 (Render Standby)|**Enabled**

 - 未存在的设置项（没有找到）

Option|Value
-|-
TBT Vt-d Base Security|**Disabled**
Thunderbolt Boot Support |**Disabled**
Security Level |**No Security**
DVMT Pre-Alloc|**64M**
DVMT Total Gfx Mem|**256M**

## 引导
> 基于 Clover 的引导方式， OpenCore 引导方式看上去有点难度，对于急于使用的我来说，还不太现实，先通过 clover 的方式正常使用后，后面再来探索 OpenCore 的引导方式。

## 系统安装
 1. 在 AppStore 上下好最新版的操作系统
 2. 使用 tonymacx86.com 的 UniBeast 工具制作安装盘
 3. 按一般安装操作系统的流程完成安装
 4. 完成安装后需要使用 Clover Configuration 工具将安装盘的 EFI 拷贝到本机的 EFI 分区
 5. 完成安装，再根据硬件驱动有所异常情况来进行调试

## 驱动安装以及调试

### ps2 键盘支持
  > ps2 键盘和鼠标的支持，需要 VoodooPS2Controller.kext 这个驱动，只需要将该驱动放置到 EFI/CLOVER/kexts/Other 下面即可。

### BIOS 支持 cfg（苹果原生电源管理支持）
 - **什么是 cfg lock**
 > 以往在构建黑苹果主机的时候，使用者大都选择技嘉主机板作为黑苹果的最佳选择，其原因是所谓的『支持原生电源管理』。
CFG Lock：关闭或开启MSR 0xe2寄存器，CPU的P-State C-State电源管理是放在MSR寄存器，当你用黑苹果的时候就必须解锁这个选项。国内主机板制造商，只有技嘉打开了这个寄存器，可以直接安装而不需要难搞电源补丁（NullCPU或者二进制改文件）。近年来MSI、华擎主机板在BIOS已加入该选项。
随着黑苹果修改的技术面提升，已有了各种自动化的安装方法，包括给原版BIOS打补丁的PMPatch/UEFIPatch，以及Clover的自动补丁，降低了构建黑苹果主机的难度。只要检查主机板的BIOS里面是否有“ CFG LOCK ”这个选项，如果有的话，就把它关掉它。如同技嘉主机板一样了，可以直接用原生电源管理。

 - **如何打开 cfg lock**
 > "CFG Lock"，其说明内容为关闭或者开启MSR 0xe2，当需要使用黑苹果时，必须解锁 MSR 0xe2，否则无法使用原生电源管理。

参考文档: [https://blog.xjn819.com/?p=317](https://blog.xjn819.com/?p=317)

### Intel HD630 集显与 Rx590独显双解
> 由于核显驱动后，在我的主板上会出现显示接口乱序的问题，直接导致的问题就是正常驱动后，主板上的接口无法正常输出到显示器上。so，又买了一张独立显卡，在独立显卡输出的基础上，将核显成功驱动，并打开了 framebuff 帧缓存，现在记录一下。

 - 先在 clover configurator 这个工具上，将核显驱动起来。
> 通过 ID 注入的方式，让系统自动识别核显并驱动，做到这步，已经把核显以及独显全部驱动起来了。

options|value
-|-
Devices -> Fake ID -> IntelGFX|**0x3E9B8086**
Graphics|**勾选 Inject Intel**
Graphics -> ig-platform-id|**0x3E9B0007**

### framebuff 缓存帧打开
> 需要 hackintool 这个工具的支持，只有在核显打开的基础上，才能进行下面的操作

参考文档：[https://blog.daliansky.net/Tutorial-Using-Hackintool-to-open](https://blog.daliansky.net/Tutorial-Using-Hackintool-to-open-the-correct-pose-of-the-8th-generation-core-display-HDMI-or-DVI-output.html)

 - 打开 **hackintool** 这个工具
 - **在顶部菜单栏**的 framebuff 这个选项中，勾选选项里的 **">= macOS 10.14"**
 - **在顶部菜单栏**的 Patch 这个选项中，选择 **System Configs -> Gigabyte -> Z390M Gaming**
 - 再点击在 hackintool 工具界面，点击 patch 选项
 - **patch 选项中基础设置勾选**

options|value
-|-
DeviceProperties|**勾选**
Connectors|**勾选**
VRAM|**勾选**
Graphic Device|**勾选**
Auto Detect Changes|**勾选**

 - **patch 选项中高级设置勾选**

options|value
-|-
Hotplug Reboot Fix|**勾选**
Spoof Video Device ID|**0x3E9B**
VRAM 2048MB|**勾选**
Enable HDMI20(4k)|**勾选**
Inject Fake IGPU|**勾选**

 - 点击 Generate Patch 来生成相对应的补丁信息
 - **在顶部菜单栏**的 File 这个选项中，选择 **Export -> Bootloader config.plist** 将生成的补丁信息注入到 config.plist 文件中

### AppleALC 音频支持

 - **确定主板的声卡型号**，查找主板的参数描述，确定自己主板板载声卡的具体型号
 - 到[AppleALC支持的声卡型号](https://github.com/acidanthera/AppleALC/wiki/Supported-codecs)上查找自己主板的型号支持。
 - 打开 clover configurator 选择 **Devices** 选项，**勾选 reset HDA**, 填入**注入 ID**值， 具体的值可以参考刚才查找的列表上的 layout 值来进行尝试。

### AppleALC 可能造成休眠重启的问题
 > 目前的 applealc 可能对 10.15.× 的支持不是那么好，休眠或者关机过程中，可能由于音频崩溃会造成系统崩溃，从而造成重启

```shell
# 打开 Clover Configuration 在 “kernel and kext patches” 选项的 “KernelToPatch”选项里添加相对应的值
find: 63 6F 6D 2E 61 70 70 6C 65 00 5F 5F 6B 65 72 6E 65 6C 5F 5F 00
replace: 6E 6F 74 2E 61 70 70 6C 65 00 5F 5F 6B 65 72 6E 65 6C 5F 5F 00
```

### Power Schedule 可能唤醒电脑
 > 升级到 10.15 后，电源计划包含了 “提醒” 的闹钟内容，这可能就造成休眠的过程中，电脑可能会被唤醒。

```shell
# 使用 pmset 命令取消所有可以出发唤醒的电源计划
sudo pmset schedule cancelall
```

 - **USB 定制**
 > 网上查询到黑苹果的 usb 有端口数量限定，虽然有相关不定，但是对休眠会有影响，如休眠过程中移动鼠标，可能造成唤醒，通过 usb 定制，将鼠标和键盘端口设定为内置，就可以解决相关问题

 参考文档：[https://post.m.smzdm.com/p/a5k6q06x/](https://post.m.smzdm.com/p/a5k6q06x/)

 - **z390 原生 nvram 支持**

 > 基于 opencore 的 SSDT-PMC.dsl，使用 maciasl 编译后放入 /EFI/CLOVER/ACPI/patched 当中即可

 - **关于OsxAptioFix2Drv-free2000.efi**


## 一些错误的补救方法

 - 手上留有一个 EFI 引导的 U 盘，用来解决各种因为修改导致的系统出错。
> 如果是因为 clover configurator 或者 hackintool 这个工具对系统设置或者补丁的修改，造成系统无法启动的情况，可以使用具备 EFI 引导功能的 U 盘，也就是说，这个 U 盘可以是你的安装盘，因为安装盘始终是可以进入安装的，此时，只需要让电脑使用 U 盘上的 EFI 来引导硬盘上的系统即可，只要能进系统，这些错误的设置都能拯救回来。

 - 10.15.4 升级后卡 "++++++++++++++++++++++++"， 这是因为 clover 的版本太低了导致的，只需要更新 efi 中的引导文件版本即可。
 

## 相关链接

 - [Opencore](https://github.com/acidanthera/OpenCorePkg)
 - [MaciASL](https://github.com/acidanthera/MaciASL)
 - [VoodooPS2](https://github.com/acidanthera/VoodooPS2)
 - [IntelMausi](https://github.com/acidanthera/IntelMausi)
 - [VirtualSMC](https://github.com/acidanthera/VirtualSMC)
 - [Lilu](https://github.com/acidanthera/Lilu)
 - [AppleALC](https://github.com/acidanthera/AppleALC)
 - [WhateverGreen](https://github.com/acidanthera/WhateverGreen)
