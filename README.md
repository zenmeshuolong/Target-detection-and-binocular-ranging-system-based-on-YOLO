# Target detection and binocular ranging system based on YOLO
## 目标检测
### 软件实现
   软件实现可以直接利用软件来直接实现YOLO的目标检测，同样可以用于后面硬件检测的验证。
   
   在软件环境下实现YOLOv2首先需要打开darknet-master文件夹，此文件夹中已经包含了yolov2.weights文件，此文件为预训练的权重文件。
   
   然后需要提供一个Linux环境，可在虚拟机中安装Linux系统或在windows系统中安装Linux模拟环境的软件，并成功配置相应的所需的环境。
   
   第三，在Linux环境下cd到darknet-master目录，输入make命令来运行其中的Makefile文件。
   
   第四，运行./darknet detect cfg/yolov2.cfg yolov2.weights data/dog.jpg代码将会弹出如图所示的检测完成图片。
   
### PYNQ-Z2实现
   利用PYNQ-Z2的实现可以实现在硬件层面的对CNN过程的加速。
   
   首先，实现PYNQ-Z2的YOLOv2的目标检测过程需要完成如下硬件准备。准备一块PYNQ-Z2的开发板，SD卡，一条可以用于数据传输的USB数据线，网线。
   
   第二，实现硬件的启动。将SD卡中烧写官方的PYNQ-Z2的镜像，PYNQ-Z2插入SD卡，并选择SD启动，打开开关并完成启动（具体PYNQ-Z2上手请查找更详细相关资料）。
   
   第三，实现VOLOv2的cnn加速IP。在vivado HLS中建立新工程，并选择xc7z020clg400-1作为目标板。将hls文件夹中的文件拷贝到与工程目录文件夹下。添加cnn.cpp和cnn.h到Source中，添加其他文件及labels文件夹到Test Bench中。添加YOLO2_FPGA作为顶层文件。完成Run C Simulation的C仿真，完成Run C Synthesis的C综合，导出IP。
   
   第四，实现YOLOv2的Block Design设计。在vivado中创建新的工程，选择为RTL Project，并选择xc7z020clg400-1作为目标板。在MANAGER→Settings中进行IP的添加，选择第三步生成的IP。建立新的Block Design并添加vivado文件夹下的配置文件pynqyolo.tcl以及约束文件PYNQ-Z2.xdc。并完成Block Design设计如下图所示。
   
Block design设计图

点击 Great HDL Wrapper完成Block Design的包装。最后生成bit文件。

   第五，首先YOLOv2的上板。完成硬件配置后，进入Jupyter，创建文件夹，上传Vivado生成的bit和tcl文件，和pynq文件夹中的文件。并run其中的yolov2.ipynb最后完成检测。
   
## 双目测距
### 图片获取
   第一，下载kinect开发工具KinectSDK-v1.8-Setup.exe和KinectDeveloperToolkit-v1.8.0-Setup.exe并安装。
   
   第二，连接kinect相机，系统自动安装kinect USB Audio相机驱动。
   
   第三，打开软件Developer Toolkit Browser v1.8.0 (Kinect for Windows)，同时获取RGB图片和depth图片。
   
#### 目标测距
   第一，将采集到的RGB图片和depth图片上传到板卡中。
   
   第二，运行yolov2.ipynb可实现目标物体的测距。

