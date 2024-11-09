# Autonomous-Driving-Based-On-Deep-Learing
在智能车上实现自动行驶，红绿灯识别和避障

Realize automatic driving, traffic light recognition, and obstacle avoidance on smart cars

# 1 摘要（ABSTRACT）
本文为实现自动驾驶系统，设计并实施了三个核心功能：自动行车、避障以及红绿灯识别，并最终将这三种功能进行分级合并。系统设计阶段，结合硬件环境，主要设计了智能车行驶、数据采集及模型部署系统。数据采集系统获取相机标定板数据以及行驶信息。经过深度处理，运用计算机视觉算法，处理得到可供模型训练的数据。自动行车系统的设计采用了两种深度学习模型和两种训练任务进行模型训练，并在比较分析的基础上，结合实际运行环境，优化训练结果。避障系统借助激光雷达扫描环境数据，确定车辆四个方向范围，并根据计算结果，指导智能车避障。红绿灯识别系统在新色彩空间下划分颜色区间并提取信号灯发光部分，进一步识别颜色，根据识别颜色控制车辆行动。将上述三种功能在已实现的情况下进行整合，设计功能优先级划分，根据实际硬件环境设计单独功能的运行逻辑。并根据最终设计的结果进行智能车实际部署以实现自动驾驶功能。

(In order to realize the auto drive system, this paper designs and implements three core functions: automatic driving, obstacle avoidance and traffic light recognition. Finally, these three functions are classified and combined. In the system design phase, combined with the hardware environment, the intelligent vehicle driving, data collection, and model deployment system were mainly designed. The data acquisition system obtains camera calibration board data and driving information. After deep processing, computer vision algorithms are used to obtain data that can be used for model training. The design of the automatic driving system adopts two deep learning models and two training tasks for model training, and optimizes the training results based on comparative analysis and actual operating environment. The obstacle avoidance system uses LiDAR to scan environmental data, determine the four directional ranges of the vehicle, and guide intelligent vehicles in obstacle avoidance based on the calculation results. The traffic light recognition system divides color intervals in the new color space and extracts the illuminated parts of the signal lights, further identifying colors and controlling vehicle movement based on the identified colors. Integrate the above three functions in the already implemented situation, design priority division of functions, and design the operation logic of individual functions based on the actual hardware environment. And based on the final design results, deploy the intelligent vehicle to achieve autonomous driving function.)

# 2 研究思路与方法（Research ideas and methods）
通过搭建场景平台，设置信号灯以及障碍物探究自动驾驶解决方法。重新设计符合智能车与场景平台下的操作系统，针对采集后的数据处理方法进行了深入的实验和测试。在搭建自动行驶系统中，选择两种符合当前计算资源下的小体量深度学习模型进行不同训练任务的测试，对比分析模型训练结果，并针对在实际情况部署所产生的问题进行解决和优化。设计智能车避障系统，利用智能车前端激光雷达扫描数据进行方向区间划分，将激光扫描到的区域划分成若干个小区间，并根据划分区间的计算结果使智能车实现避障。结合实际剩余计算资源设计红绿灯识别系统，根据不同颜色数值区间的差异性进行颜色识别。当上述三种功能完成时，设定三种功能优先等级，结合实际运行环境设计自动驾驶系统。

<div align=center>
<img width="400"  src="https://github.com/user-attachments/assets/cc7c1954-4396-4905-ab40-b5c04e42adda">
<img/></div>

（Explore solutions for autonomous driving by building a scene platform, setting up traffic lights and obstacles. Redesigned an operating system that is compatible with intelligent vehicles and scene platforms, and conducted in-depth experiments and tests on the data processing methods after collection.In building an autonomous driving system, two small-scale deep learning models that are suitable for the current computing resources are selected for testing different training tasks, and the training results of the models are compared and analyzed. Solutions and optimizations are made based on the problems arising from deployment in practical situations.Design an intelligent vehicle obstacle avoidance system that uses the front-end LiDAR scanning data of the intelligent vehicle to divide the direction interval, dividing the area scanned by the laser into several small intervals, and enabling the intelligent vehicle to achieve obstacle avoidance based on the calculation results of the divided intervals.Design a traffic light recognition system based on actual remaining computing resources, and recognize colors according to the differences in numerical ranges of different colors. When the above three functions are completed, set the priority of the three functions, and design the auto drive system in combination with the actual operating environment.）

# 3 实验环境与平台(Experimental environment and platform)
由于希望在实车平台上验证前文所训练的自动驾驶模型，以考验自动驾驶模型在实际 情况下的可靠性。为此搭建了专门面向自动驾驶工况场景的智能车平台。该平台由幻宇智 能车平台改装而来，软件控制基于机器人操作系统(Robot Operating System,ROS)。本章将 对使用到的硬件及软件部分进行介绍。之后在模拟场景下进行试验，以验证自动驾驶模型 的实际性能。

（Due to the desire to validate the previously trained autonomous driving model on a real vehicle platform, in order to test the reliability of the autonomous driving model in practical situations. We have built an intelligent vehicle platform specifically designed for autonomous driving scenarios. This platform is modified from the Huanyu Intelligent Vehicle Platform, and its software control is based on the Robot Operating System (ROS). This chapter will introduce the hardware and software components used. Afterwards, experiments will be conducted in simulated scenarios to verify the actual performance of the autonomous driving model.）

## 3.1 场景平台的搭建(Construction of Scene Platform)
为了实现自动驾驶的安全性和可靠性，大量的数据收集和测试是必不可少的。然而，在真实道路上进行数据收集和测试存在一些问题，例如高昂的成本、时间限制、安全风险等。因此，建立自动驾驶模拟环境成为了一种有效的解决方案。自动驾驶模拟环境是通过仿真技术来模拟现实世界中的各种场景和情况，以进行自动驾驶系统的开发和测试。在这个模拟环境中，可以利用虚拟的道路网络、车辆模型和传感器模拟真实道路上的交通情况。通过收集和分析模拟环境中的数据，可以评估自动驾驶系统在不同情况下的性能和稳定性。首先，进行自动驾驶模拟环境道路的设计，实现基本的道路行驶，智能车根据当前路况做出前进，后退，左转与右转动作

<div align=center>
<img width="400" alt="截屏2024-11-09 11 34 30" src="https://github.com/user-attachments/assets/ed445929-7c38-4b3d-824e-ddb650c4f61d">
<img/></div>

(In order to achieve the safety and reliability of autonomous driving, extensive data collection and testing are essential. However, there are some issues with data collection and testing on real roads, such as high costs, time constraints, safety risks, etc. Therefore, establishing an autonomous driving simulation environment has become an effective solution. The automatic driving simulation environment simulates various scenes and situations in the real world through simulation technology to develop and test the auto drive system. In this simulation environment, virtual road networks, vehicle models, and sensors can be used to simulate traffic conditions on real roads. By collecting and analyzing the data in the simulation environment, the performance and stability of the auto drive system in different situations can be evaluated.Firstly, the design of an autonomous driving simulation environment road is carried out to achieve basic road driving. The intelligent vehicle makes forward, backward, left turn, and right turn actions based on the current road conditions.)

所设计的自动驾驶模拟环境在宽度为420厘米长度460厘米的矩形上进行车道设计。为了进一步保证模拟实际行车情况所涉及到的路况，设计车道宽度为35厘米对应1.5倍车身宽度。并从两条黑色标记点开始以逆时针顺序运行。共有6个直角弯，1个回头弯，2个折角弯和7条直道组成。

<div align=center>
<img width="353" alt="截屏2024-11-09 11 03 18" src="https://github.com/user-attachments/assets/e79cd77c-63ba-4d45-8718-8e7cbd2095d6">
<img/></div>

(The designed autonomous driving simulation environment performs lane design on a rectangle with a width of 420 centimeters and a length of 460 centimeters. In order to further ensure the road conditions involved in simulating actual driving situations, the designed lane width is 35 centimeters, corresponding to 1.5 times the width of the vehicle body. And run in counterclockwise order starting from the two black marked points. There are a total of 6 right angle bends, 1 turn back bend, 2 corner bends, and 7 straight paths.)

