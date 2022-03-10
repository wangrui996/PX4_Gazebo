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

*3.




