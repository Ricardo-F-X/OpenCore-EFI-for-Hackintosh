# OpenCore-EFI-for-Hackintosh
OpenCore EFI for Hackintosh on Lenovo eraser Z510
Editor: Ricardo Xie
本文记录2020年开始，我断断续续摸爬滚打的Hackintosh历程。

2020.2

疫情时代的远程办公，给了我在工作之余对自己的设备进行改进的时机和心力。尤其在入手iPad Pro2020和iPhone XS之后，整备Apple全家桶的心思日固。于是从远景和GitHub上爬帖开始，历经十天左右，我开始着手实操。

但是对原Win环境的依赖还是让我不敢冒进，先用了VMware15，挂载了Mac OS Catalina，效果还可以，但是虚拟机的功率严重限制了性能，导致我全无在Mac环境下生产的意愿。同时由于隔着一层Windows，众多硬件无法与其他Apple设备联动，所以很快这份镜像就停用了。

黑果如果要用，就要用最黑的。

2020.4

工作节奏在一番忙碌后稍缓，我也开始重新打量Hackintosh的配置。在Bilibili参考了@大头蔡老师和@黑苹果屋的一系列教程之后，我决心推倒重来，对Windows进行了全盘的备份和整理。同时找到了一个和我同机型的黑果微信群，开始折腾。

最初的这次配置，过程其实十分简单。对于大多数黑果小白来说，真的其实只需要花力气找到同配置或者相似配置的朋友，套用一套Clover UEFI即可。在获取了Lenovo Z510的Clover之后，用blatch加载mac OS的镜像后直接安装即可。

在安装过程中，我也碰到了众多新手的常见问题，比如不会用-v进入啰嗦模式来查看Warning message；或者卡在五国界面加载停滞；这种问题在针对性的爬帖之后都能在操作层面得到快速的解决。Catalina达成。

达成Catalina后，体验得出了几个要点：

    * 直接使用clover的引导要比虚拟机效率高出几倍不止；哪怕是Z510这种四代u的老旧笔记本。
    * 同样硬件配置下，macos在一定程度上甚至比win环境更加稳定，更加流畅。
    * 由于非白果，众多硬件没有办法面面俱到，例如facetime、imassage需要三码注入，airdrop需要适合的硬件和网卡，随航同理。甚至随航会有细微的花屏；HDMI不能输出等众多问题，但是基本的Macos已经很让人满意。
    * 黑苹果永远在折腾的路上，哪怕多次下定决心舍弃一些功能，但是只要出现系统更新或者爬到更好的引导帖子，还是会不自主的打开config.plist

2020.5

为了完善黑果，打开AirDrop和sidecar，我购入了一块BCM94352HMB的网卡，本以为就是简单的拆机替换，但是万万没想到，联想的主板在网卡的替换上做了白名单的限制。因而问题转换成破解白名单。

网上关于Z510的白名单破解，已有完整的教程。主要原理是在利用原有BIOS的睡眠模式不断电时解除其对BIOS镜像的非官方刷写的限制，网上提示教程中有操作风险，此言不虚，后来者需谨慎处理。

刷写之后，替换原有网卡；同时在原有EFI中，添加关于BCM网卡的驱动kexts，同时在config中编辑注入参数即可。airdrop和sidecar完美开启。

需要注意的是，sidecar的使用前提，需要将机型注入为支持sidecar的机型，同时兼顾自身硬件匹配度。

2020.9

回到老家，短暂逍遥几日。看到clover转opencore的趋势愈盛，我准备也做个迁移，在OC下简化一下引导，同时为Big Sur的安装做好准备。

转移OC的过程很简单，参考了大头蔡老师的教程，仿照原来的文件目录结构，成功转移。但是在安装big sur时，无法引导安装。出现了众多卡logo、卡IO warning、卡12分钟安装等众多问题，甚至在安装好镜像到硬盘分区后，仍然无法从硬盘启动。

因为学习任务较多，短暂搁置big sur的安装任务。

2021.1.15