## 3.2 智能车平台(Intelligent car platform)
自动驾驶智能车平台的搭建基于幻宇自研的一款配有麦克纳姆轮的四驱遥控车，其上安装的硬件系统结构如图11所示。其中主板选用NVIDIA Jetson B01开发版，该开发版采用ARM架构。搭载了四核心Cortex-A57处理器，具有128核Maxwell GPU及4GB LPCDDR内存。支持多种AI框架与算法，兼顾了小体型和大算力。下控制器选用幻宇自研主板，负责以串口通信的方式接收上方主板信号，并以PWM波的形式通过主板芯片传递给下游的四个编码电机，借此控制智能车底盘运动。直流12V编码驱动电机为智能车运行提供强大驱动力。对于感知模块，考虑到后期数据集所需数据的形式，通过使用板载的传感器方案，其中包括一个奥比中光所研发的具有感知景深的双目摄像头和一个用于辅助矫正的自带IMU以及思岚 A1 激光雷达。传感模块可给出车身纵向速度和角速度并将这两种状态量传递给主板。传感器模块可以做到10HZ的测量频率。其中12V锂电池通过下主板对上主板进行供电，电机主要由下主板的供电端口进行12V供电。车辆控制算法的软件编写基于ROS系统，其中车辆控制端主要通过geometry_msgs功能包和sensor_msgs功能包。控制模块的运算频率设定为10Hz。即在实际过程中智能车对前方路况的判断并做出行动的时间可以控制在100ms内，体现出自动驾驶算法的实时性。

<div align=center>
<img width="400" alt="截屏2024-11-09 13 17 58" src="https://github.com/user-attachments/assets/3ddacd6f-520e-435d-b267-4120e34751e5">
<img/></div>
<div align=center>
<img width="400" alt="截屏2024-11-09 11 17 12" src="https://github.com/user-attachments/assets/703ce7c2-3c8d-4856-b848-936718ca24b1">
<img/></div>

(The construction of the autonomous driving intelligent car platform is based on a four-wheel drive remote control car equipped with Mecanum wheels developed by Huanyu. The hardware system structure installed on it is shown in Figure 11. The motherboard uses NVIDIA Jetson B01 development version, which adopts ARM architecture. Equipped with a quad core Cortex-A57 processor, featuring a 128 core Maxwell GPU and 4GB LPCDDR memory. Supports multiple AI frameworks and algorithms, balancing small size and high computing power. The lower controller uses a self-developed motherboard from Huanyu, which is responsible for receiving signals from the upper motherboard through serial communication and transmitting them in the form of PWM waves to the four encoding motors downstream through the motherboard chip, thereby controlling the motion of the intelligent vehicle chassis. The DC 12V encoded drive motor provides powerful driving force for the operation of intelligent vehicles. For the perception module, considering the form of data required for the later dataset, an onboard sensor scheme is used, which includes a binocular camera with perception depth developed by Obi Zhongguang, a built-in IMU for auxiliary correction, and a Silan A1 laser radar. The sensing module can provide the longitudinal velocity and angular velocity of the vehicle body and transmit these two state variables to the motherboard. The sensor module can achieve a measurement frequency of 10Hz. The 12V lithium battery supplies power to the upper motherboard through the lower motherboard, while the motor is mainly powered by the 12V port on the lower motherboard. The software development of the vehicle control algorithm is based on the ROS system, where the vehicle control end mainly uses the geometriy_msgs and sensor-msgs function packages. The operating frequency of the control module is set to 10Hz. In the actual process, the time for intelligent vehicles to judge and take action on the road conditions ahead can be controlled within 100ms, reflecting the real-time performance of autonomous driving algorithms.)

## 3.3 计算平台（Computing platform）
由于本次实验的数据集主要由图像数据组成，其特点是数据量大、数据具有高维性或结构性。这对计算机的中央处理器以及图形处理器提出了较高的要求。为了满足对计算资源的需求，我们选择了两台高性能计算机进行数据处理与模型训练。
首先，选用了一台搭载M2 PRO芯片的计算机，该机具有10核中央处理器和16核图形处理器，以及16GB四通道内存。这台计算机主要负责整体框架搭建，实验尝试性验证以及部分训练任务。M2 PRO芯片的强大计算能力和高效的内存管理，使得它能够快速处理大量的图像数据，提高实验效率。其次，选用了一台惠普Z600工作站，该工作站搭载了双核至强E5640中央处理器，双路DDR4 32GB内存和1TB固态硬盘，以及一块特斯拉丽台p100显卡，该卡拥有24GB的显存。这台工作站主要用于处理大规模的多维数据和模型训练的任务。双中央处理器和大容量的内存使工作站能够连续处理大规模的图像数据，1TB的固态硬盘则提供了足够的空间，保证了数据处理与模型训练的顺利进行，进一步保证了计算的可靠性。
 
# 4 车辆操控与数据处理
## 4.1 操控系统设计
数据采集分为标定图像数据采和行驶图像与行驶速度的采集。由于不同部分对数据集的要求不同，所以按每种部分的要求分别设计智能车操控系统。并在次基础上设计了行驶轨迹，行车录像等功能，操作方式上主要尝试了电脑端操控与PS2手柄端操控，并最终找到最优的解决方案。
### 4.1.1 操控系统的分析与选取
在本次试验基于机器人操作系统ROS上，采用Python语言对智能车的操作进行设计。分别设计了电脑键盘端智能车操控系统与PS2手柄端智能车操作系统。从而实现智能车前进，后退，左转，右转。并在此基础上实现相对应的数据采集功能。
在电脑端，通过电脑端键盘中“w”,“s”,“a”,“d”键位实现对智能车的前进，后退，左转与右转。在实际搭建过程中使用到了pynput,pygame,curses等库进行测试，由于在实际运行中部分库只能在一个刷新频率下读取一个键盘输入值，因此在一个刷新周期中智能车只能进行一个动作，从而导致智能车运行卡顿不畅。为了保证智能车动作的连贯性，放弃电脑端的设计进而转向PS2手柄端的智能车操控系统。PS2手柄端正面共有13个键位。其中三个为内置功能，其余为可开发键位。为了更好的反应智能车实际的运行情况，选取两个遥感键位作为实现智能车前进，后退，左转，右转的真实运行情况。其余键位根据实际的数据采集需要进行合理的开发。首先确定每个键位在ROS系统中所代表的位置在哪里。根据ROS中的节点joy_node进行打印.
<div align=center>
<img width="400" alt="截屏2024-11-09 13 24 53" src="https://github.com/user-attachments/assets/217009c2-99e8-4a57-8308-fd4ddddab34d">
<img/></div>
键位信息设计智能车的操控系统,实现智能车的前进，后退左转与右转以及智能车的三种运行速度的切换。在运行开始前,设置初始基础速度为0（base_speed=0）,遥感手柄的axes[7]左右两个按键为智能车实现三种运行速度的切换按键，左按键用于加速调节,右按键用于减速调节，且调节的最大值为3。智能车的前进力度与转弯力度由遥感手柄的axes[1]与axes[2]的两个参数linear_speed和angular_speed控制，最后通过速度综合控制函数得到最终行驶的线速度与角速度。

### 4.1.2 数据采集系统设计
#### 4.1.2.1 相机标定数据采集系统
标定图像对自动驾驶系统的精确感知和决策具有关键作用。通过打开roscore内核，joy_node节点以及基础启动环境进行相机标定数据采集系统的设计。通过建立ROS系统图像传输通路，订阅相机节点和操作手柄节点，设定并选取PS2操作手柄按键buttons[3]对图像拍照进行触发和保存。并在新的一轮的数据采集前清除掉上一次采集的数据保证数据不发生混乱提高数据在相同环境下的一致性。

#### 4.1.2.2 行车数据采集系统
在实现自动驾驶过程中，图像是自动驾驶系统中最主要的感知信息来源。依托于智能车运行操作系统，进行智能车行车数据采集系统的实现。首先进行图像数据通路在ROS系统中的搭建，通过ROS系统中相机节点与操作系统节点，设定并选取PS2操作手柄按键axes[6]对行车图像数据进行采集，其中左按键为开始行车数据采集，右按键为结束行车数据采集并保存，与此同时发生的还有行车时间、行车线速度和角速度的数据采集。并在新的一轮数据采集之前清除掉上一次采集的数据。

#### 4.1.2.3 行车轨迹采集系统
不管是在行车数据采集时还是在进行模型的部署，都需要对智能车行驶的轨迹进行记录。由于ROS操作系统自带有智能车方位记录的功能，在实现智能车方位记录的过程中需要对智能车所在的坐标系进行转换，通过订阅Obom节点获得智能车当前的坐标以及四元组方向，四元组经过欧拉角的转换获得智能车相对于第一次位置所朝向的角度。通过数据的整合将智能车当前X轴、Y轴、Z轴以及智能车朝向角度W存储在文本文件中。

### 4.1.3 模型应用系统设计
当模型训练结束时，不仅要对模型进行测试集与验证集上的测试，还需要对模型进行实际应用的测试。通过实际应用测试得到在具体情况下此模型所产生的问题和运行效果，为了建立起模型与智能车之间的关系，设计模型应用系统为以后智能车自动驾驶整体系统的设计与实现奠定基础。在此系统中需要通过建立起图像数据通路，当系统开始运行获得图像数据时，需要使用与图像数据处理相同的处理方法，之后通过保存好的模型获得不同训练任务下的结果，根据不同训练任务下的结果进一步处理得到模型预测的操控数据，并在最后发布到智能车行驶的节点上，驱动智能车在不同情况下做出合理的运行行为，更加直观的反应出当前模型的训练效果。

