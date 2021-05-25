# Lenovo Miix520 Hackintosh’s Boot files
支持macos10.14.x、10.15.x

联想 Miix-520 二合一平板电脑 Hackintosh 即用配置(Clover)

#### 目录

1. 配置信息
2. 系统运行截图
3. 正常工作
4. 不正常工作
5. bug与解决方式
	- bug1:触摸板与触摸屏不能同时使用
	- bug2:睡眠唤醒后键盘失效(重新拔插后正常)
6. 更新日志
7. 常见问题解答
	- 1:安装前的准备有哪些？
	- 2:为什么安装时进度条走一半就卡住了？
	- 3:为什么安装后触屏不能用？
	- 4:为什么睡眠唤醒后键盘与触摸板就不能用了？
	- 5:为什么多指触控不正常？
	- 6:我可以随意更新系统吗？
	- 7:开机声音怎么开启与关闭？
	- 8:读卡器能驱动吗？
	- 9:无线网卡怎么驱动？10:为什么转换为以OpenCore更新为主？
	- 10:为什么转换为以OpenCore更新为主？
        - 11:如何禁用OpenCore开机声音？

## 1.配置信息

- 品牌型号：联想Miix 520
- cpu：i5 8250u 
- 显卡：uhd620
- 内存：8G
- 声卡：alc298
- **无线网卡：原装 Intel AC3165**
- 屏幕大小：12.2寸
- 分辨率：1920x1200
- NVME硬盘：samsung pm961 256Gb
- bios：6ncn35ww

## 3.正常工作

1. 声显网：OK
2. USB，TypeC(霹雳)：OK
3. 电量显示：OK
4. 亮度调节：OK
5. 变频：OK
6. 盒盖睡眠 开盖唤醒：1/2 OK (不安装插件需插拔键盘)
7. 触摸屏、手写笔：OK
8. 蓝牙：OK
9. usb键盘、鼠标唤醒：OK
10. SD读卡器：自行解决（逃。。。。。

## 4.不正常工作

1. I2C的重感、摄像头（无解）
2. SD读卡器  (实测有解)
3. iMessage（未实测，有解）
4. 指纹识别（无解）

## 5.bug与解决方式

### -> 睡眠唤醒后键盘失效(重新拔插后正常)

这是个奇葩的bug，可能和键盘硬件有关，经过搜索，发现不少用户遇到了唤醒后鼠标失效、键盘失效的问题，重新拔插后又能正常使用了，针对这个bug，解决办法就是安装sleepwatcher来监控系统的睡眠唤醒，在电脑唤醒时执行一条重连usb设备的命令，该补丁包默认适合miix 520的键盘bug，如果想要用到别的电脑上解决重连usb的问题，需要修改/usr/local/acai/patch，里面的0x14500000是miix 520键盘的usb口的地址。

#### 解决：

1. 安装brew，终端执行如下命令


    `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

2. 安装sleepwatcher，终端执行如下命令

	`brew install sleepwatcher`

3. 下载解压补丁包 : 解决唤醒后需要拔插键盘问题的方案.zip

4. 终端进入补丁包目录，执行如下命令

	`sudo sh ./patch.sh`
## 7.常见问题解答

### 1:安装前的准备有哪些？
1. U盘一个，容量最好大于**8G**；电脑硬盘预留一个分区，用来装mac；硬盘的esp分区大小大于等于**200Mb**，否则安装mac系统时抹盘会失败(macos esp分区大小约200Mb)。
2. bios里磁盘模式为ahci，启动模式为uefi，**安全启动关闭。
3. dmg格式的macos10.14.x或macos10.15.x 镜像，（推荐 黑果小兵 双EFI版）。
4. 刻录镜像到u盘的工具，推荐[etcher](https://www.balena.io/etcher/)
5. 刻录完成后，替换u盘esp分区的clover文件夹为该项目的clover（BOOT如果不替换也可正常运行 则不替换，否则替换）
6. 按照 [常见问题解答2](https://github.com/acai66/lenovo-miix-520-hackintosh-CLOVER#2为什么安装时进度条走一半就卡住了) 里的操作来屏蔽显卡安装。
7. 安装完成后，添加硬盘启动引导（ICEBOOT）。
	
### ~~2:为什么安装时进度条走一半就卡住了？~~
~~A: 由于未知的原因，动态的显存补丁不能生效，所以导致dvmt显存检测时过不去而卡住，解决办法是暂时屏蔽显卡来安装，安装完成后，来运行kext utility来重建驱动缓存；具体做法是启动到clover界面，按字母o，找到graphics开头的选项，勾选里面的inject intel ，再修改*-platfrom-id为0x12345678，再esc键回clover主界面，启动安装，等安装好进入到桌面后，运行kext utility，输入密码，等待操作完成，quit，再不屏蔽显卡启动就好了。~~
<div align=center><img src="https://raw.githubusercontent.com/acai66/lenovo-miix-520-hackintosh-CLOVER/master/Resource/images/disable_grapgics.png" width="95%" /></div>

### 3:为什么安装后触屏不能用？
A: 可能是与系统内置i2c驱动有冲突或者第一次启动时驱动注入不成功有关，安装后完成，请重启1·2次，如果触屏还是不能使用，请手动移除sle里的AppleIntelLpssI2C.kext和AppleIntelLpssI2CController.kext，并运行kext utility来修复权限重建缓存，再重启测试触屏。

### 4:为什么睡眠唤醒后键盘与触摸板就不能用了？
A: 请看readme里 bug与解决方式部分
	
### ~~5:为什么多指触控不正常？~~
~~A: voodooi2c系列驱动的bug，等待acai66修复这个，会更新

### 6:可以随意更新系统吗？
A: 理论上可以直接更新系统，但是新系统可能会把显卡驱动恢复到未打补丁的状态，所以更新系统时也屏蔽显卡启动，等更新完成后，再运行kext utility，再不屏蔽显卡启动。
### 7:开机声音怎么开启与关闭？
A: clover的新特性支持开机声音，并且我也添加转换过后的macbook的开机声音文件到主题文件夹里了，由于这个特性的设置储存在nvram里，所以直接使用clover可能并不会有声音，需要进行有关的设置才行，使用新版的clover，开机启动到clover界面时，按字母o，找到Startup sound output,里面的Volume是音量，界面友好易懂，可自行测试。

### 8:读卡器能驱动吗？
A: 默认不再添加读卡器驱动，原因是这个不稳定，会有启动问题或别的问题，而且需求一般不是很明显。该项目的这个Sinetek-rtsx.kext就是读卡器驱动，据说这个不能用或者有别的问题，如果有问题，请自行编译该驱动，[原驱动项目开源地址](https://github.com/sinetek/Sinetek-rtsx)
