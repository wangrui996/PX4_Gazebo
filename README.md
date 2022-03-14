# PX4_Gazebo  

## PX4 + Gazebo仿真环境配置  

[重要参考](https://zhuanlan.zhihu.com/p/91329291)  


主要参考其中编译px4的依赖工具


安装交叉编译工具  

sudo apt-get install arm-none-eabi-gcc --version



* 1.新建一个工作区 
```shell   
mkdir -p PX4/src    
cd PX4/src  
catkin_init_workspace  
git clone https://github.com/PX4/Firmware.git    
```
* 2.下载源码  
```shell
cd PX4/src  
//递归到子目录
cd Firmware/  
git checkout -b v1.12.0 //与自己实机版本一致  
git submodule update --init --recursive  
//编译  


```

## 仿真设置  

1.下载模型models，放在~/.gazebo/models路径下
2.环境变量设置参考台式机.bashrc文件  
3.将上面的models中双目相机和imu模型放在~/.gazebo/models文件夹下  
4.将上面的world文件放在Firmware下的sitl_gazbeo下的world中  
5.将上面的models中双目相机无人机放在Firmware下的sitl_gazbeo下的models下  
6.将launch文件复制到Firmware的launch文件夹  

## 报错  

1. 上述设置好以后，启动launch文件一直报错，使用gazbeo命令单独打开world文件没有问题  就修改了自带了mavros_posix_sitl.launch使用双目的无人机模型，报错如下：
libcurl: (51) SSL: no alternative certificate subject name matches target host name 'api.ignitionfuel.org'  

**解决方案**  
将~/.ignition/fuel/config.yaml中的 https://api.ignitionfuel.org 改为 https://api.ignitionrobotics.org即可。


**2.新的带双目相机和IMU的无人机模型，可以在原iris文件夹下的模型的基础上添加双目相机和IMU**    

3.



## 获取Gazebo中机器人真是位姿  

调用gazebo/get_model_state服务，通过机器人的名字直接获得真实位姿信息，并以pose发布
#include"ros/ros.h"
#include"iostream"
#include"gazebo_msgs/GetModelState.h"
#include"geometry_msgs/PoseStamped.h"
using namespace std;

int main(int argc,char**argv)
{
    
    ros::init(argc, argv, "true_pose_publisher");
    ros::NodeHandle n("~");
    ros::Rate loop_rate(1);
    //获取机器人名字
    string robot_name;
    n.param<std::string>("robot_name", robot_name, "tb3_0");
    cout<<robot_name<<endl;
    //用于发布机器人真实坐标
    ros::Publisher pose_publisher=n.advertise<geometry_msgs::PoseStamped>("true_pose",10);
    //用于获得真实坐标
    ros::ServiceClient client=n.serviceClient<gazebo_msgs::GetModelState>("/gazebo/get_model_state");
    int seq=0;//发布序号
    while(ros::ok())
    {
    
        gazebo_msgs::GetModelState srv;
        srv.request.model_name = robot_name; //指定要获取的机器人在gazebo中的名字；
        if (client.call(srv))
        {
    
            geometry_msgs::PoseStamped pose;
            pose.header.frame_id="gazebo";
            pose.header.seq=seq++;
            pose.header.stamp=ros::Time::now();
            pose.pose=srv.response.pose;
            pose_publisher.publish(pose);
        }
        else
        {
    
            ROS_ERROR("Failed to call service /gazebo/get_model_state");
        }
        loop_rate.sleep();
    }

    return 0;
}