## 4.2 数据采集
### 4.2.1 标定数据采集
本次相机标定采用张正友相机标定法，通过黑白棋盘格对相机进行标定。通过calib.io网站对标定所需的黑白棋盘格标定板进行设计。标定板由8行12列的小方格组成，每个小方格的边长大小为20mm。

在对标定数据采集的过程中，将标定板与摄像头放置在多个不同的位置和角度，以确保从各个视角对相机进行全面的标定。提高标定的准确性与鲁邦性，避免引入新的误差。通过相机标定数据操控系统的操作，共拍摄了13张标定图片。




### 4.2.2 行车数据采集
在设计好的自动驾驶环境中进行数据采集，运行行车数据采集系统。通过PS2手柄的操控，从起始点以逆时针作为正方向进行智能车行驶的线速度与角速度数据以及摄像头数据采集。为了进一步保障采集数据的平衡化以起始点顺时针方向再一次进行数据采集。重复操作，直到采集到足够多的数据后停止并将需要的数据进行导出。

为了进一步提高数据采集量，将行车数据采集系统中的行车图像数据通过改变订阅节点进行压缩，并缩短采集周期从而提高采集频率。最终通过行车数据采集系统共采集到11560张行车数据图片，每一张行车数据图片以采集时间进行命名。行车过程中的线速度与角速度以采集时间，线速度，角速度的格式存储。且三列数据均为浮点数据类型。


## 4.3 数据处理
### 4.3.1 标定数据处理
首先对采集的标定数据进行是否为空的判断，保证采集的标定数据集存在且路径正确，由于之后进行角点的寻找基于图像强度变化且不依赖于颜色信息，所以对图像进行灰度化处理，进一步突出图像形状、纹理和图案特征。其次根据标定板的行数和列数进行内脚点的确定，根据此次所使用的标定板的行数与列数分别为8行12列，所以得到内脚点的行数为7行11列，并建立起图像坐标系点。最后通过图像交点的寻找得到相机内部参数矩阵。

通过张正友黑白棋盘标定法得到相机内部参数，分别为相机内部参数矩阵，相机畸变量，相机旋转量和相机平移量。根据相机标定计算得到的结果进行采集图片的畸变矫正。如图所示分别为采集得到的原图像和畸变矫正后的图像。

### 4.3.2 行车数据处理
#### 4.3.2.1 行车图像处理
##### （1）行车图像数据分析
图像数据主要由智能车前的摄像头进行图像的捕捉，通常捕捉到的图像大小为480*640像素，并包含三个颜色通道（红色、绿色、蓝色）。如图25所示为智能车前摄像头捕捉到的行驶图像，图像中主要包含车道情况，周围光线以及部分周围环境。



通过预览行车数据采集系统所捕捉到的原始图像，发现采集的所有图像数据中主要存在三个问题分别为光线强度、路面纹理以及实验场地无关特征的加入。分别如图26,图27和图28所示。分别为采取原始图像中的光线强度，路面纹理以及无关场地特征。

##### （2）行车图像数据增强处理
针对以上在数据集中发现的问题进行相关的图像数据处理，使得图像数据集特征更加突出，提高此后模型训练的精确度。由于车道与车道周围颜色的对比度明显，进一步突出图像当前的特征，通过提高图像对比度进行图像数据增强处理，对比度增强的情况下与原始图像的对比.


通过增加对比度，使图像中最暗和最亮部分之间的差异变得更加明显，在原始图像与对比度处理后的图像中红色框可以看到光线强度在此发生了明显的削弱，几乎与光斑周围的颜色融为一体但还是存在并没有完全消除，蓝色框中的路面纹理由于图像对比度的增强变得愈加明显。可以清楚的看到路面纹理的走向，若以当前数据作为训练集则会对训练的模型造成一定影响，可能导致模型泛化能力降低，从而导致模型在实际部署中效果不佳。在无关特征中由于只是图像对比度的改变所以无关特征变化不大，在原始图像与对比度处理后的图像中由黄框所示。

##### （3）行车图像数据高斯化与灰度化处理
在当前情况下，为了进一步提高数据质量，减少图像中由于光线强度和路面纹理的影响，使用高斯模糊使图片减少噪声降低细节层次从而保留当前图像最主要的特征。由于当前图像分辨率较大，所以使用了较大的高斯核进行模糊化处理，高斯模糊处理后的结果如下。

由于车道在图像中占比了较大的部分所以通过高斯模糊化处理后，只是使光线强度与路面纹理发生了扩散，光线强度与路面纹理原本的轮廓并没有由于高斯模型与周围环境相融合，如图中红色框与蓝色框。通过高斯模糊使较远处的无关特征与周围环境的轮廓发生了融合，没有与原图像中无关特征的明显轮廓如图中黄色框所示。同时，通过图像的高斯模糊可以看到图像当前的主要特征如图中绿色线标注部分为车道边界线以及车道边界线的延伸。后针对以上所设计到的三种问题进行针对性处理。
为了进一步解决上述所存在的问题，使原始图像从三通道图像转变为单通道图像，在保留图像主要特征完整度的前提下进一步优化图像处理效率，降低图像存储和使用的复杂程度，通过图像灰度化实现进一步的处理。在单通道图像下，图像主要特征边缘更加清晰且由于没有其他颜色的干扰使得黄色框中无关特征与周围环境的融合程度更高，红色框中光线强度的干扰得到了进一步的扩散，由于灰度化只是颜色强度的一种改变，所以路面纹理在图像灰度化中没有得到更好的处理如图中蓝色框所示。

##### （4）行车图像数据二值化处理
在当前场景中，车道与车道周围所形成的强对比度，且光线强度与路面纹理同时处于浅色车道部分，车道周围为深色部分，二者颜色差异度较大，可以通过图像分割进行真实世界与图像世界的转换。由于车道与车道周围颜色结构相对单一故使用图像二值化方法进行图像分割。针对此选用了全局阈值法，自适应阈值法与OTSU二值化法分别实现对当前图像的分割。

由于场景平台不同方位光照强度的不统一情况，故在全局阈值法中进行了图像灰度的平均值计算，使用计算得到的灰度平均值作为全局阈值。在全局阈值法处理下相比于其他两种处理方法图像较远处的无关特征复杂度明显降低，其次是OTSU二值法，自适应阈值法由于分块计算，当一小块图像与周围图像灰度值较大时，将导致与灰度值较小的区域差别过大，故保留下了过多的无关特征，且在处理光线强度与路面纹理上也发生了此种问题。而其他两种算法由于是基于全局处理所以全局阈值法与OTSU二值法的效果要远远好于自适应阈值法。
针对全局阈值法与OTSU二值法进行数据处理的相关验证与分析，避免由于在不同实验环境下算法的不稳定性，使得图像处理方案具有泛化能力。在较强的光线下的场景环境中进行图像数据的验证。


通过全阈值法与OTSU二值法进行处理后，在全阈值法中由于光照较强导致阈值设置的较高，从而导致在全阈值法处理中图像主要特征表现不明显，如图中绿色标识。在距离智能车较近的地方全阈值法处理的结果相比于OTSU二值法处理的结果出现了较多非正确特征值，相比之下OTSU二值法保留了较多的特征信息。

##### （5）行车图像数据的联通组件分析
通过实际部署发现，数据在经过二值化处理后，仍然会存在一些处理不充分的特征，如下图39所示，即使后期通过了数据图像的裁剪，但也会导致由于在原始数据上存在难以处理的无关特征。在二值化处理后，使得赛道与赛道周围的差异性不断增大，由于此原因，故使用基于联通组件分析的方法对数据进行图像主体提取，并在图像主体提取后，根据图像主体重新制作新的数据集。从二值化以后的图像中，右上角部分（红色椭圆圈内）由于在原始图像中为周围墙壁与道路周围环境的交界处属于客观存在不可避免的无关特征，为了减小这些客观无关特征的干扰，进行联通组件分析的方法对图像主体特征进行提取，提取结果如图所示，根据提取出的图像按照原图像尺寸进行生成，最终形成图所示的新图像数据。


##### （6）行车图像数据的裁剪与缩放
最后对当前的处理结果进行图像的裁剪与缩放，在保证图像保留当前主要特征的前提下对图像无关特征进行裁剪。裁剪图像上半部分三分之一比例，从原图像480*640比例，裁剪到320*640比例，并在图像裁剪后对图像进行缩放，以提高模型训练效率与存储空间结构的优化。裁剪后的图像如图42所示，保留了当前图像数据中主要特征进一步帮助模型理解图像并使用图像进行模型部署。


最后将图像进行等比缩放，裁剪图像由原来320*640比例，等比缩放到100*200比例相比于原始图像数据在内存中缩小了11.25倍。并由后续所建立的行车数据处理系统与行车速度等数据进行打包，最终形成训练集与测试集。

