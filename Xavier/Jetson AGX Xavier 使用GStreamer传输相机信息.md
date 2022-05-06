## Jetson AGX Xavier 使用GStreamer传输相机信息

#### 1. 由于AGX Xavier 自带opencv版本为OpenCV4，在使用GStreamer进行测试时，会出现各种问题，无法找到合适的解决方案，查阅资料可能和OpenCV版本有关，因此尝试更换OpenCV版本。

1.1 卸载OpenCV4

```
 sudo apt purge libopencv*
```

1.2 安装OpenCV3

根据GitHub上提供的适配Xavier的OpenCV版本进行安装

地址`https://github.com/jetsonhacks/buildOpenCVXavier.git`

下载

```
git clone  https://github.com/jetsonhacks/buildOpenCVXavier.git
```

安装，cd 到`buildOpenCVXavier-master`目录下

```
cd  /home/gao/Downloads/buildOpenCVXavier
./buildOpenCV.sh
```

等待较长安装时间

`这是安装完后给出的支持选项，你看 ffmpeg 也有了，cuda 也有了，gstreamer 也有了!`

新开一个terminal：

```
nvidia@nvidia-desktop:~$ pkg-config --cflags --libs opencv
```

>-I/usr/local/include/opencv -I/usr/local/include -L/usr/local/lib -lopencv_cudabgsegm -lopencv_cudaobjdetect -lopencv_cudastereo -lopencv_dnn -lopencv_ml -lopencv_shape -lopencv_stitching -lopencv_cudafeatures2d -lopencv_superres -lopencv_cudacodec -lopencv_videostab -lopencv_cudaoptflow -lopencv_cudalegacy -lopencv_calib3d -lopencv_features2d -lopencv_highgui -lopencv_videoio -lopencv_photo -lopencv_imgcodecs -lopencv_cudawarping -lopencv_cudaimgproc -lopencv_cudafilters -lopencv_video -lopencv_objdetect -lopencv_imgproc -lopencv_flann -lopencv_cudaarithm -lopencv_viz -lopencv_core -lopencv_cudev

**测试** 

CMakeLists.txt 中默认把一些目录给注释了，你可以去掉注释，例如我去掉了 gpu，因为等会想测试一下，然后继续下述操作

```
vidia@nvidia-desktop:~/opencv/samples$ mkdir build
nvidia@nvidia-desktop:~/opencv/samples$ cd build
nvidia@nvidia-desktop:~/opencv/samples$ cmake ..
nvidia@nvidia-desktop:~/opencv/samples$ make -j8
```

1.3 测试基本图片功能

```
nvidia@nvidia-desktop:~/opencv/samples/build$ cd cpp

nvidia@nvidia-desktop:~/opencv/samples/build/cpp$ ./example_cpp_image ../../data/home.jpg
```

可以显示

1.4 测试GPU的解码功能

```
nvidia@nvidia-desktop:~/opencv/samples/build$ cd gpu

nvidia@nvidia-desktop:~/opencv/samples/build/gpu$ ./example_gpu_video_reader /home/nvidia/usb/zuosi/test-videos/desert-1.mp4
```

>Segmentation fault (core dumped)
>
>gpu 解码失败，这个平台不支持!

参考链接：https://blog.csdn.net/ChuiGeDaQiQiu/article/details/115676421

#### cuda_runtime.h: No such file or directory

编译过程中可能会出现如下错误：

`cuda_runtime.h: No such file or directory`

```
export PATH="/usr/local/cuda10.2/bin:$PATH"
export LD_LIBRARY_PATH="/usr/local/cuda-10.2/lib64:$LD_LIBRARY_PATH"
```



cuda 路径未设置或者设置错误

#### 2. 可能cv_bridge需要重新安装

```
sudo apt-get install ros-melodic-cv-bridge
```



#### 3. 遇到cv_bridgeConfig.cmake：113 问题

ROSERROR : CMake Error at /opt/ros/melodic/share/cv_bridge/cmake/cv_bridgeConfig.cmake:113 (message)

产生这个的原因是：在Xavier中我把原来opencv4.1.1版本卸载了，重新安装了opencv3.2

