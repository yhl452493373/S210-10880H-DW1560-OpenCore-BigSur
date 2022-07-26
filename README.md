# S210-10980HK(10880H)-DW1560-OpenCore-BigSur，支持Monterey、Ventura

## 目前来说，已经基本没啥可完善的地方了，因此以后的更新都是常规更新，即OpenCore和Kext更新

# 2021.12.16重要说明：今天查电源日志的时候，发现每隔两小时会唤醒一次，唤醒原因似乎为【唤醒以供以网络访问】和另一个RTC任务（不确定RTC任务是否就是因为开了唤醒以供网络访问出现的），因此需要在电源管理里面把“唤醒以供以太网访问”、断电后自动启动、启用电能小憩都关掉。

# i211AT网卡在Monterey下无法上网有两种解决方式，酌情选用：

## 1、换驱动，将SmallTreeIntel82576换成[AppleIGB](https://github.com/donatengit/AppleIGB)。此方式下，可以正常上网，但是虚拟机下使用桥接网络桥接到211网卡，无法上网。如果你不使用虚拟机桥接网络，建议用这个。（config.plist中默认这样处理）

## 2、刷固件，将i211at刷成i210，然后去掉AppleIGB和SmallTreeIntel82576这两个驱动，此法副作用未知


基于opencore0.8.3开发版

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
以下均基于BigSur 11.4测试，Catalina没测试。如果要使用Catalina，建议用最新的10.15.7进行测试。（2021.08.19测试，Monterey可以正常安装使用。）

①、全部正常：
DP、HDMI2.0（单DP、单HDMI、DP+HDMI）
睡眠唤醒
原生电源管理
DW1830蓝牙
DW1830无线网卡
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

### 6、可以在windows中安装macos支持文件（启动转换助理的“操作”菜单中可以下载，进windows后，安装），实现类似白苹果的windows下选择启动系统功能。注意，安装后需要在c:\program files\apple software update下将bootcamp升级，否则bootcamp会错误识别所有mac磁盘都可作为macos启动

