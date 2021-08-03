# S210-10980HK(10880H)-DW1560-OpenCore-BigSur

## 目前来说，已经基本没啥可完善的地方了，因此以后的更新都是常规更新，即OpenCore和Kext更新

基于opencore0.7.1

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
以下均基于BigSur 11.4测试，Catalina没测试。如果要使用Catalina，建议用最新的10.15.7进行测试

①、全部正常：
DP、HDMI2.0（单DP、单HDMI、DP+HDMI）
睡眠唤醒
原生电源管理
DW1560蓝牙
DW1560无线网卡
I219V（背面靠近双USB3.0的那个）
I211AT（背面靠近电源接口的那个）
USB（3.0、2.0、type-c）
随航、接力、隔空投送
耳机、麦克风、DP/HDMI音频

### 3、BIOS设置
Boot - CSM Configuration - CSM Support [ Disable ]

### 4、其他问题
系统偏好设置 - 节能 中的电池项设置不能保存，这个直接忽略，毕竟本来就没电池
系统偏好设置 - 节能 中的电能小憩需要取消选中，否则长时间睡眠后可能崩溃
启动时，如果出现苹果logo被压扁，尝试换一个显示器。我自己的XV273K暗影骑士，接HDMI口，启动一阶段就是扁的，进系统后正常。我的便携显示器，接的mini dp，启动过程logo都正常
如果你觉得 关于本机 - 内存 那里显示的4根内存插槽不符合小主机的内存槽数量，可以参考 OpenCore0.6.3以上自定义内存信息设置
多台显示器情况下，进recovery后要不了多久会花屏，只接一台显示器进recovery即可

### 5、特殊情况说明
S210,10880H机型，声卡为alc 282，win下必须装reltek声卡驱动
alc 282目前最接近的id为86，声卡有声音，麦克风无声；
或者选择VoodooHDA代替AppleALC；此法存在缺陷：多个带喇叭的显示器下，只有一个显示器能发声，且睡眠唤醒后，所有显示器都不发声，但是耳麦接口都正常

### 6、opencore0.6.6开始，删除了bootstrap.efi，因此你更新此efi后，可能会变成只能进入到windows的情况，请用easyuefi修改opencore启动项，将其指向efi磁盘下/boot/bootx64.efi，然后把其启动顺序调整到最上面，重启即可

### 7、可以在windows中安装macos支持文件（启动转换助理的“操作”菜单中可以下载，进windows后，安装），实现类似白苹果的windows下选择启动系统功能。注意，安装后需要在c:\program files\apple software update下将bootcamp升级，否则bootcamp会错误识别所有mac磁盘都可作为macos启动

