# ros::spin()和ros::spinOnce()区别与详解

---
## 函数意义
两者为ROS消息回调处理函数,同行都会出现在ROS的主函数中,程序需要不断调用ros::spin()或ros::spinOnce();
两者区别在于前者调用后不会再返回,即主函数到此就不往下在调用后还可以继续执行之后的程序.
在ROS消息发布订阅机制中,程序中写了相关的消息订阅函数,那么在程序执行的过程中,除主程序以外,ROS还会自动在后台按照规定的格式,接受订阅的消息,但是所接收到的消息并不是立刻就被处理,而是必须要等到ros::spin()或ros::spinOnce()执行的时候才被调用,这就是消息回调函数的原理.
## 区别
如上所述,ros::spin()在调用后不会再返回,即程序执行到此不在往下执行,而ros::spinOnce()再调用后还可以继续执行之后的程序

由函数名可以看出,一个是直接调用;另一个是只调用一次,如果还想再调用,就需要加上循环
需要注意,ros::spin()函数一般不会出现在循环中,因为程序执行到spin()后就不调用其他语句了,也就是说该循环没有任何意义,还有就是spin()函数后面一定不能有其他语句(return 0除外),有也不会执行. ros::spinOnce()的用法相对来说很灵活,但是往往需要考虑调用消息的时机,调用频率,以及消息池的大小,这些需要根据时机情况协调好,不然会造成数据丢包或者延迟的错误.

## 常见使用方法
需要强调的是,如果写了消息订阅函数,必须在相应的位置加上ros::spin()或者ros::sppinOnce()函数,不然永远得不到另一边发出的数据或者消息.

### **ros::spin()函数用起来比较简单,一般都在主程序的最后,加入该语句即可.**
**发送端:**
```C++
# include "ros/ros.h"
# include "std_msgs/String.h"
# include <stream>

int main(int argc ,char **argv)
{
    ros::int(argc, argv,"talker");
    ros::NodeHandle n;
    ros::Publisher chatter_pub = n.advertise<std_msgs::String> ("chatter",100);
    ros::Rate loop_rate(10);
    
    int count = 0;
    while (ros::ok())
    {
        std_msgs::String msg;
        std::stringstream ss;
        ss<<"hello world"<<count;
        msg.data = ss.str();
        ROS_INFO("%s",msg.data.c_str());
        /**
          *向 Topic::chatter发送消息,发送频率为10Hz(1秒发10次);消息池最大容量1000.
          */
        chatter_pub.publish(msg);
        
        loop_rate.sleep();
        ++count;
    }
        return 0;
}
```

**接收端**
```C++
#include "ros/ros.h"
#include "std_msgs/String.h"
 
void chatterCallback(const std_msgs::String::ConstPtr& msg)
{
    ROS_INFO("I heard: [%s]", msg->data.c_str());
}
 
int main(int argc, char **argv)
{
    ros::init(argc, argv, "listener");
    ros::NodeHandle n;
    ros::Subscriber sub = n.subscribe("chatter", 1000, chatterCallback);
 
    /**
     * ros::spin() 将会进入循环， 一直调用回调函数chatterCallback(),每次调用1000个数据。
     * 当用户输入Ctrl+C或者ROS主进程关闭时退出，
     */
    ros::spin();
    return 0;
}
```

### **对于ros::spinOnce()的使用，虽说比ros::spin()更自由，可以出现在程序的各个部位，但是需要注意的因素也更多。比如：**

 1. **对于有些传输特别快的消息，尤其需要注意合理控制消息池大小和ros::spinOnce()执行频率;比如消息送达频率为10Hz, ros::spinOnce()的调用频率为5Hz，那么消息池的大小就一定要大于2，才能保证数据不丢失，无延迟。**
 
```C++
/**接收端**/<br>#include "ros/ros.h"
#include "std_msgs/String.h"
  
void chatterCallback(const std_msgs::String::ConstPtr& msg)
{
    /*...TODO...*/ 
}
  
int main(int argc, char **argv)
{
    ros::init(argc, argv, "listener");
    ros::NodeHandle n;
    ros::Subscriber sub = n.subscribe("chatter", 2, chatterCallback);
  
    ros::Rate loop_rate(5);
    while (ros::ok())
    {
        /*...TODO...*/ 
 
        ros::spinOnce();
        loop_rate.sleep();
    }
    return 0;
}
```

2.**ros::spinOnce()用法很灵活，也很广泛，具体情况需要具体分析。但是对于用户自定义的周期性的函数，最好和ros::spinOnce并列执行，不太建议放在回调函数中**
```C++
/*...TODO...*/
ros::Rate loop_rate(100);
  
while (ros::ok())
{
    /*...TODO...*/
    user_handle_events_timeout(...);
 
    ros::spinOnce();                 
    loop_rate.sleep();
}

```