##### 4.3.2.2 行车速度数据处理
行车数据速度标签主要由时间，线速度，角速度构成。在进行收集的过程中，将采集到的数据存储在文本文档中，对此后的数据预处理会造成一定阻碍，所以将文本数据转换为表格格式，方便后续在行车速度数据上的数据处理。转换后的原数据如表2所示。

表2 部分速度数据转换结果（截取部分)
|时间|	线速度|	角速度|
|:---:| :---: | :---: |
|1706265089.13|	0.002917931	|0|
|1706265094.68|	0.125	|0.231022313|
|1706265099.25|	0.00156066	|0.25|
|1706265099.28|	0.005632472	|-0.25|
|1706265189.36|	0.06124844	|0|
|1706265189.39|	0.065320253	|0|

由于线速度与角速度由正负表示方向，数值表示大小，不够直观和解释，所以对采集的线速度与角速度进行速度的分解，使展现的速度更加细化。若线速度数值为正则车辆向前行驶，反之，向后行驶。同理若角速度为正则车辆向左侧转向，反之，向右侧转向。根据上述原理，在原始数据中加入前进，后退，左转，右转四个方向的分动作标签从而更加直观的展现出动作的组成部分。如表3所示为差分后的速度标签。

表3 差分后的速度标签
|动作|标签|动作|标签|
|:---:| :---: | :---: |:---: |
|静止	|0000|	向左后行驶|	0101|
|前进	|1000|	向右后行驶|	0110|
|向左前行驶	|1010|	逆时针旋转|	0010|
|向右前行驶|1001|	顺时针旋转|	0001|
|倒车/刹车|0100|	

在速度差分后的标签表示下，使智能车当前的状态表示更加的直观，但速度差分标签在计算机存储的情况下包含所有位信息，所以对存储资源可能会造成一定的浪费，由于智能车实现的功能量大以及有限的计算资源，在可以表示当前智能车动作的情况下，进一步优化标签形式提高计算资源的使用效率。根据智能车行驶的九个行驶状态，进行行驶标签的设定。将智能车行驶分为9种变长标签类别。优化传输或存储资源的使用效率，且在智能车实际行驶过程中对前方道路的识别具有高频率的动态变化特性，相对于速度差分编码简化的行驶状态编码更能直观的反应当前智能车行驶状态，方便更好的分辨智能车在当前环境下路况识别的准确性。如表4所示为根据智能车行驶的九个状态对行驶状态标签的设定。

表4 根据运行状态划分的标签（进行了变长标签补齐）
|动作|标签|动作|标签|
|:---:| :---: | :---: |:---: |
|静止|000000001|	向左后行驶|	000100000|
|前进|000000010|	向右后行驶|	001000000|
|向左前行驶|000000100|逆时针旋转|010000000|
|向右前行驶|000001000|顺时针旋转|100000000|
|倒车/刹车|000010000|

根据运行状态划分的标签可在分类模型中使用，由于线速度与角速度的绝对值都处于0到1之间故不做过多的数据处理，线速度与角速度可在回归模型中使用。在采集到的所有数据中由于智能车无线通讯系统由于并行处理其他任务而可能产生智能车反应缓慢，对于PS2手柄所发出的指令不能及时执行，从而使智能车存在静止状态，对此，在数据采集过程中对线速度与角速度标签全为0的数据进行删除。在实际采集状态下，智能车可能与异常值产生原因相同的原因和实际客观因素的影响下造成数据采集量的不同，为了避免后期模型在训练时由于先前数据集所存在的问题造成模型的过拟合或在实际部署过程中存在模型对实际运行环境感知错误的情况，所以对数据进行均衡化处理，使各个状态标签下的数据不产生数据量差距过大的情况，保证模型训练时训练数据的水平化，不造成模型训练结果的偏失。

#### 4.3.2.3 行车数据处理系统
结合上述行车图像处理与行车数据处理的处理方法，设计行车数据处理系统，方便由于不确定因素从而进行多次采样后的数据处理情况。图像数据与速度数据相互依赖又相互约束。设计行车数据并行处理系统流程


当数据采集时，由于ROS系统与各个自身功能部件的协调使得在数据采集过程中图像数据与行车速度数据大致重合但还不精确，为了避免由于时间差问题从而导致模型训练时误差的累积，故在读取标签文本数据并进行四类标签转换和行车图像数据后，首先进行两者数据量的对比，并进行时间一致性检验，保留都为在同一时刻发生的数据，并在此时获取行车图像路径索引。并处理速度为0的无效数据以及数据均衡化处理。在整个智能车行驶中占比最高的为前进，向左前行驶和向右前行驶的数据，由于在数据采集过程中，存在智能车在进入弯道前的位置不适宜接下来的弯道行驶，需要原地对智能车的位置进行调整，所以才存在了占比较小的倒车/刹车，顺时针旋转和逆时针旋转的数据。由于这三种标签占比量较小，而且在三大标签进行速度交接时可以起到润滑平滑换的作用故保留当前三种标签，对占比较大的三类标签进行标签处理。得到的结果如表5所示。

表5 数据均衡化处理前与处理后数据量对比
|动作|	处理前|	处理后|	动作	|处理前|	处理后|
|:---:| :---: | :---: |:---: | :---: |:---: |
|前进|4325|3304|倒车/刹车|9|9|
|向左前行驶|3484|3298|顺时针旋转|12|12|
|向右前行驶|3698|3300|逆时针旋转|32|32|

根据表5所示的占比最高的标签在数据均衡化后数据量相差不大，数据量基本均衡。数据均衡化的结果在经过需要删除数据名称的筛选和无效数据进行合并。在得到无效数据和均衡化要删除的图像名称后，图像根据无效数据和均衡化要删除的图像名对图像数据进行删除，删除后并再次进行两者数据量的比较和又一次的数据一致性检验。在此之后，图像数据进行数据增强，高斯模糊与灰度化，二值化与等比缩放，速度数据则根据标签进行分层抽样，并进行数据的划分得到速度数据的训练集与测试集，而后图像数据根据划分得到的训练集与测试集进行图像训练集与测试集的分类，在每组数据中进行分层抽样操作，并抽取30%作为测试集与剩下70%的数据作为训练集，最后将训练集与测试集进行打包形成最终的训练数据集与测试数据集。

# 5 自动驾驶功能实现与整合
## 5.1 自动行车系统的设计与实现
由于在实际的模型部署中，智能车运用模型对当前路况进行合理且快速的判断。因此建立起的自动行车模型架构不宜过大，模型应具备模型体量小且训练效果较好的特点，根据实际应用情况，本章节介绍了针对行车数据的分析以及英伟达端到端模型和LeNet-5模型的训练并选择最优模型作为智能车自动行驶模型解决方案。
### 5.1.1 行车数据分析
通过分析行车数据进一步提高模型训练的合理性，提高模型的准确率。首先行车数据采集围绕场景平台进行，通过获取PS2操控手柄的信号使智能车绕场景平台中的道路行驶，借助ROS平台中Obom功能包对智能车运行的足迹进行绘制.


由于智能车只凭借Obom功能包进行方位采集，所以在进行数据可视化时产生了一定的误差。但在整体上，智能车完整的绕场景平台的道路进行行驶，没有产生在行车数据采集中由于没有完整的行驶而导致的数据偏颇。在模型开始训练前对行车图像数据进行归一化处理同时除以255以便减轻计算平台压力进一步提高训练效率，对于行车速度数据则需要选择适合的数据类型，通过将采集的一组行车速度数据进行可视化.

通过可视化下的行车速度可以更加直观的看到在行车速度数据集中线速度与角速度的分布情况。由于在设定线速度与角速度时，设定的基础速度为0.25与0.5所以使得线速度与角速度在图像存在上界，又由于设定的速度并不激进以及智能车大部分情况为前进运行，所以在线速度上没有出现负值。在角速度上由于存在弯道等运行情况或者由于默认速度设置相对保守使得转弯半径过大就会出现线速度为0从而只有角速度原地调整智能车车头朝向。在出弯道调整或正常行驶过程中发生车辆发生偏移从而修正车身，通过修正的幅度大小而导致了角速度相比于线速度有更多的随机性。线速度与角速度都由PS2操作手柄进行控制其遥杆量的大小为线形分布，但在可视化线速度与角速度图中由于基础速度设定的限制也可看作是智能车四个方向速度的分解只不过智能车动作幅度的大小取决于每个方向上速度存在的时间。针对上述两种情况分别建立对应的回归模型与分类模型进行验证并在择优的基础上进行进一步的优化。

