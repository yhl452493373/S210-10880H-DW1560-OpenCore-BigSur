# S210-10980HK(10880H)-BCM94360Z4-OpenCore-Sonoma，支持Monterey、Ventura、Sonoma

## 目前来说，已经基本没啥可完善的地方了，因此以后的更新都是常规更新，即OpenCore和Kext更新

# 注意：
+ **csr-active-config改为通过`ToggleSipEntry.efi`开关，关闭状态值改为`7F0A0000`，可以在`UEFI`-`Drivers`-`ToggleSipEntry.efi`的`Arguments`里面设置，目前是`0xA7F`，计算后就是`7F0A0000`**
+ **如果你在macOS 13 Ventrua中更新系统时，每次的都必须下载全量的更新包，可以试试将 Kexts 中的 BlueToolFixup.kext 临时禁用，然后重启在更新。**
+ **由于我的BCM94360Z4是完全免驱的，所以我只启用AirportBrcmFixup来修改网卡地区，你需要根据你的硬件来启用BrcmPatchRAM**
+ **如果禁用 BlueToolFixup.kext 后重启，蓝牙能正常工作，那么可以把改kext从配置文件中删除**
+ **如果禁用 BlueToolFixup.kext 后重启，蓝牙能正常工作，那么可以试着将 BrcmFirmwareData.kext 以及 BrcmPatchRAM3.kext 禁用。如果禁用这两个kext后重启蓝牙仍然能正常工作，则可以直接将蓝牙相关的驱动完全移除。**
+ **如果禁用 BlueToolFixup.kext 后重启，蓝牙不能正常工作，那么在更新完成后，启用该kext**

