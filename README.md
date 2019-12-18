# Dell Optiplex 7020 Hackintosh指南
## 机器配置
- CPU：i7-4790
- GPU：HD4600 (成功驱动) + R5 240（未驱动）
- 芯片组：Q87
## 安装结果
显卡1536MB，DP外接1080p显示器成功驱动，声卡网卡USB接口均成功驱动
## 安装指南
### 备份重要数据
**下列操作有可能导致硬盘数据丢失或者现有操作系统无法引导。**
### 安装盘制作
使用TransMac或其他同类软件将macOS Mojave 10.14.5操作系统写入U盘，然后使用Disk Genius或其他同类软件将本EFI文件夹拷入U盘的EFI分区
### BIOS相关设置
首先，由于7020机型较旧，BIOS版本很可能是出厂时的旧版，请先在Windows下去Dell官网下载更新最新版的BIOS。更新完成后请先恢复BIOS的默认设置。

其次，我接触到的7020机型是传统的BIOS引导+MBR分区表，macOS无法支持MBR分区表的硬盘，请将硬盘分区表转换为GPT。

然后依照如下设置

- General
  - Advanced Boot Options -> Enable Legacy Roms选中
  - UEFI: BootPathSecurity 选择Never
- System Configuration
  - Integrate NIC 选择Enable
  - SATA Opreation 选择AHCI
- Video
  - Primary Dsiplay 选择Intel
  - Power Management -> Deep Sleep Control 选择Disabled
- Post Behavior
  - MEBx Hotkey 反选Enable HotKey
- Virtulisation Support
  - VT for Direct I/O 反选Enable VT-d

请勿擅自修改其他配置，然后点击Apply重启即可
### 安装macOS
拔掉网线，开机F12选择U盘引导
### 调整DVMT大小
安装完成后，如果你的HD4600显卡无法正常驱动，请参照这里操作

1. 将GRUB EFI Shell拷入另一个FAT32格式U盘的/EFI/BOOT/目录下，并将EFI文件命名为bootx64.efi
2. 重启使用这个U盘引导
3. 在命令行中输入
```
setup_var 0xDA2 0x0
setup_var 0x263 0x3
```
重启进入macOS即可。
## 参考资料
1. [tonymacx86](https://www.tonymacx86.com/threads/dell-optiplex-3020-7020-9020-high-sierra-installation.253400/)
2. [tonymacx86](https://www.tonymacx86.com/threads/dell-7020-without-external-gpu-build-files-and-overview-for-the-experienced-4k-ready.279108/)
3. [tonymacx86](https://www.tonymacx86.com/threads/dell-optiplex-7020-4k-monitors-on-intel-4600-integrated-gpu.282589/)