### 5.1.2 行车模型的建立
#### 5.1.2.1 英伟达端到端模型
英伟达端到端模型（NVIDIA end-to-end Model）是通过整合硬件和软件来实现人工智能任务端到端优化，且建立在英伟达GPU架构上利用并行计算能力加速深度学习、机器学习等任务的一种模型。在英伟达端到端模型中神经网络输入的为原始图片，神经网络输出的为直接控制指令。由于在神经网络中每一个部分对于系统都起特征提取和控制的作用使得特征提取层和控制输出层的分界不明显。
英伟达端到端模型其基本的模型结构由卷基层与全链接层组成。在基本结构中神经网络的第一层使用归一化层对输入的数据进行归一化操作，对输入数据的每一个纬度都除以255并加上负0.5，将所有元素归一到负0.5到正0.5中，第一层不涉及学习过程。而后共有五个卷积层，其中前三层有的卷积核个数逐层增加分别是24个，36个和48个，选择了大小为5的卷机核和大小为2的步长，后两层每层共有64个卷积核选择了大小为3的卷积核但没有设定步长大小。后三层为全连接层每一层神经元个数分别是250个，50个和2个。其中除最后一层的激活函数以外，其余层的激活函数都为指数线性单元激活函数（elu）,关于卷积核与步长的设定英伟达并没有做过多的解释。在实际应用中针对典型参数进行尝试并选择效果较好的参数，但可能无法给出合理的解释。由于卷积核数量逐层增多使得提取出的图像特征越通用，进一步保留图像的准确特征，方便图像的进一步训练。

以英伟达端到端模型为基础进一步改进此模型，使改进的模型更好的适用于行车图像数据。由于模型结构在较为复杂的情况下造成模型的过拟合现象，又由于全连接神经网络中参数较多容易导致发生模型过拟合。所以在最后一层卷积层后加入了抛弃率为50%的Dropout层，并将三层的全连接神经网络去除中间一层的隐藏层，根据模型实际的任务情况对最后一层的输出层神经元个数与激活函数进行更改。

### 5.1.2.2 LeNet-5模型
LeNet-5模型是一种早期的卷积神经网络模型，由Yann Lecun等人于1990年开发，起初LeNet-5模型主要用于手写数字识别任务，但由于其具有很好的任务分类效果，后广泛使用于图像识别任务中。LeNet-5模型是深度学习和计算机视觉领域的里程碑之一，其结构为后来的深度学习提供了重要参考。
在手写数字识别中，LeNet-5模型架构共有7层。第一层为卷积层，共有6个大小为5的卷积核,第二层为池化层，平均池化层大小设定为2。第三层同样为卷积层，卷积层由16个大小为5的卷积核构成。第四层由大小为2的平均池化层构成。第五层由120个大小为5的卷积核构成，最后两层大小分别为84和10的全连接层构成。通过不断的进行卷积操作以及池化操作使得模型尽最大可能使图像特征保留下来，提高图像特征提取效果，帮助模型进行训练,其中除最后输出层以外，都为修正线性单元激活函数（relu）。LeNet-5模型结构如图。

根据当前所要进行的分类任务与回归任务对模型结构进行改造。图像经过数据处理中等比缩放的变换，大小从480*640转换到150*200，根据当前图像大小适当扩大了每一层卷积核的个数。第一层卷积核从原先6个增加为16个，第三层卷积核从原先16个减少到64个。后在实际训练过程中根据不同的训练任务对最后一层的输出神经元个数与激活函数进行更改。

### 5.1.3 行车模型的训练
#### 5.1.3.1 英伟达端到端回归模型的训练
构建英伟达端到端回归模型并训练，由于训练数据为回归任务，将模型最后一层输出神经元个数设置为两个，激活函数选用线性回归单元激活函数(linear)。为了方便评判模型回归的拟合程度所以将损失函数设定为均方误差，优化器选用自适应矩估计(adam)作为最小化损失函数并选用平均绝对误差作为模型的评估指标。设定模型学习率为0.000001，对数据集进行600次迭代，每次迭代的样本大小为300，并用测试集的数据与标签计算模型效果。根据测试集的平均绝对误差作为模型早停依据，避免模型发生过拟合，提高训练效率。如图49和图50分别为回归模型训练时的损失值与平均绝对误差的变化情况。

根据回归模型损失值变化与回归模型平均绝对误差变化可以看到，在训练过程中由于使用自适应矩估计作为优化函数，模型在开始训练后验证集与测试集的损失值快速下降，且在前50次训练中测试集的损失值下降幅度远远大于训练集的下降幅度。训练集与测试集在50次训练后二者下降趋势变缓，但整体为下降趋势。在平均绝对误差中，测试集与训练集的下降趋势与损失值变化相同，训练集与测试集的平均绝对误差随着训练模型下降速度变缓，并最终达到模型早停设定条件，模型训练结束。

#### 5.1.3.2 英伟达端到端分类模型的训练
构建英伟达端到端分类模型并训练，标签为多分类任务的九维数组，且训练任务为分类任务，所以将模型最后一层输出层的神经元个数设置为9，激活函数选用归一化指数函数(softmax)。模型损失函数选用分类交叉熵，由于随机梯度下降算法在实际表现中不如亚当优化算法(adam)，所以优化器选用亚当优化算法并约束学习率为0.000001。模型评估指标选用准确率作为判断依据，对数据集进行600次迭代，样本批次大小设定为600，并用测试集的数据与标签计算模型效果，为保证模型训练不发生过拟合现象故加入早停使模型在训练达到验证集准确率最高的情况下暂停训练。

根据分类模型损失值变化与准确率变化可知，在训练过程中分类模型的损失值在第50次训练左右降低到最小。在准确率变化中，测试集的准确率在第6次左右训练达到了一个小高峰，而后模型不断的训练使测试集的准确率下降并最终随着训练不断上升，最终达到最高值，训练集的准确率则是随训练次数的不断训练逐渐上升，并在最终达到稳定不在发生较大变化。由于模型设置了训练早停使的在第50次训练后停止训练，测试集准确率不在发生变化。模型训练结束。

#### 5.1.3.3 LeNet-5回归模型的训练
构建LeNet-5回归模型并训练，由于训练结果为连续型变量。模型最后一层输出层的个数应为两个神经元，且激活函数为线性激活函数(linear)。模型设置损失函数为均方误差，优化器选用亚当优化算法，为了避免学习速度过快而造成过拟合现象，故将优化器的学习率设定为0.000001，模型训练效果选用平均绝对误差作为判断依据。并对模型进行600次训练，每次训练样本大小设定为300。并用测试集的数据与标签计算模型效果，并根据测试集的平均绝对误差设置模型早停条件。

根据训练历史的可视化可以看到，由于模型在训练过程中加入早停，使得当训练集的平均绝对误差达到最低并持续一段时间后模型停止训练。其中训练集与测试集的损失函数不断下降，下降率随着不断训练逐渐变缓直到停止训练。训练集与测试集的平均绝对误差也伴随训练不断下降，并在最后达到最低。平均绝对误差下降率也随着不断训练逐渐变缓直到停止训练，模型训练结束。

#### 5.1.3.4 LeNet-5分类模型的训练
构建LeNet-5分类模型的训练，每组标签为九维数组。根据标签纬度将最后一层设定为9个输出神经元，选用归一化指数函数(softmax)作为激活函数，由于标签属于多分类类型，故使用分类交叉熵作为损失函数，为了避免模型在训练过程中发生过拟合现象，设置模型训练早停，以测试集准确率的最大值作为早停依据，并采用亚当优化算法作为优化函数，设置学习率为0.000001。模型评估指标选用准确率作为判断依据，对数据集进行600次迭代，故每次迭代的样本批次大小设定为600，并用测试集的数据与标签计算模型效果从而体现模型训练的准确度。

在LeNet-5分类模型的训练中，当训练集的准确率达到最高并持续一段时间的情况下为了避免发生过拟合的现象，训练发生早停。在LeNet-5分类模型损失值变化中，测试集的变化情况与训练集相同，并随训练次数的不断增加二者的损失值不断下降。在LeNet-5分类模型准确率变化中模型在训练到第10次左右时模型在测试集与训练集的准确率快速增高，并在第30次左后测试集和训练集的准确率上升到较高位置，上升速度逐渐变缓，且二者变化情况相同，测试集与训练集的准确率随着不断的训练逐渐增高并保持，模型训练结束。

### 5.1.4 行车模型的选取
#### 5.1.4.1 模型评判依据
在线速度与角速度回归任务中，选用平均绝对误差(Mean Absolute Error, MAE)和均方误差(Mean Squared Error, MSE)作为回归任务的主要评判标准。其中平均绝对误差可以帮助评估回归模型的准确性，得到预测结果与实际观测值之间的平均绝对偏差，且对异常值的鲁棒性表象较好，不会因为某个极端异常值的存在而使误差值被放大。设m为样本个数， 为预测值，yi为真实值，平均绝对误差的计算公式如下：
 