# 关于Sonoma
+ **Sonoma本身去掉了以前没有问题的博通网卡的支持，只能用OCLP打补丁来恢复支持，但这么做有副作用：不能增量更新、每次更新后重新打补丁**
+ **Sonoma通过OCLP项目可以驱动博通网卡，但是存在限制：OCLP项目组支持的网卡的设备ID（Hackintool - PCIe - 设备）为`0x43BA`、`0x43A3`、`0x43A0`、`0x4331`、`0x4353`的才能驱动**
+ **为方便OCLP给Sonoma打补丁，默认关闭了SIP，禁用了Secure Boot Model**
+ **OCLP使用的软件`OpenCore-Patcher`可以下载Sonoma的安装包，下载会自动安装到系统的应用程序里面**
+ **`OpenCore-Patcher`下载地址：[OpenCore-Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher/releases)。**
+ **OCLP驱动方法，请移步[使用OCLP在macOS Sonoma中驱动博通网卡](https://bbs.pcbeta.com/viewthread-1975133-1-1.html)，请仔细阅读**
+ **OCLP驱动后的网卡若速率不达标，请移步[解决sonoma下博通网卡OCLP补丁后仍然无法驱动以及驱动后速率低的问题，送给需要的人](https://bbs.pcbeta.com/viewthread-1975162-1-1.html)**

# 2023.10.09说明：DUANG的音效是否播放，取决于NVRAM中的`StartupMute`是否为`00`。这个值由`系统设置`-`声音`-`启动时播放声音`决定。如该选项开启，则启动时播放DUANG的声音，关闭则不播放。如果要强制播放，则把config.plist中的`UEFI`-`PlayChime`改为`Enable`。DUANG的音效只能由3.5mm耳机孔输出，hdmi或dp无法输出。

# 2021.12.16重要说明：今天查电源日志的时候，发现每隔两小时会唤醒一次，唤醒原因似乎为【唤醒以供以网络访问】和另一个RTC任务（不确定RTC任务是否就是因为开了唤醒以供网络访问出现的），因此需要在电源管理里面把“唤醒以供以太网访问”、断电后自动启动、启用电能小憩都关掉。

# i211AT网卡在最新的Ventura、Monterey下目前免驱，因此删除了[AppleIGB](https://github.com/donatengit/AppleIGB)驱动，同时不再需要刷成210，直接i211即可（或者可以试试用efi中提供的i211网卡固件重刷一次）。

# 从macOS 12.3开始，[不需要SSDT-PLUG.aml](https://dortania.github.io/OpenCore-Post-Install/universal/pm.html#enabling-x86platformplugin)，因此本EFI`默认为禁用`状态。如果你系统版本低于12.3，请启用该ACPI补丁。

基于opencore0.9.5正式版

+ 2023.10.09 声卡id由11改为37，解决插入耳机后微信等软件声音小问题；增加进入OC选择界面时的DUANG的音效（DUANG的音效只能由3.5mm耳机孔输入，hdmi或dp无法输出）
+ 2023.10.07 更新AirportBrcpFixup、RestrictEvents到最新
+ 2023.09.13 更新oc到0.9.5正式版，更新kext到最新
+ 2023.08.08 更新oc到0.9.4正式版，更新kext到最新
+ 2023.08.01 增加AMFIPass.kext，启动参数增加-amfipassbeta，移除amfi0=0x80，解决VMWare Fusion以及注入后的PD虚拟机无法使用问题
+ 2023.07.27 csr-active-config改为通过`ToggleSipEntry.efi`开关，关闭状态值改为`7F0A0000`，可以在`UEFI`-`Drivers`-`ToggleSipEntry.efi`的`Arguments`里面设置，目前是`0xA7F`，计算后就是`7F0A0000`
+ 2023.07.26 更新oc到0.9.3正式版，更新kext到最新，支持安装Sonoma，通过OCLP驱动部分博通网卡
+ 2023.03.10 更新oc到0.9.0正式版，更新kext到最新
+ 2023.02.15 更新oc到0.8.9正式版，更新kext到最新
+ 2023.01.04 更新oc到0.8.8正式版，更新kext到最新
+ 2022.12.07 更新oc到0.8.7正式版，更新kext到最新
+ 2022.11.08 更新oc到0.8.6正式版，更新kext到最新
+ 2022.10.07 去除i211的AppleIGB驱动，在最新的Monterey、Ventrua下测试，为免驱
+ 2022.10.06 更新oc到0.8.5正式版，更新kext到最新
+ 2022.09.17 更新AppleIGB驱动以修复macOS Ventura下i211网卡无法上网问题；重新定制视屏输出端口
+ 2022.09.07 更新oc到0.8.4正式版，更新kext到正式版;重新定制USB端口
+ 2022.08.02 更新oc到0.8.3正式版，更新kext到正式版
+ 2022.07.26 解决macOS Monterey下，i211at网卡无法上网问题
+ 2022.07.11 更新oc到0.8.3开发版，更新部分kext到开发版（现在支持macOS Beta3）
+ 2022.07.07 更新oc到0.8.2正式版，更新kext到正式版（此版不支持安装macOS13 beta3）
+ 2022.06.12 更新oc到0.8.2开发版，更新kext到开发版
+ 2022.06.07 更新oc到0.8.1，更新kext到最新
+ 2022.03.08 更新oc到0.7.9，更新kext到最新
+ 2022.02.11 更新oc到0.7.8，更新kext到最新
+ 2022.01.11 更新oc到0.7.7，更新kext到最新
+ 2021.12.16 修复睡眠后每2小时被唤醒一次的问题
+ 2021.12.07 更新oc到0.7.6，更新kext到最新
+ 2021.11.10 更新oc到0.7.5，更新kext到最新
+ 2021.10.07 增加-revsbvmm参数，在任意SecureBootModel下都能检测到更新；关闭SecureBootModel
+ 2021.10.02 变更部分配置
+ 2021.09.30 改变PciRoot(0x0)/Pci(0x2,0x0)的AAPL,ig-platform-id为0900A53E ，同时重新定制显卡补丁，以解决Monterey Beta8下DP无输出问题（Beta7以及以下版本未测试）
+ 2021.09.24 更新oc到0.7.4开发版，据说解决了beta6升级到beta7时一直重启的问题，需要用OCC最新版并切换到开发版来编辑（也可以用OCAT最新版）
+ 2021.09.07 更新oc到0.7.3，更新kext到最新，支持macOS12的AirPlay to Mac (已测试）)
+ 2021.08.19 支持Monterey Beta版，可以正常安装使用，但部分软件和Monterey存在兼容性问题。
+ 2021.08.03 更新oc到0.7.2，更新kext到最新，支持macOS12的AirPlay to Mac（未测试）
+ 2021.08.02 增加HoRNDIS驱动，可以通过安卓的USB共享网络上网
+ 2021.07.23 修复重启问题
+ 2021.07.14 重新定制USBMap.kext;修复单显示器下DP音频或HDMI音频未识别的问题
+ 2021.07.06 更新oc到0.7.1，更新kext到最新
+ 2021.06.09 更新oc到0.7.0，更新kext到最新
+ 2021.05.06 更新oc到0.6.9，更新kext到最新
+ 2021.04.07 更新oc到0.6.8，更新kext到最新
+ 2021.03.02 更新oc到0.6.7，更新kext到最新
+ 2021.02.24 修复在macOS 11.2.1 (20D75)中，插入ipad设备充电后出现的频繁突然断电问题

---
 
### 1、我的配置
机型S210，i9 10980hk，64G，4K显示器（mini dp）+4K显示（hdmi 2.0），声卡为alc235（老板说是alc 233，但声卡id0x10ec0235即实际alc 235）

### 2、工作情况
以下均基于Ventrua测试

①、全部正常：
DP、HDMI2.0（单DP、单HDMI、DP+HDMI）
睡眠唤醒
原生电源管理
BCM94360Z4蓝牙免驱
BCM94360Z4无线网卡免驱
I219V（背面靠近双USB3.0的那个）
I211AT（背面靠近电源接口的那个）
USB（3.0、2.0、type-c）
随航、接力、隔空投送
耳机、麦克风、DP/HDMI音频

### 3、BIOS设置
Boot - CSM Configuration - CSM Support [ Disable ]

### 4、其他问题
系统偏好设置 - 节能 唤醒以供网络访问，取消选中，否则睡眠后过一段时间会被唤醒
系统偏好设置 - 节能 断点后自动启动，取消选中，这个在黑苹果上没啥用
系统偏好设置 - 节能 中的启用电能小憩需要取消选中，否则长时间睡眠后可能崩溃
启动时，如果出现苹果logo被压扁，尝试换一个显示器。我自己的XV273K暗影骑士，接HDMI口，启动一阶段就是扁的，进系统后正常。我的便携显示器，接的mini dp，启动过程logo都正常
如果你觉得 关于本机 - 内存 那里显示的4根内存插槽不符合小主机的内存槽数量，可以参考 OpenCore0.6.3以上自定义内存信息设置

### 5、特殊情况说明
S210,10880H机型，声卡为alc 282，win下必须装reltek声卡驱动
alc 282目前最接近的id为86，声卡有声音，麦克风无声；
或者选择VoodooHDA代替AppleALC；此法存在缺陷：多个带喇叭的显示器下，只有一个显示器能发声，且睡眠唤醒后，所有显示器都不发声，但是耳麦接口都正常

### 6、可以在windows中安装macos支持文件（启动转换助理的“操作”菜单中可以下载，进windows后，安装），实现类似白苹果的windows下选择启动系统功能。注意，安装后需要在c:\program files\apple software update下将bootcamp升级，否则bootcamp会错误识别所有mac磁盘都可作为macos启动。安装后，会隐藏一些磁盘，请参照[彻底解决win10开机后，D盘或者其他隐藏，每次要重新添加盘符的方法](http://www.purplestone.cn/share/2361.html)处理

