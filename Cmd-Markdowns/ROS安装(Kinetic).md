# ROS安装(Kinetic)

---

 1. 设置sources.list
 设置计算机以接受packages.ros.org中的软件

    `sudo sh -c'echo“deb http://packages.ros.org/ros/ubuntu $（lsb_release -sc）main”> /etc/apt/sources.list.d/ros-latest.list'`
    
 2. 设置公钥

        sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116

 3. 更新软件包索引

    sudo apt-get update && sudo apt-get upgrade -y

 4. 安装ROS Kinetic Kame

    sudo apt-get install ros-kinetic-desktop-full
安装所有额外的rqt相关的功能包

    sudo apt-get install ros-kinetic-rqt*

 5. 初始化rosdep
在使用ROS之前，您需要初始化rosdep。rosdep使您可以轻松地为要编译的源安装系统依赖项，并且需要在ROS中运行一些核心组件

    sudo rosdep init
    rosdep update

 6. 安装rosinstall
 这是安装ROS的各种功能包的程序。它是被频繁使用的有用的工具，因此务必要安装它。

    sudo apt-get install python-rosinstall

 7. 加载环境设置文件
 下面加载环境设置文件。里面定义着ROS_ROOT和ROS_PACKAGE_PATH等环境变量。

    source /opt/ros/kinetic/setup.bash

 8. 创建并初始化工作目录
ROS使用一个名为catkin的ROS专用构建系统。为了使用它，用户需要创建并初始化catkin工作目录，如下所示。除非用户创建新的工作目录，否则此设置只需设置一次

     mkdir -p ~/catkin_ws/src
    cd ~/catkin_ws/src
    catkin_init_workspace

 9. 测试安装结果
所有ROS安装已完成。要测试它是否正确安装，请关闭所有终端窗口并运行一个新的终端窗口。现在通过输入以下命令来运行roscore。

    roscore
如果运行之后像下面没有出现错误，则说明成功安装。退出是[Ctrl+c]。
![roscore.png-62.9kB](https://cdn.jsdelivr.net/gh/GaoSHF/7011/blogs/202205/roscore.png )

 **与Python兼容问题(No module catkin_dkg.package)**

    -- Using Python nosetests: /usr/bin/nosetests-2.7
    ImportError: "from catkin_pkg.package import parse_package" failed: No module named catkin_pkg.package
    Make sure that you have installed "catkin_pkg", it is up to date and on the PYTHONPATH.
    CMake Error at /opt/ros/kinetic/share/catkin/cmake/safe_execute_process.cmake:11 (message):
      execute_process(/home/wgb/anaconda2/bin/python
      "/opt/ros/kinetic/share/catkin/cmake/parse_package_xml.py"
      "/opt/ros/kinetic/share/catkin/cmake/../package.xml"
      "/home/wgb/catkin_ws/build/catkin/catkin_generated/version/package.cmake")
      returned error code 1
    Call Stack (most recent call first):
      /opt/ros/kinetic/share/catkin/cmake/catkin_package_xml.cmake:63 (safe_execute_process)
      /opt/ros/kinetic/share/catkin/cmake/all.cmake:151 (_catkin_package_xml)
      /opt/ros/kinetic/share/catkin/cmake/catkinConfig.cmake:20 (include)
      CMakeLists.txt:52 (find_package)


​    
    -- Configuring incomplete, errors occurred!
    See also "/home/wgb/catkin_ws/build/CMakeFiles/CMakeOutput.log".
    See also "/home/wgb/catkin_ws/build/CMakeFiles/CMakeError.log".
    Invoking "cmake" failed


​    
***解决办法：***
检查一下Python的版本： `wgb@wgb:~$ python -V`
检查一下catkin依赖的Python版本： `wgb@wgb:~$ dpkg -L python-catkin-pkg`
如果Python的版本和catkin依赖的版本不一样，说明Python依赖包有问题，解决办法：

    conda install setuptools
    pip install -U rosdep rosinstall_generator wstool rosinstall six vcstools
[1]: http://static.zybuluo.com/GaoSHF/eelpl33z7rar72cpe8pgox08/roscore.png

 