均方误差通过计算预测值与实际观测值之间的平方差的平均值来衡量预测误差的大小，方便模型根据均方误差结果进行参数调整，但对于异常值比平均绝对误差较敏感，从而对结果造成影响。设m为样本个数， 为预测值，yi为真实值，均方误差的计算公式如下：
在速度差分的分类任务中，选用准确率(Accuracy，Acc)作为模型评判的依据。准确率对于分类任务是常用评估标准之一，通过更加直观的反应正确预测的样本数量占总样本数量的比例使得人们可以更快的理解，在整体表现上反应模型的准确率，通过准确率的比较从而选择最佳模型。

其中Right为正确数据个数，All为全部数据个数。

#### 5.1.4.2 模型评判
基于英伟达端到端模型和LeNet-5模型两种基本模型分别根据不同训练任务进行了模型的训练。分别训练出了两种任务，两种架构的四种模型。为了进一步选择出实现智能车自动行驶的最优模型，将对这四种模型结合模型判断依据中的标准以及训练时间，模型大小，模型实际效果等几个纬度对模型进行比较。
在英伟达端到端回归模型和LeNet-5回归模型训练中，在保证参数相似，训练环境相同的情况下进行两种模型的比较，英伟达端到端回归模型与LeNet-5回归模型训练结果如表所示。

表6 两种回归类模型的训练结果比较
|模型类别|MSE|MAE|训练批次占比|
|:---:| :---: | :---: |:---: | 
|英伟达端到端回归模型|0.010790275|0.046966029|158/600|
|LeNet-5回归模型|0.011214698|0.052184216|340/600|

在两种回归类模型的训练比较中，LeNet-5回归模型的均方误差为0.011214698大于英伟达端到端回归模型的均方误差值0.010790275，而在平均绝对误差中LeNet-5回归模型也大于英伟达端到端回归模型的结果，在每批训练时间相似的情况下，英伟达端到端回归模型的训练时间要远远小于LeNet-5回归模型的训练时间。
在英伟达端到端分类模型和LeNet-5分类模型训练中，在保证参数相似，训练环境相同的情况下进行两种模型的比较，英伟达端到端分类模型与LeNet-5分类模型训练结果如表所示。

表7 两种分类模型的训练结果的比较
|模型类别|ACC|训练批次占比|
|:---:| :---: | :---: |
|英伟达端到端分类模型|0.97969496|52/600|
|LeNet-5分类模型|0.94803104|41/600|

在英伟达端到端分类模型与LeNet-5分类模型的比较中，LeNet-5分类模型的准确率为0.94803104小于英伟达端到端分类模型的准确率0.97969496。且两种模型单次训练时间相似，其中LeNet-5分类模型的训练批次占比小于英伟达端到端分类模型，LeNet分类模型的训练速度更快。
由于智能车所存在的算力资源一定的情况下，占用计算资源较小的模型会提高计算频率从而提高识别效率，针对四种模型进行内存资源占用上的比较。

表8 四种模型内存空间占比
|模型类别|内存占比|模型类别|内存占比|
|:---:| :---: | :---: |:---: | 
|英伟达端到端回归模型|41.20MB|英伟达端到端分类模型|43.13MB|
|LeNet-5回归模型|83.66MB|LeNet-5分类模型|83.67MB|

根据表8中的数据可以清晰的看到，LeNet-5为基础的模型比英伟达端到端为基础的模型内存占用高，在相同任务的情况下，LeNet-5模型要比英伟达端到端模型多大约40MB内存空间，这主要由于模型架构的区别，当模型架构相似时，更多的卷积层可以使数据快速降低纬度，数据在通往后续的全连接层时，全连接层训练所需的参数数量相应减少。且在基础模型相同的情况下，分类模型由于最后一层的神经元个数比回归多，分类模型的内存占用比回归模型高，在LeNet-5的分类模型与回归模型的比较中，分类模型只比回归模型内存占用多0.01MB。但在英伟达端到端模型中，分类模型比回归模型内存占用多1.93MB。

### 5.1.5 行车模型的实现与改进
#### 5.1.5.1 行车模型的实现
在进行了模型训练结果的数据对比与分析下，为了进一步加强对模型实际场景下的验证，分别将四种模型进行实际部署，通过行车轨迹采集系统对四种模型再一次的进行比较，车辆轨迹可以很好的反应出智能车在实际情况中的运行状态。更好的体现出智能车模型识别的稳定性与准确性。


车轨迹采集系统为单传感器收集数据，所以在智能车车头朝向会存在误差，由于车头朝向数据的偏差，导致起始位置与结束位置不重合。但智能车在实际车道中的表现并没有由于误差而造成消失。
模型部署的实际效果主要是智能车在直行车道中的稳定性以及智能车在行驶弯道时的表现，主要评判标准为在直行车道中智能车蛇形运动幅度的大小，造成蛇形运动的产生主要分为两个方面，其一是在进行数据处理时缩小了感受域，导致模型只针对处理后的感受域做出判断，不能结合未来道路的变化进行提前的判断来及时修正车身，使车身修正幅度过大。其二，智能车在道路行驶所占据的位置很大程度上决定了智能车是否修正当前车身。当出现弯道时，智能车入弯位置很大程度上决定了智能车出弯位置，当智能车在不合适的出弯位置时，智能车这时就会进行车身姿态的修正，由于在实际计算平台中需要结合智能车有限的算力进行判断，所以识别频率受到限制，导致智能车在修正车身后不能及时的辨别智能车当前位置，使辨别速度与实际情况产生滞后效果，最终使智能车陷入不断的车身姿态修正过程中。
在上图四种模型中，红色圈内是智能车在不同模型模型下的行驶轨迹。其中英伟达端到端类模型中，回归模型比分类模型所产生的轨迹更加平滑。但结合真实的汽车行驶情况中，车辆在道路上行驶与车道的位置事实上并不是保持相对单一的位置，而是根据实际情况下对车辆在车道中的位置进行实时的修正。这种修正会随着速度的变化导致修正幅度的改变。在低速行驶中，车辆可以在很短的时间内实现很大幅度的修正范围，但在高速行驶中，车辆要实现与低速行驶中相同的修正幅度，需要的修正时间要远远大于车辆在低速下修正所需的时间。出于安全考虑的真实情况下，虽然两种算法都达到了可判断自行行驶的效果，但英伟达端到端分类模型的实际行驶效果要优于英伟达端到端回归模型。英伟达端到端分类模型的调整次数少且幅度小，调整精确。LeNet-5模型中，LeNet-5回归模型在直行道路的稳定性明显优于LeNet-5分类模型。LeNet-5回归模型的调整幅度更小且调整次数明显小于LeNet-5分类模型。英伟达端到端模型总体优于LeNet-5模型行驶的轨迹。
智能车对弯道的识别行驶也是评判四种模型识别道路准确性的参考依据，智能车在行驶弯道中所呈现的轨迹越平滑，表明模型在合适的入弯位置识别并做出了正确的判断。在绿色圈内，为一个连续弯道，连续的弯道可以反应出模型对于快速变化的路况是否可以做出及时的正确调整，如果智能车处于不利于进入弯道的位置，则会导致智能车在通过弯道时驶出车道，或使智能车在弯道中进行调整造成时间的浪费。根据上图中四个模型在连续弯道中的表现，英伟达端到端回归模型的轨迹相比于英伟达端到端分类模型轨迹更加平滑，但智能车在通过第一个弯道后，在进入第二个弯道时，英伟达端到端回归模型车头指向性不如英伟达端到端分类模型车头的指向性更加清晰。而在LeNet-5模型中，LeNet-5回归模型在连续弯道的行驶轨迹比LeNet-5分类模型的行驶轨迹更加平滑，且LeNet-5分类模型在第一个弯道中进行车头非平滑的调整，虽然使智能车最终保持到了车道内，但因此浪费了更多的时间。最终在行驶到第二个弯道时，LeNet-5分类模型车头的指向不如LeNet-5回归模型车头指向性明确。英伟达端到端分类模型和LeNet-5回归模型轨迹明显优于剩余的两种模型。 

#### 5.1.5.2 行车模型的改进
在行车模型的实现结果中，可以发现在四种模型的实际部署中都存在在直行车道中车身发生了一定幅度的抖动最终产生蛇形现象。抖动程度的大小取决于很多影响因素，比如智能车计算资源有限所导致计算频率降低，从而导致智能车反应不及时。或者在数据采集时，由于车道存在非传统弯道而导致了智能车在出弯时不能处于合适位置，从而造成模型计算错误。为了避免由于以上所存在的问题导致智能车在行驶过程中出现的蛇行现象。通过缩小智能车在行驶过程中的感受野，训练模型进行当前道路的判断，进而合理抑制智能车在直行道路行驶过程中的蛇形现象。
在真实驾车行驶过程中，车辆在直行道路行驶过程中也并非始终保持道路中间，期间也有驾驶人员的不断修正，通过驾驶员的不断修正来使得车辆保持在道路中间位置。修正波长越长则反馈的体感强度越小，震荡感越轻微。修正波长越短则反馈的体感越强，震荡感越明显。因此减小车辆自身行驶修正强度至关重要。驾驶员通常做出决策并不局限于远处视野路况，而是进行远近的路况扫描，并最终做出行驶决策。根据此缩小智能车的感受视野，对当前这部分感受视野进行路况判断。通过对路况的判断再结合正常自动行驶模型所需要的原始感受野，最终输出车辆在当前路况下合理的行驶速度。

