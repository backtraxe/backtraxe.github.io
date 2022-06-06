# 黑苹果安装教程


<!--more-->

## 一、工具

- U盘（≥16G）
- Mac OS镜像（.dmg）
- **适合你的电脑的EFI文件**
- [balenaEtcher](https://www.balena.io/etcher/)（刻录工具）
- [DiskGenius](https://www.diskgenius.cn/)（分区工具）

## 二、制作U盘启动盘

1. 插入U盘，然后打开balenaEtcher软件。

<img src="/imgs/黑苹果安装教程/image-20201123194153284.png" alt="步骤1">

2. 点击`Select image`选择Mac OS镜像。

<img src="/imgs/黑苹果安装教程/image-20201123194619134.png" alt="步骤2">

3. 软件会自动识别出你的U盘，最后点击`Flash!`。

<img src="/imgs/黑苹果安装教程/image-20201123194643257.png" alt="步骤3">

4. 等待刻录完成（20min左右），之后会有一个完整性检测（15min左右）。

<img src="/imgs/黑苹果安装教程/image-20201123200153948.png" alt="步骤4">

5. 当软件显示`Flash Complete!`时表示刻录成功。

<img src="/imgs/黑苹果安装教程/image-20201123201524888.png" alt="步骤5">

## 三、配置Clover引导驱动

1. 打开DiskGenius，找到U盘上的`ESP`分区，删除`EFI`文件夹。
2. 把适合自己电脑`EFI`文件夹复制进去。（这里只能用快捷键复制粘贴）
3. 保存更改。

## 四、制作黑苹果系统盘

### 硬盘分区安装

压缩卷（≥25G）。

选中压缩出的空闲分区，右键`新建简单卷`，一直点击`下一步`但选择`不要格式化这个卷`。

### 整块硬盘安装

删除磁盘所有分区即可。

## 五、BIOS设置

以技嘉（Gigabyte）主板为例：

1. `BIOS`->`FastBoot`->`Disable`

2. `BIOS`->`Windows 8/10 Features`->`Windows 8/10`

   将操作系统类型设置为其他操作系统。从不支持Microsoft签名安全启动的第三方操作系统启动时，将“操作系统类型”设置为“其他操作系统”以获取优化的功能。

3. `BIOS`->`CSM Support`->`Disabled`

   禁用CSM。兼容性支持模块（CSM）是UEFI固件的组件，该组件通过模拟BIOS环境来提供旧版BIOS兼容性，从而允许仍使用旧版操作系统和某些不支持UEFI的选件ROM。Clover和OpenCore引导都支持UEFI引导。禁用CSM使BIOS可以轻松发现Bootloader。

4. `BIOS`->`LAN PXE Boot Option ROM`->`Disabled`

5. `BIOS`->`Storage Boot Option Control`->`UEFI`

6. `BIOS`->`Other PCI devices`->`UEFI`

7. `Peripherals`->`Initial Display Output`->`PCIe Slot`(独显)/`IGFX`(核显)

8. `Peripherals`->`Above 4G Decoding`->`Disabled`

9. `Peripherals`->`Trusted Computing`->`Security Device Support`->`Disable`

10. `Peripherals`->`USB Configuration`->`Legacy USB Support`->`Disabled`

    禁用旧版USB支持。

11. `Peripherals`->`USB Configuration`->`XHCI Hand-off`->`Enabled`

    启用XHCI切换。

12. `Peripherals`->`Network Stack Configuration`->`Disabled`

13. `Peripherals`->`SATA and RST Configuration`->`SATA Mode Selection`->`AHCI`

    将SATA设置为AHCI。通过高级主机控制器接口（AHCI）模式，可以在SATA驱动器上使用高级功能，例如热插拔和本机命令队列（NCQ）。AHCI还允许硬盘以比传统IDE模式更高的速度运行。

14. `Chipset`->`VT-d`->`Disabled`

    禁用VT-D。VT-d特别是IOMMU规范。扩展允许您访问虚拟机下的物理硬件（例如，运行Linux的系统可以在虚拟机上运行Windows。如果没有VT-d，则视频卡会被仿真，并且游戏速度会很慢。视频卡可以进入直通模式，并且可以在Windows下作为真实硬件（可以安装nvidia驱动程序）进行访问，并且视频卡的性能类似于运行本机Windows实时预览的情况。但是对于许多黑苹果用户，VT-D不会造成任何问题，但是如果您是新手，则尝试安装和配置Hackintosh禁用VT-D并安装。您可以在安装后根据需要启用VT-D。

15. `Chipset`->`IOAPIC 24-119 Entries`->`Enabled`

## 六、黑苹果安装

## 七、更改硬盘启动

## 八、其他问题

### EFI分区扩容

1. 分出合适空间大小，可使用Windows自带的`磁盘管理`或者`DiskGenius`。
2. 打开DiskGenius。
3. 找到`ESP`分区，右键选择`备份分区到镜像文件`，选择合适的文件路径并保存（选择`热备份`）。
4. 删除`ESP`分区。
5. 在任意分区上右键选择`建立ESP/MSR分区`，调整合适的分区大小并确认。
6. 保存更改。
7. 找到`ESP`分区，右键选择`从镜像文件还原分区`，选择刚才备份的镜像文件即可。

## 九、参考

- [黑果小兵的部落阁](https://blog.daliansky.net/)
- [黑苹果MacOS Big Sur 11.0 安装教程及驱动工具](https://blog.csdn.net/qq_28735663/article/details/107149730)
- [主流电脑配置的通用引导文件，包含CLOVER与OpenCorer双引导](https://blog.csdn.net/qq_28735663/article/details/109062067)

