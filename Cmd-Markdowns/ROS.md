#ROS

---
## 创建工作空间

 1. home下创建catkin_ws文件夹
 2. 文件夹下创建src文件夹
 3. 在src下初始化文件夹`catkin_init_workspace`
 4. 在catkin_ws下`catkin_make`(catkin_make都是在catkin_ws下进行）
 5. 设置环境变量`source devel/setup.bash`(当前环境变量）
 6. 设置系统环境变量`vi ~/.bashrc`
 7. 文件最后添加`source ~/catkin_ws/devel/setup.bash`
 8. 上述配置生效`source ~/.bashrc`
 9. 打印当前环境变量`echo $echo $ROS_PACKAGE_PATH`（两个路径/home/gao/catkin_ws/src:/opt/ros/kinetic/share）  


## 创建功能包

 1. 在src文件夹下进入terminal
 2. `catkin_create_pkg learning_communication std_msgs rospy roscpp`创建功能包
 3. 编译功能包`catkin_make`在catkin_ws下

    **同一个工作空间中，不允许存在同名功能包
    不同工作空间中，允许存在同名功能包**

## 通信编程

 1. 实现一个发布者learning_communication文件夹下src中创建talker.cpp   listener.cpp
 2. 编译代码添加依赖库
    1.CMakeLists.txt中Build下添加
        `add_executable(talker src/talker.cpp)
         target_link_libraries(talker ${catkin_LIBRARIES})
        add_executable(listener src/listener.cpp)
         target_link_libraries(listener ${catkin_LIBRARIES})`
    
 3. 编译catkin_ws下进入终端`catkin_make`

## 运行可执行代码
 4. 启动ROS  `roscore`直接在终端下
 5. 启动talker打开终端输入`rosrun learning_communication talker`
 6. 启动listener打开新终端输入` rosrun learning_communication listener`
 7. 通信建立
![talker_listener.png-151.1kB](https://cdn.jsdelivr.net/gh/GaoSHF/7011/blogs/202205/talker_listener.png )
 8. 话题建立
    1. learning_communication文件下建立msg文件夹添加文件Person.msg
    2. package.xml中添加功能包

   ` <build_depend>message_generation</build_depend>
  <exec_depend>message_generation</exec_depend>`
    3. CMakeLists.txt中添加编译选项

    `find_package(catkin REQUIRED COMPONENTS  ...  message_generation)`

 Declare ROS messages, services and actions中添加
    `add_message_files(FILES Person.msg)    
    generate_messages(DEPENDENCIES std_msgs)`
catkin specific configuration 中添加
`catkin_package(
CATKIN_DEPENDS  roscpp rospy std_msgs message_runtime
)`

 6. 编译
    catkin下进入terminal`catkin_make`
 7. 根目录下进入terminal`rosmsg show Person`

## 服务编程
## 动作编程
## 分布式通信

 1. 设置IP地址，确保底层链路的连通
     远端IP地址查询：ifconfig
     本地IP地址查询：ipconfig
 2. 把对方IP地址分别设置到host文件中
    远端：
    sudo vi /etc/hosts
    [sudo] password for robot:
    本地IP   主机名（例如：gsf-pc）
    ping 设置的ip
    本地:上述同样的操作

## Launch文件

    <launch>
        <node pkg="turtlesim" name="sim1" type="turtelsim_node'/>
        <node pkg="turtlesim" name="sim1" type="turtelsim_node'/>

launch文件中的根元素采用`<launch>`标签定义
![launch.png-175.9kB](https://cdn.jsdelivr.net/gh/GaoSHF/7011/blogs/202205/launch.png)

## 粒子滤波定位(particle filter loa)

## 自适应蒙特卡洛定位(Adaptive Monte Carlo Localization)