通过行车数据图像处理的结果进行道路识别感受野的划分，道路识别感受野通过道路识别模型识别出当前道路情况，而处理后的行测数据则通过自动行车模型计算出当前需要的线速度与角速度。若道路识别出当前道路为直行车道，自动行车预测的角速度值过大，则进行智能车角速度的抑制；若道路识别出当前道路为弯道，自动行车预测的角速度值则不受到抑制。并获得最终角速度与线速度使智能车进行行驶。
通过对行车数据采集处理后的数据进行路况判断的感受野范围的提取，以原图像中间为界限，向上向下提取原图像高度的20%。并通过人工分类的方法对图像进行分类。共分为弯道与直道两种类别，每种类别共1253张图片

为了减小道路识别模型对智能车计算资源的占用，提高模型的计算速度，这里使用广义线性回归对模型进行训练。将数据集划分为测试集与训练集，其中测试集占原数据集个数的20%，其余为训练集。根据划分后的数据对模型进行训练。模型训练后在测试集上的准确率达到了0.94076923，在测试集上模型训练的准确率为0.93406593。且训练速度为23秒，模型占用内存为10MB。模型训练结果较好。
在实际部署过程中，模型可以根据当前感受野对路况进行分辨，并控制自动行驶模型计算到的角速度进行调控。如图为遇到不同路况时道路识别模型识别到的结果。

通过加入道路识别辅助智能车行驶，避免智能车在行驶过程中产生的蛇形运动。通过道路识别结果可以看到道路识别模型对当前道路识别的结果，若识别为直行车道则把识别区域的白色车道填充为绿色，若识别结果为弯道，则把识别区域的白色车道填充为红色。根据不同颜色车道的标注从而更好的让智能车识别当前的车道情况。后应用于智能车自动行驶中，如图65改进后智能车行驶轨迹所示，图中行驶轨迹相较于四个自动行驶模型轨迹更加顺滑，且在直行车道中虽能体现出智能车在车道中的调整但调整频率与调整次数明显下降。智能车运行的稳定性与蛇行现象明显提升与减少。


## 5.2 避障系统的设计与实现
在自动驾驶中，当车辆遇到车道突然侵入时，车辆的避障显得尤为重要。本章将分析智能车前方的激光雷达所采集到的车辆周围数据，设计智能车避障算法进行智能车周围障碍的判断，并对当前障碍做出反应最终实现智能车的避障动作。

### 5.2.1 激光雷达数据分析
激光雷达收集到的数据来自于智能车所搭载的思岚 A1 雷达。激光雷达设定扫描频率为5.5HZ设定为标准工作模式。激光雷达所扫描的数据结构主要分为9个部分，分别为头部分，角度最大值，角度最小值，每次扫描的角度差，每次扫描的时间差，总扫描时间，距离最大值，距离最小值和距离列表。
根据起始部分的角度最大值和角度最小值以及每次扫描角度差可以计算出在距离列表中每一个距离所对应的旋转角。旋转角和距离经过极坐标系与笛卡尔坐标系的转换得到当前车辆周围的环境状态。且每次扫描坐标原点为智能车点位。

### 5.2.2 避障系统的设计
为了避免当车辆遇到障碍物后继续前进，与障碍物距离不断减小，当距离障碍物过近时无法完成完整的避障动作，所以根据此种情况设定大于车辆中心到边缘距离的最大预警值。当车辆到障碍物的距离小于最大预警值时进行避障。实现前进，后退，左转与右转四个方向的运动，避免与障碍物发生接触。为了增强车辆对周围环境的感知能力，根据车辆尺寸分别对智能车正前方向，智能车正后方向，智能车正左方向以及智能车正右方向的障碍物进行划分，在智能车正前方以及正右方向和正左方向划分的区间上找到与智能车最近障碍物的距离。

当智能车与障碍物的距离达到设定的最大预警值时，车辆线速度将设置为0，并根据车辆正左侧与正右侧与障碍物距离的大小进行车辆运行方向的调整。若车辆左侧距离大于车辆右侧距离则智能车将向左侧以设定的速度进行左侧转向；若车辆右侧的距离大于车辆左侧距离则智能车将向右侧以设定的速度进行右侧转向。当智能车与障碍物的距离大于设定的最大预警值时则停止转向并以设定的默认线速度向前前进。


### 5.2.3 避障系统的实现
根据避障逻辑从而实现智能车的行车避障，如图69所示智能车处于开始行驶时刻，如图紫色代表智能车所在的位置，绿色部分代表设定的智能车预警值距离，若障碍物在智能车预警值距离内则标记为红色，而蓝色代表通过坐标变换激光所扫描到的障碍物。由于智能车正前方距离障碍物的距离大于预警值范围，所以在智能车以默认线速度前进到避障过程B所在的位置如图70所示。当在次位置扫描到前方障碍物的距离小于等于智能车预警值距离时，智能车停止前进。由于激光雷达测得智能车左侧距离大于右侧距离时，智能车进行默认角速度向左选择到避障过程C所在的位置如图71所示。当智能车正前方距离再一次大于设定的预警距离时，智能车停止转向并恢复默认线速度向前行驶到达避障过程D所在位置如图72所示。从而完成智能车整个避障过程。


## 5.3 红绿灯识别系统的设计与实现
在实际行车过程中，红绿灯在日常行车场景中是占比最多的一种行车状况。本章主要分析了红绿灯的基本参数，并结合当前车辆运行环境设计红绿灯颜色的识别以及距离的判断。从而在一定条件下使智能车对当前信号灯进行正确判断，进一步完善智能车自动驾驶能力。

### 5.3.1 红绿灯数据分析
红绿灯主要由红，黄，绿三种二极管显示颜色，并加入红绿灯倒计时屏幕。运行电压为3V。其中红灯和绿灯时间都为9秒黄灯时间为3秒，整个红绿灯高为8cm。当红绿灯开始运行时，每一次先从绿色灯开始，然后再到黄色灯最后到红色灯显示，且在每种灯显示的后3秒，灯的显示形式从长亮变为每1秒闪烁四下直至下一个颜色的灯亮起。



智能车前方行驶的路况图像主要由前方摄像头进行拍摄。摄像头距离地面高度为18cm。由于摄像头拍摄的角度，智能车行驶道路的宽度以及红绿灯本身大小的限制。所以将红绿灯设置在行驶车道的右侧。方便摄像头的捕捉与识别，确保摄像头的视野范围内存在完整的红绿灯信号。摄像头拍摄得到的图像长度为640像素高度为480像素，图像通道为RGB三通道。

### 5.3.2 红绿灯识别系统的设计
当智能车行驶到有红绿灯所在的路段时，为了达到与现实世界相同的逻辑判断，实现智能车遇到红灯时停止行驶，绿灯时开始行驶的功能，首先建立ROS图像的传输通道并对摄像头所拍摄的图像进行色彩颜色的转换，使图像原本的RGB颜色转换成HSV颜色范围。当红灯、绿灯或黄灯亮起时光线由于灯的周围的白色覆盖件使灯周围呈现较多的白色，针对由于光线所产生的白色范围进行提取并获得当前对应的遮罩层。结合相机标定数据对遮罩层图像进行图像畸变矫正，在此基础上结合所识别出红绿灯在图像中的具体位置，通过运用相似三角形的原理，计算出智能车当前位置与红绿灯之间的距离。若当前距离大于设定的距离，则让智能车保持继续的形式；若当前距离小于等于设定距离，则对具体信号进行识别。对遮罩中最大的区域进行图像的裁剪并进行HSV图像颜色空间的转换。通过设定红色，绿色和黄色的颜色范围进一步确定红绿灯当前为哪一种信号。当检查出的信号为绿灯或空时保持智能车继续行驶，若检查出的信号为红灯或黄灯时则使智能车停止行驶直到信号灯变为绿色。


### 5.3.3 红绿灯识别系统的实现
为了更好的验证红绿灯系统的可行性，依托于相机标定数据采集系统采集包括三种红绿灯信号的共13张图片进行验证。根据红绿灯识别系统的设计，由于不同颜色的灯光所在红绿灯周围的覆盖件上反光都为白色。因此，通过转换原始图像的色域对白色区域进行图像提取。根据图75和图76可以清晰的看到灯珠由于发光导致周围颜色为近似白色。并进行白色区域的边缘检测得到面积最大的白色区域。所对应的面积最大的白色区域也就为当前具体哪一个颜色的信号灯在使用。


