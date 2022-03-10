# PX4_Gazebo  

## PX4 + Gazebo仿真环境配置  


### 安装编译px4的工具  

```shell
sudo add-apt-repository ppa:george-edison55/cmake-3.x -y
sudo apt-get update
# 必备软件
sudo apt-get install python-argparse git-core wget zip \
    python-empy qtcreator cmake build-essential genromfs -y
# 仿真工具
sudo add-apt-repository ppa:openjdk-r/ppa
sudo apt-get update
sudo apt-get install openjdk-8-jre
sudo apt-get install ant protobuf-compiler libeigen3-dev libopencv-dev openjdk-8-jdk openjdk-8-jre clang-3.5 lldb-3.5 -y
```


* 1.新建一个工作区    
mkdir -p PX4/src    
cd PX4/src  
catkin_init_workspace  
git clone https://github.com/PX4/Firmware.git    

* 2.下载源码  
cd PX4/src  
git clone https://github.com/PX4/Firmware.git  

//递归到子目录
cd Firmware/  
git submodule update --init --recursive  

*3.
https://blog.csdn.net/yanwumuxi/article/details/80097294?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase  
https://www.ncnynl.com/archives/201709/2013.html