* 发现唤醒后存在重启问题 复现率100%
    * 唤醒后5s内一切正常然后黑屏重启。
    * 重启后warning reoprt显示 错误原因为AppleHDAHDMI_DPDriver::setPowerState，故考虑屏蔽本无用的HDMI Audio输出
        * 首先在Device Add下找到了Text为IntelHDMIAudio的键，同时打开HackintoshTool发现，音频设备中存在HDMI输出
        * [CSDN](https://blog.csdn.net/lxyoucan/article/details/111877620?fileGuid=WyD6hCDJ9T9kGqTr)帖子中提供了屏蔽HDMI Audio的补丁，测试无效
        * [远景](http://bbs.pcbeta.com/viewthread-1850761-1-1.html?fileGuid=WyD6hCDJ9T9kGqTr)帖子中提到，在声卡中添加补丁

属性(键)                       属性值                     类型
No-hda-gfx           AAAAAAAA AA0A          data

但测试发现，OC 0.6.3中此键值为text型，不好更改，搁置

        * [Github](https://github.com/SuperNG6/MSI-B360-Catalina-EFI/issues/146?fileGuid=WyD6hCDJ9T9kGqTr)中提供了OC配置文件中的kernel/quirks/PowerTimeoutKernelPanic，打开这个开关就可以解决问题，实测解决问题

本文记录2020年开始，我断断续续摸爬滚打的Hackintosh历程。

2020.2

疫情时代的远程办公，给了我在工作之余对自己的设备进行改进的时机和心力。尤其在入手iPad Pro2020和iPhone XS之后，整备Apple全家桶的心思日固。于是从远景和GitHub上爬帖开始，历经十天左右，我开始着手实操。

但是对原Win环境的依赖还是让我不敢冒进，先用了VMware15，挂载了Mac OS Catalina，效果还可以，但是虚拟机的功率严重限制了性能，导致我全无在Mac环境下生产的意愿。同时由于隔着一层Windows，众多硬件无法与其他Apple设备联动，所以很快这份镜像就停用了。

黑果如果要用，就要用最黑的。

2020.4

工作节奏在一番忙碌后稍缓，我也开始重新打量Hackintosh的配置。在Bilibili参考了@大头蔡老师和@黑苹果屋的一系列教程之后，我决心推倒重来，对Windows进行了全盘的备份和整理。同时找到了一个和我同机型的黑果微信群，开始折腾。

最初的这次配置，过程其实十分简单。对于大多数黑果小白来说，真的其实只需要花力气找到同配置或者相似配置的朋友，套用一套Clover UEFI即可。在获取了Lenovo Z510的Clover之后，用blatch加载mac OS的镜像后直接安装即可。

在安装过程中，我也碰到了众多新手的常见问题，比如不会用-v进入啰嗦模式来查看Warning message；或者卡在五国界面加载停滞；这种问题在针对性的爬帖之后都能在操作层面得到快速的解决。Catalina达成。

达成Catalina后，体验得出了几个要点：

    * 直接使用clover的引导要比虚拟机效率高出几倍不止；哪怕是Z510这种四代u的老旧笔记本。
    * 同样硬件配置下，macos在一定程度上甚至比win环境更加稳定，更加流畅。
    * 由于非白果，众多硬件没有办法面面俱到，例如facetime、imassage需要三码注入，airdrop需要适合的硬件和网卡，随航同理。甚至随航会有细微的花屏；HDMI不能输出等众多问题，但是基本的Macos已经很让人满意。
    * 黑苹果永远在折腾的路上，哪怕多次下定决心舍弃一些功能，但是只要出现系统更新或者爬到更好的引导帖子，还是会不自主的打开config.plist

2020.5

为了完善黑果，打开AirDrop和sidecar，我购入了一块BCM94352HMB的网卡，本以为就是简单的拆机替换，但是万万没想到，联想的主板在网卡的替换上做了白名单的限制。因而问题转换成破解白名单。

网上关于Z510的白名单破解，已有完整的教程。主要原理是在利用原有BIOS的睡眠模式不断电时解除其对BIOS镜像的非官方刷写的限制，网上提示教程中有操作风险，此言不虚，后来者需谨慎处理。

刷写之后，替换原有网卡；同时在原有EFI中，添加关于BCM网卡的驱动kexts，同时在config中编辑注入参数即可。airdrop和sidecar完美开启。

需要注意的是，sidecar的使用前提，需要将机型注入为支持sidecar的机型，同时兼顾自身硬件匹配度。

2020.9

回到老家，短暂逍遥几日。看到clover转opencore的趋势愈盛，我准备也做个迁移，在OC下简化一下引导，同时为Big Sur的安装做好准备。

转移OC的过程很简单，参考了大头蔡老师的教程，仿照原来的文件目录结构，成功转移。但是在安装big sur时，无法引导安装。出现了众多卡logo、卡IO warning、卡12分钟安装等众多问题，甚至在安装好镜像到硬盘分区后，仍然无法从硬盘启动。

因为学习任务较多，短暂搁置big sur的安装任务。

2021.1.15

* 发现唤醒后存在重启问题 复现率100%
    * 唤醒后5s内一切正常然后黑屏重启。
    * 重启后warning reoprt显示 错误原因为AppleHDAHDMI_DPDriver::setPowerState，故考虑屏蔽本无用的HDMI Audio输出
        * 首先在Device Add下找到了Text为IntelHDMIAudio的键，同时打开HackintoshTool发现，音频设备中存在HDMI输出
        * [CSDN](https://blog.csdn.net/lxyoucan/article/details/111877620?fileGuid=WyD6hCDJ9T9kGqTr)帖子中提供了屏蔽HDMI Audio的补丁，测试无效
        * [远景](http://bbs.pcbeta.com/viewthread-1850761-1-1.html?fileGuid=WyD6hCDJ9T9kGqTr)帖子中提到，在声卡中添加补丁

属性(键)                       属性值                     类型
No-hda-gfx           AAAAAAAA AA0A          data

但测试发现，OC 0.6.3中此键值为text型，不好更改，搁置

        * [Github](https://github.com/SuperNG6/MSI-B360-Catalina-EFI/issues/146?fileGuid=WyD6hCDJ9T9kGqTr)中提供了OC配置文件中的kernel/quirks/PowerTimeoutKernelPanic，打开这个开关就可以解决问题，实测解决问题

