
# Lenovo Miix520 Hackintosh’s Boot files
支持macos10.14.x、10.15.x

联想 Miix-520 二合一平板电脑 Hackintosh 即用配置(Clover)

#### 目录

1. 配置信息
2. 正常工作模块
3. 不正常工作模块
3. bug与解决方式
	- bug1:触摸板与触摸屏不能同时使用
	- bug2:睡眠唤醒后键盘失效(重新拔插后正常)
      - bug3:多点触控失效
5. 安装指北

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

## 2.正常工作模块

1. 声显网：OK 速度略有下降
2. USB，TypeC(霹雳)：OK 满速
3. 电量显示：OK
4. 亮度调节：OK
5. 变频：OK
6. 盒盖睡眠 开盖唤醒：1/2 OK (不安装插件需插拔键盘)
7. 触摸屏、手写笔：OK
8. 蓝牙：OK
9. usb键盘、鼠标唤醒：OK
10. SD读卡器：自行解决（逃。。。。。

## 3.不正常工作模块

1. I2C的重感、摄像头（无解）
2. SD读卡器  (实测有解)
3. iMessage（未实测，有解）
4. 指纹识别（无解）

## 4.bug与解决方式
   ### 1:触摸板与触摸屏不能同时使用
（看起来没多大问题，无需求）

   ### 2.睡眠唤醒后键盘失效(重新拔插后正常)

这是个奇葩的bug，可能和键盘硬件有关，经过搜索，发现不少用户遇到了唤醒后鼠标失效、键盘失效的问题，重新拔插后又能正常使用了，针对这个bug，解决办法就是安装sleepwatcher来监控系统的睡眠唤醒，在电脑唤醒时执行一条重连usb设备的命令，该补丁包默认适合miix 520的键盘bug，如果想要用到别的电脑上解决重连usb的问题，需要修改/usr/local/acai/patch，里面的0x14500000是miix 520键盘的usb口的地址。

   #### 解决：
      1. 安装brew，终端执行如下命令

    `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
      2. 安装sleepwatcher，终端执行如下命令

	`brew install sleepwatcher`

      3. 下载解压补丁包 : 解决唤醒后需要拔插键盘问题的方案.zip

      4. 终端进入补丁包目录，执行如下命令

	`sudo sh ./patch.sh`
    
### 3.多指触控失效
（暂无方案）
    
## 5.安装指北
1. U盘一个，容量最好大于**8G**；电脑硬盘预留一个分区，用来装mac；硬盘的esp分区大小大于等于**200Mb**，否则安装mac系统时抹盘会失败(macos esp分区大小约200Mb)。
2. bios里磁盘模式为ahci，启动模式为uefi，**安全启动关闭。
3. dmg格式的macos10.14.x或macos10.15.x 镜像，（推荐 黑果小兵 双EFI版）。
4. 刻录镜像到u盘的工具，推荐[etcher](https://www.balena.io/etcher/)
5. 刻录完成后，替换电脑硬盘esp分区的clover文件夹为该项目的clover（BOOT如果不替换也可正常运行 则不替换，否则替换）
6. 安装完成后，添加硬盘启动引导(推荐ICEBOOT)
