# Apollo 2.0 框架及源码学习
Apollo文档可以在doc目录下找到:

 - quickstart:快速入门手册
 - demo_guide:演示指南
 - how to contribute code:贡献代码必读
 - howto:编译、运行、修改代码教程
 - specs:Apollo技术文档

## 一、软硬件框架
![apollo技术框架][1]
Apollo技术框架主要有四部分组成：
 - **Reference Vehicle Platform: 参考汽车层**，用于对汽车实现电子化的控制，其中应用到的CAN协议一般由主机厂定制，严格保密。
 - **Reference Hardware Platform:参考硬件层**,为Apollo推荐的硬件设备,如计算单元,GPS/IMU,Camera等等
 - **Open Software Platform:开放软件平台**,为Apollo的核心部分
 - **Cloud Service Platform:云服务层**,为百度可提供的数据,高精度地图等各项云服务

## 二、Modules
所有的核心模块的源码都集中在`modules`目录中，主要包含如下模块：
![modules][2]

**calibration:** 标定模块,使用前对系统进行校准和标定,包括激光雷达和摄像头,毫米波雷达与摄像头.所谓校准就是要对齐激光雷达、摄像头以及毫米波雷达获得的信息，我们知道激光雷达可以获得详细的3D信息，但是不能获得颜色信息，摄像头可以获得颜色信息，但是无法获得深度等3D信息，毫米波雷达不能获得颜色信息，但是可以获得3D信息，三者获得的信息对齐后，就可以同时获得实际环境中的3D信息和颜色信息。
**canbus:**管理CAN卡和CAN通讯,把接受到的信号传送给相应的模块,同时将Control模块的命令下发到车辆中
**common:**其他模块之外的代码都在这里
**control:**主控制模块,基于车道规划和车辆当前状态,输出转向,加速和控制信号到CAN卡
**data:**收集,存储,处理收集到的各种数据
**dreamview:**这是一个web应用,可以帮助用户随时掌握系统的输出数据,包括车道,位置车身等情况
**drivers:**包含CAN卡、激光雷达、毫米波雷达、GPS以及摄像头等相关设备的驱动程序
**e2e:** end to end，端到端深度学习，所谓e2e指的是由传感器的输入，直接决定车的行为，例如油门，刹车，方向等。也就是机器学习的算法直接学习人类司机的驾驶行为。这部分在代码中需要另外下载，学习的数据主要来源于传感器的原始数据，包括图像、激光雷达、雷达等。end-to-end输入以图像为主。 输出是车辆的控制决策指令，如方向盘角度、加速、刹车。 连接输入输出的是深度神经网络，即通过神经网络直接生成车辆控制指令对车辆进行横向控制和纵向控制，中间没有人工参与的逻辑程序。横向控制，主要是指通过方向盘控制车身横向移动，即方向盘角度。纵向控制，是指通过油门和刹车控制车身纵向的移动，即加速、刹车等。横向模型的输出没有采用方向盘角度，而是使用要行驶的曲率（即拐弯半径的倒数）。
**elo：**百度的车辆自身定位部分，结构如下:
![elo][3]
这部分的代码也是另外下载。前向的摄像头会采集车道数据以实现更精确的定位，输出的位置信息包括车辆的x y z坐标，还有就是在百度高精度地图中的ID。
利用高精地图的自定位模块，Apollo1.5新增模块，完全独立的模块，无源码只有.so文件，单独的运行环境，固定在运行在NVIDIA Drive PX2 (PDK 4.1.4.0)Ubuntu16上.利用训练好的caffe模型来检测车道线和有精确坐标的高精地图匹配从而实现定位，依赖一个粗精度低成本的gps及摄像头这两个传感器。
**localization：**车辆定位服务,包含两种定位方式,一种是GPS和IMU,另外一种是多传感器融合,输出位置估算结构体
**map:**地图
**monitor:**次模块用于检测硬件状态以及整个系统的健康程度
**perception:**该模块用于对障碍物和红绿灯等交通信号的感知,主要依靠摄像头数据以及激光雷达和毫米波雷达的数据，当然还有高精度地图，输出3D的障碍物信息，包括方向、速度和障碍物类型，当然还有红绿灯等交通信号。模块可以将障碍物标注为机动车、非机动车、行人和其他，使用的是激光点云算法。
**planning：**根据车辆位置和车辆状态、地图、障碍物、导航信息等计算具体的车道。规划车道有两种方式，一种是提前把轨迹存入程序，然后根据车辆状态和位置提取轨迹另外一种是实时计算。
**prediction：**根据障碍物的位置、航向、速度、加速度计算障碍物的可能轨迹
**routing：**路径规划，根据地图和起点终点位置计算出具体的导航信息。
**third_party_perception：**第三方感知库
**tools：**工具模块,包括一些可视化的工具,bag包录制及回放脚本

## 三、Apollo工作的整体流程:
- 首先，用户输入目的地，routing模块就可以根据终点位置计算出具体的导航信息。
- 激光雷达、毫米波雷达和摄像头拍摄到的数据配合高精度地图,由percepting模块计算出3D的障碍物信息并识别交通标志及交通信号
- 这些数据进入perdiction模块，计算出障碍物的可能轨迹
- 结合以上信息并根据车辆定位模块localizationg提供的车辆位置由planning模块得到车辆应该走的具体车道。
- 得到车道后车辆control模块结合车辆的当前状态计算加速、刹车和方向的操作信号，此信号进入CAN卡后输出到车内，如此实现了车辆的自动驾驶。
- 在整个流程中，monitor模块会及时监测硬件及系统的健康状况，出现问题肯定就会中止驾驶过程。对于驾驶中的信息，用户可以通过web应用dreamview来查看