重新安装了cv_bridge，在cv_bridge中找opencv的默认路径不一样，所以要修改。在哪里修改呢？

这里：/opt/ros/melodic/share/cv_bridge/cv_bridgeConfig.cmake

![8f73a1a017c1b7d681f54c67105424f9.png](https://cdn.jsdelivr.net/gh/GaoSHF/7011/blogs/202205/8f73a1a017c1b7d681f54c67105424f9.png)

PS：原因是：使用`sudo apt install ros-melodic-desktop-full`安装的ros，那么会默认安装`opencv`版本到`/usr/include`,`/usr/lib`,`/usr/share`三个目录。但是如果我们从opencv官网源码编译安装的（以最常用的opencv3.2为例）opencv会默认安装到`usr/local`下对应的三个子目录。

因此，將96行中的三個路徑更換爲``/usr/include,/usr/lib,/usr/share`

#### 4. 在使用rqt_image_view時，出現 rqt_image_view：command not found

首先確定系統中是否還有rqt

```
sudo apt-get install ros-melodic-rqt
sudo apt-get install ros-melodic-rqt-graph
sudo apt-get install ros-melodic-common-plugins
```

若出現`qt_gui_main() found no plugin matching 'XXX'問題，打開terminal

```
rm  ~/.config/ros.org/rqt_gui.ini
rqt
```

#### 5. 使用`gscam`

gscam是一个 ROS 包，最初由[Brown Robotics Lab](http://robotics.cs.brown.edu/)开发，用于通过标准[ROS Camera API](http://ros.org/wiki/camera_drivers)广播任何 基于[GStreamer](http://gstreamer.freedesktop.org/)的视频流。

依賴

>- gstreamer1.0-tools
>- libgstreamer1.0-dev
>- libgstreamer-plugins-base1.0-dev
>- libgstreamer-plugins-good1.0-dev

打开terminal

```
cd  Downloads/
git clone https://github.com/ros-drivers/gscam.git
```

创建工作空間

```
sudo mkdir catkin_ws/src
```

复制gscam到src目录下

在`catkin_ws`目录下打开terminal

```
catkin_make
cd ..
source devel/setup/bash
roslaunch  gacam v4l.launch
```

新打开一个terminal

```
rqt_image_view
```

即可看到摄像头画面

**注意事项**

```
<launch>
  <!-- This launchfile should bring up a node that broadcasts a ros image
       transport on /webcam/image_raw -->

  <arg name="DEVICE" default="/dev/video0"/>
  <!-- The GStreamer framerate needs to be an integral fraction -->
  <arg name="FPS" default="30/1"/>
  <arg name="PUBLISH_FRAME" default="false"/>
  <arg name="GST10" default="false"/>

  <node ns="v4l" name="gscam_driver_v4l" pkg="gscam" type="gscam" output="screen">
    <param name="camera_name" value="default"/>
    <param name="camera_info_url" value="package://gscam/examples/uncalibrated_parameters.ini"/>
    <param unless="$(arg GST10)" name="gscam_config" value="v4l2src device=$(arg DEVICE) ! video/x-raw-rgb,framerate=$(arg FPS) ! ffmpegcolorspace"/>
    <param if="$(arg GST10)" name="gscam_config" value="v4l2src device=$(arg DEVICE) ! video/x-raw,format=RGBx,framerate=$(arg FPS) ! ffmpegcolorspace"/>
    <param name="frame_id" value="/v4l_frame"/>
    <param name="sync_sink" value="true"/>
  </node>

  <node if="$(arg PUBLISH_FRAME)" name="v4l_transform" pkg="tf" type="static_transform_publisher" args="1 2 3 0 -3.141 0 /world /v4l_frame 10"/>
</launch>
```

在直接使用v4l.launch文件时会出现报错，主要原因总结为如下两条：

1. 在v4l.launch文件中，由于`ffmpegcolorspace`已经在gstreamer中已经重命名为videoconvert，故将其替换即可
2. 在使用`rqt_image_view`可能不会出来相机图像，将v4l.launch文件中`!video/x-raw-rgb`更改为`!video/x-raw`即可