当具体识别到信号时，根据图77的识别结果，将所框选的区域进行截取，截取出具体的信号类别，之后再次使用颜色范围提取的方法，划分红色，黄色以及绿色范围进行颜色判断，若红色范围大于黄色与绿色则为红灯，若黄色颜色范围大于红色与绿色则为黄灯，绿色亦然，根据此判断得到具体的信号标识。如图78，图79，图80所示为截取到的红灯，黄灯与绿灯。


通过截取到的不同颜色的信号灯进行当前智能车与信号灯之间距离的判断。根据针孔成像原理所知真实时间与相机世界比例呈相似三角形关系。通过已知的红绿灯高度和相机内参数焦距和图像呈现的像素点可以得到智能车距离红绿灯的大致距离。通过红绿灯的距离进而判断智能车的停车位置。如图为红绿灯的识别结果。红色字代表智能车识别红绿灯的信号种类，蓝色代表智能车距离红绿灯的距离。

## 5.4 自动驾驶整体系统的设计与实现
### 5.4.1 自动驾驶整体系统的设计
根据已实现的功能对其整合从而实现自动驾驶整体体系。在自动驾驶众多传感器中，设定传感器功能控制优先级至关重要。这将取决当车辆周围出现情况时，车辆会依据最高级别传感器进行行为决策，如当行驶至红绿灯路口时前方信号灯变为红色，突然冲出非机动车。此时，车辆周围毫米波雷达就作为最高优先级。行车电脑会对其进行数据分析并做出决策，当威胁消除时在进行红绿灯最高优先级的判断。
根据本文所要实现的三个功能进行功能优先级的化分。根据设定的模型行车环境从远到近进行智能车功能等级的排序，分别是红绿灯等级大于避障系统等级，避障系统等级大于自动行车系统。

当自动驾驶整体系统启动时，首先通过激光雷达数据通路和ROS图像传输通道获得智能车前方激光雷达扫描数据与摄像头捕捉图像。当红绿灯识别系统识别到红绿灯后对具体信号进行判断。若此时红绿灯信号为不可通行时则等到可通行信号的开放。当红绿灯识别系统反馈的信号为可通行或无信号灯时，则避障系统运行结果有效。当避障系统检查到前方有障碍物时则启动避障算法直到障碍物在所设置的阈值之外。在避障系统没有检测到障碍物时自动行车系统接管智能车控制并完成整体的车辆运行，达到最终的自动驾驶整体系统的建立。

### 5.4.2 自动驾驶整体系统的实现
自动驾驶整体系统的实现是依托于以上自动行车系统，避障系统以及红绿灯识别系统所实现的。为了更好的达到各个系统的完美融合，所以在初始场景的设定下加入障碍物与信号灯。
在原有场景下设置避障环境和红绿灯识别环境。激光雷达通过扫描周围环境，智能车通过避障系统实现智能车在当前设置的环境下通过障碍区域。通过智能车前置摄像头识别当前信号灯的类别，智能车通过判断信号灯的类别做出相应的动作。根据加入的避障区域与红绿灯识别区域划分出不同环境下智能车的功能区间。

智能车测试环境部署结束后，根据自动驾驶整体系统的设计的功能优先级进行整体系统的实现。为了使各部分传感器协调工作，避免由于各个功能在运行时对计算资源的占用而发生系统阻塞，内存溢出的问题。以及个功能部件的响应时间不一致导致智能车不能及时做出反应的问题。进一步优化由于硬件不足而导致的计算错误，由于ROS系统本身自带服务(Services)来进行并行数据交互，所以将各个功能部件进行单独计算并将各部分计算结果返回给处理整体数据的部分，经过计算，运行行驶算法，得到智能车运行速度，并最终实现智能车的正确行驶。
每一个智能车功能都进行独立于其他功能的运行，自动行驶系统会根据当前路况进行数据处理，运行模型得到的速度，并最终将智能车行驶所需要的角速度与线速度发布到主节点上，红绿灯识别系统，则根据红绿灯处理算法，订阅前置摄像头图片进行红绿灯信号的识别，并将识别到的信号发布到主节点上。避障系统则通过订阅避障节点，进行数据坐标的转换，并计算出四个方向需要的数据，将四个方向的数据发布到主节点上。最后，通过订阅上面每一个功能节点输出的数据，结合自动驾驶整体系统的设计的功能优先级对障碍，信号和速度进行整体的判断，并将最终得到的速度发布到智能车速度节点上实现智能车的运行。

根据设计的自动驾驶整体系统与智能车测试环境完成以后，对智能车进行实际测试，以验证智能车各个功能部件运行的完整度和自动驾驶整体系统决策的正确性，实现智能车在实际测试中的自动行驶，障碍避障和信号识别。

将智能车放到起始位置，启动自动行驶整体系统以及智能车轨迹记录系统和智能车摄像系统，对智能车在测试过程中遇到的路况以及行驶轨迹进行记录，不仅方便智能车在测试过程中自动行驶模型对当前路况判断准确性的评估比较，还进一步记录了第三视角智能车实际的行驶轨迹，以更加直观的视角方便对智能车行驶情况进行整体评估。在自动行驶整体系统启动以后，智能车将自行对识别到的路况产生相应的判断，对信号灯信息进行判别和对障碍物进行避障动作，依托于以上三种运行功能构建智能车自动行驶系统。如图为智能车在测试环境下的行驶轨迹。



根据智能车在测试环境下的运行轨迹。可以看到，智能车在整体运行中，随着从起始位置开始逐渐到结束位置，智能车的轨迹的颜色不断的加深。在智能车自动行驶部分中，智能车不论是在直行道路与弯道轨迹都表现的非常光滑，只有在一小部分场景下智能车才进行了车身姿态的修正，在红绿灯识别系统的情况下由于智能车检测到前方信号灯为红色信号时进行了停车操作如图85红绿灯识别停车，才使在红绿灯识别位置产生了明显的颜色差异。在行驶到避障系统中时如图激光雷达避障，由于小车运行避障系统如图86，避障系统设置默认速度过慢才导致了避障的范围内智能车行驶轨迹的颜色的快速加深，由于智能车需要时时刻刻对周围环境进行数据采集并进行计算判断，所以在进行避障的第一个右转过程中，智能车表现出对周围环境的判断，并没有过快的执行行驶策略。而是进一步调整车身姿态，以最优的计算结果驱动智能车决策从而使智能车通过避障行驶区域。后根据自动行驶系统的计算结果引导智能车返回开始起点。因此在此环境下实现了智能车的自动行驶，红绿灯识别以及避障行驶和分级控制功能的集合。
 
# 结论
本文自动驾驶项目成功实现了自动行驶、红绿灯识别、避障以及智能车道路判断等关键功能。尽管这些功能各自独立，但在构建自动驾驶系统的过程中，它们都是必不可少的。在自动行驶功能的实现过程中，初期阶段的行车数据获取至关重要，因为一个优质的数据集对后续的模型训练起到了决定性的作用。本次实验的环境设置为理想状态，道路周围颜色较深，包含直角弯道、回头弯道、直行道和斜弯道等多种路况。然而，由于硬件运行频率的不同，数据采集过程中的速度与图像存在时间差，这可能导致车辆无法及时做出判断。在模型训练过程中，选择了小体量模型以避免内存阻塞，但由于时间限制，模型的选择与测试还不够充分，无法实现准确可靠的运行结果。在避障系统的构建中，只对智能车周围四个方向的数据进行了分析，避障算法设计较为简单，基本实现了智能车在有障碍物下的避障行驶。未来的改进中，可以从更复杂的方向和层次进行智能车周围环境的判断，进一步提高智能车对障碍物的判断力和决策正确性。
在红绿灯识别系统中，我们使用了颜色空间的转换，但这种方法在非特定环境下可能会造成红绿灯信号的识别错误。未来，可以考虑加入Yolo V5,transformer，内容识别等模型，以提高系统在真实环境下的泛化能力。此外，本次自动驾驶系统主要依赖于以上三种功能的整合，但各个系统之间的协调也是至关重要的。在本项目中，我们主要采用纵向优先级的方式对自动行驶系统中的功能进行分级划分。然而，在实际情况中，自动驾驶系统需要在不同路况下进行相应优先级的调整，同时在优先级下，自动驾驶各种传感器的数据需要相互流通。处理单元需要对这一时刻下的数据进行并行计算，才能保证决策思路与优先级发挥出相应的作用。总的来说，通过本次实验，实现了自动驾驶的一小部分功能，并通过实际测试得到了预期的结果。在智能车自动驾驶的搭建过程中，软件与硬件的协同工作至关重要。未来，将继续优化和完善自动驾驶系统，以实现更多的功能，并提高系统的稳定性和可靠性。随着技术的不断进步，自动驾驶将会在未来的交通出行中发挥越来越重要的作用。