## 几个重要的功能的模块之间的关系如图所示(Apollo2.0)
![此处输入图片的描述][4]
由图可知,**定位**和**高精度地图**是Apollo整个软件体系的基石,在感知环节,摄像头会通过高精度地图知道交通灯所在的位置,制定ROI区域,从而减少计算量
在一个实时系统中,一个任务的调度方式大致可以分为两类:事件驱动(time-triggered)和事件驱动(event-triggered)
**时间驱动**是指该任务的出发条件是事件,通常该任务为周期性任务,在Apollo的代码中,以时间为触发条件的模块都会提供Ontimer这样的接口
**事件驱动**是指任务的触发条件为某一事件,例如看到红灯会踩刹车,在这个操作中踩刹车举动是由红灯亮
## Localization
这个模块中,Apollo提供了两种定位模式
1. RTK-based,即用GNSS+IMU的传统定位方式,该模式是属于timer-triggered,该模式下的定位应该会被周期性的调用
2. 多传感器融合(Multi-sensor Fusion Localization),利用GNSS,IMU和Lidar三者配合使用进行定位,大概过程为:通过对比Lidar采集的点云和实现建好的地图得到Lidar的定位结果,之后Lidar定位,GNSS定位,IMU三者用卡尔曼滤波做融合,如下图所示
![此处输入图片的描述][5]

## Perception
![此处输入图片的描述][6]
Perception模块有两个重要的模块,即**3D障碍物感知**和**交通灯感知**
摄像头只负责感知交通灯的状态,不负责其他障碍物的状态
3D障碍物感知部分使用卡尔曼滤波,融合了lidar和radar检测的结果,正常情况下,视觉检测障碍物也是很重要的一部分
在Apollo 2.0中没有做视觉融合
百度自定位模块如下图所示:
![v2-3ea8301a48befa27c8735dd621bbfc0c_r.jpg-69.7kB][7]

## Planning
Planning 模块是 timer-triggered的, 以固定的频率被调用，该模块提供了两种方案：
>1. RTK replay planner (since Apollo 1.0)
>2. EM planner (since Apollo 1.5)

规划车道有两种方式,一种是提前把轨迹存入程序,然后根据车辆状态和位置提取轨迹,另外一种是实时计算

Apollo最新开发的是lattice planner决策算法:
>lattice Planer是基于斯坦福大学在DARPA挑战赛中使用过的一种路径规划算法,优势在于更好的理解交通状况,降低规划的复杂性,告诉情况表现优异,可参考[Optimal Trajectory Generation for Dynamic Street Scenarios in a Frene´t Frame][8]
## Control & Canbus
![v2-b5befcd8a3f1c478ce377281703d4e0c_r.jpg-94.8kB][9]
如Control控制模块提供了三个重要的数据接口:OnPad,OnMonitor和OnTimer,其中OnPad 和OnMonitor用于仿真和人机交互,OnTimer用于周期性的指令传递最后,规划模块需要知道位置(定位:我在哪里)以及当前自主车辆信息(底盘:什么是我的状态)
控制模块将决策模块生成的轨迹作为输入,计算出底层所需要的具体指令,比如加速,减速,转向等等,并通过OnTimer接口,周期性的向Canbus传递质控指令.Canbus一旦接收到具体的指令之后就会调用对应的函数执行,与此同时,Canbus也会周期性的向Control模块发送汽车当前的信息,在决策模块中需要知道`位置(定位:我在哪里)以及当前自主车辆信息(底盘:什么是我的状态)`


  [1]: http://static.zybuluo.com/GaoSHF/ll0dvsbf5j6r9uhckpfnq71g/v2-48ffe6eaa05e29810ed2b56bc626cd1a_r.jpg
  [2]: http://static.zybuluo.com/GaoSHF/rg0b2r9pfglbf2lke8npo0v6/Modules.png
  [3]: http://static.zybuluo.com/GaoSHF/qi5p7ri3nnohiux0qer9y13l/.jpg
  [4]: https://media.githubusercontent.com/media/ApolloAuto/apollo/master/docs/specs/images/Apollo_2_0_Software_Arch.png
  [5]: http://static.zybuluo.com/GaoSHF/0ghf4fkko8qdpodij0zn5xnp/v2-ddff4c82cacff6301481c85a8989d609_r.jpg
  [6]: http://static.zybuluo.com/GaoSHF/gyckvf7clljfq0u3wd0eyddd/v2-cf6d199a7cf3d7d6c460e1ef7051db65_r.jpg
  [7]: http://static.zybuluo.com/GaoSHF/slpz67965u8s9qnene7qsjol/v2-3ea8301a48befa27c8735dd621bbfc0c_r.jpg
  [8]: http://link.zhihu.com/?target=https://pdfs.semanticscholar.org/0e4c/282471fda509e8ec3edd555e32759fedf4d7.pdf
  [9]: http://static.zybuluo.com/GaoSHF/a2b0egty4nl9iq9jzcwdkv01/v2-b5befcd8a3f1c478ce377281703d4e0c_r.jpg