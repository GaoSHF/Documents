## Jetson AGX Xavier 系统重装

1. 前期准备

​	提前下载好.dtb和.img文件，

```
百度云盘https://pan.baidu.com/s/1Nmvhv5vrcZopoQNt60BH4w 
提取码：8888 
```

​	一台安装Ubuntu18.04的主机，NVIDIA SDK manager with JetPack 4.4 for Xavier，usb连接线

2. 下载并安装NVIDIA SDK manager 的Debian安装包

   下载地址 https://developer.nvidia.com/nvidia-sdk-manager

   安装(也可以双击直接安装)

   ```
   sudo apt install ./sdkmanager-[version].[build#].deb
   ```

3. 安装前Xavier的设置

   - 确定ignition 开关为模式0

     ![image-20211202142225315](https://cdn.jsdelivr.net/gh/GaoSHF/7011/blogs/202205/image-20211202142225315.png)

   - 接通电源

   - 使用USB数据线连接Xavier和已经安装了Ubuntu的主机

     ![image-20211202142515872](https://cdn.jsdelivr.net/gh/GaoSHF/7011/blogs/202205/image-20211202142515872.png)

   - 启动Xavier（NRU)到 recovery mode

     ![image-20211202142546819](https://cdn.jsdelivr.net/gh/GaoSHF/7011/blogs/202205/image-20211202142546819.png)

     在关机状态下保持按压 Force Recovery Button，然后按下开机键（开机键按一下就可以），大约五秒后松开 Force Recovery Button。

   - terminal中输入`lsusb`，如果系统成功启动recovery mode，将会看到如下结果

     ![image-20211202143029532](https://cdn.jsdelivr.net/gh/GaoSHF/7011/blogs/202205/image-20211202143029532.png)

     

     4. 重刷NRU系统

        ```
        sudo apt-get update
        sdkmanager
        ```

        在STEP 01中 需要准备host machine（即已安装Ubuntu的主机），target hardware（Jetson AGX Xavier），JetPack4.4（不是JetPack 4.4 DP ）如下所示

        ![image-20211202143821891](https://cdn.jsdelivr.net/gh/GaoSHF/7011/blogs/202205/image-20211202143821891.png)

        如果未显示Detected，回到3步骤继续

        ![image-20211202144002629](https://cdn.jsdelivr.net/gh/GaoSHF/7011/blogs/202205/image-20211202144002629.png)

        JetPack 4.4 (NOT JetPack 4.4 DP)

        ![image-20211202144020317](https://cdn.jsdelivr.net/gh/GaoSHF/7011/blogs/202205/image-20211202144020317.png)

        进入STEP 02，点击Jetson OS 以及“i accept the terms ...”在SDK Manager底部，然后CONTINUE，该过程需要联网下载大量文件，该过程完成后会在L4T路径下产生一个`flash.sh`文件大概在如下路径中

        ```
        ~/nvidia/nvidia_sdk/JetPack_4.4_Linux_JETSON_AGX_XAVIER/Linux_for_Tegra
        ```

     5. 刷写Neousys 预编译系统Image

        按照3中进入到Recovery mode

        把.dtb文件放到类似如下路径中（注意.dtb文件名字，后续要用）

        `~/nvidia/nvidia_sdk/JetPack_4.4_Linux_JETSON_AGX_XAVIER/Linux_for_Tegra`

        把.img重命名为`system.img`放到类似下边路径中

        `~/nvidia/nvidia_sdk/JetPack_4.4_Linux_JETSON_AGX_XAVIER/Linux_for_Tegra/bootloader`

        在终端输入

        ```
        export L4T_ROOT_J4_4=/home/nvidia/nvidia/nvidia_sdk/JetPack_4.4_Linux_JETSON_AGX_XAVIER/Linux_for_Tegra
        export SYS_IMG=NRU-100_20201222
        sudo cp $SYS_IMG $L4T_ROOT_J4_4/bootloader/system.img
        ```

        

     6. 刷写文件系统和Device Tree

        ```
        export  L4T_ROOT_J4_4=/home/nvidia/nvidia/nvidia_sdk/JetPack_4.4_Linux_JETSON_AGX_XAVIER/Linux_for_Tegra
        # NRU_JetPack4.4_v0.8为上述.dtb文件名字确保与文件名一致
        export NRU_DTS=NRU_JetPack4.4_v0.8
        cd $L4T_ROOT_J4_4
        sudo ./flash.sh -r -d $NRU_DTS.dtb jetson-xavier mmcblk0p1
        
        ```

        刷写完成后如图所示

        ![image-20211202150135608](https://cdn.jsdelivr.net/gh/GaoSHF/7011/blogs/202205/image-20211202150135608.png)

     

     

     

     