# NAS组建操作

## 购买硬件
 - 暴风酷播云
   > cpu + 主板：华擎 j3455-itx
   > 硬盘：自带 16G 万由 ssd（无机械硬盘）
   > 内存：金士顿 8G DDR3 1600
   > 电源：1u 150w
   > 机箱：万由代工, 前置 usb 3.0接口
   > 风扇：机箱风扇（Gelid silent 8）, 电源风扇
 - 机械硬盘 西数蓝盘 4t
 - u盘 闪迪酷豆 16G usb2.0 (用来作为引导启动)

## Mac地址收集和计算
 - mac地址收集

## 主板升级
 - doc刷mac教程：(资源百度网盘)
    > 需要制作一个 HP 的 dos 启动盘，据说是华擎官方推荐的 dos 工具
    > 制作完 dos 启动盘后，需要将 mac 命令文件放到 u 盘中，这样在 dos 环境下才能使用
```shell
http://www.u-share.cn/forum.php?mod=viewthread&tid=23418&extra=&page=1
```
 - 升级 bios
    > 1. 如果开机进入 bios 后有密码，需拆机拆主板电池清除 bios 密码
    > 2. 查看 bios 的版本号，然后根据官网的升级教程来升级，注意版本号之间的升级顺序
    > 3. 下载相对应的 bios 文件解压缩后将文件拷贝至 u 盘，插入主板进入 bios 进行选择升级
 - bios 下载地址
    > 需要注意，如果原来的版本小于 1.60，需要先升级 1.70这个版本
    > 目测 1.60 这个版本可能有问题，官方页面已经没有
 ```shell
http://www.asrock.com/mb/Intel/J3455-ITX/index.cn.asp?cat=Download&os=All
 ```

## 系统安装
 - 系统版本选择
    > 1. j3455 主板对应 群晖 ds918+, 所以系统和引导的版本选择相对应机型的
    > 2. 本地的安装 loader 选用的版本是 1.04b
    > 3. 系统选用 6.2.1
 - 启动盘制作（需要用 u 盘来做引导，这是最方便的操作）
    > 制作启动盘时还要注意修改 grub.cfg 引导文件， 相关的操作参考教程
```shell
https://www.nas2x.com/threads/dsm-6-2-1-20190221.29/
```

 - 启动盘固件下载
```shell
https://xpenology.com/forum/topic/12952-dsm-62-loader/
```

 - 安装系统
    > 有一点需要注意，引导和系统固件的版本要匹配得上才行
```shell
https://post.m.smzdm.com/p/and2km63/
```

 - 系统镜像下载(官方镜像地址)
```shell
https://archive.synology.com/download/DSM/release/
```
## 硬件改装
 - 思路
     >1u 电源的风扇噪音太大，另外机箱风扇的质量也不怎么样，先把电源全部更换了，再看看机箱风扇的效果；如果机箱风尚不给力，则把机箱风扇给更换了。
 - 功耗分析
     > 根据网络上对 j3455 黑群晖功耗的实测，实际不会超过 60w， 所以按照 60w的功耗标准来进行电源采购即可。

## 后续硬件预采购
 - 改 DC 电源（笔记本电源）
     > DC-ATX 直插模块（预计60-70元）
     > 电源适配器（预计60-70元）
     > 1u 电源转 DC 插口挡板（预计20+元）

## 实际使用情况
 - 机箱风扇为 8cm 风扇
 - 使用 bios 将机箱风扇的挡速调至最小，声音在正常运行时几乎听不到
 - 电源在正常使用过程中，风扇的声音也可以忽略（不排除日后长时间使用声音会逐渐变大）
 - 电源长时间运行过程中的发热还是有的，考虑下 150w 根本用不到，考虑风扇声音变大后再改 dc 模块

## 目前还存在的缺陷
 - 不能关机和重启(看引导后面还会不会升级)