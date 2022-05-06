#ROS安装以及学习中的问题
---
## ROS安装：
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

## ros  catkin_make出错
`Invoking "make -j4 -l4" failed, ImportError: No module named 'em'`
![roscmake_err.png-122.3kB](https://cdn.jsdelivr.net/gh/GaoSHF/7011/blogs/202205/roscmake_err.png)

解决办法：
安装`empy`库：`pip install empy`

## ros日志删除

    rosclean pure

## 新导入的功能包，在terminal中利用roslauch无法找到
`[xxxx.launch] is neither a launch file in package [xxxx] nor is [xxxx] a launch file name`
![launch0.png-17.9kB](https://cdn.jsdelivr.net/gh/GaoSHF/7011/blogs/202205/launch0.png )

**解决办法：**
返回工作空间进行source即可
```
$ cd ~/catkin_ws
$ source devel/setup.bash
```

​    






[1]: http://static.zybuluo.com/GaoSHF/84elhm68pk32s0oqiiyuqoiu/roscmake_err.png
[2]: http://static.zybuluo.com/GaoSHF/drjo5n0mxeyaqnkvnlqagrgm/launch0.png