---
title: Directx11 - 01.配置环境
date: 2018-07-29 21:51:03
tags: Directx11
categories: Directx11
thumbnail: /images/Directx11_01_10.png
---
## 选择教程：
我跟的是[Braynzar Soft Tutorials](https://www.braynzarsoft.net/viewtutorial/q16390-braynzar-soft-directx-11-tutorials)
之前有跟过[DirectXTutorial.com](http://www.directxtutorial.com/LessonList.aspx?listid=11)，讲的挺好的，但是到一半就要付费了。
也看到过很多推荐说龙书或者dx自带的教程，但是因为我对C++不是很熟，首要的要求是教程的代码可以直接跑起来的，所以最后选了这个。

## 安装软件：
- Visual Studio 2017 [Download here](https://visualstudio.microsoft.com/zh-hans/downloads/)
- DirectX SDK June 2010 [Download here](https://www.microsoft.com/en-us/download/details.aspx?id=6812)
如果安装dx的时候遇到**s1023**的问题，是因为电脑上已经安装了Microsoft Visual C++ 2010 Redistributable，需要先卸载才能成功安装dx
![](/images/Directx11_01_01.png)
![](/images/Directx11_01_02.png)

## 新建项目：
菜单栏File -> New -> Project
![](/images/Directx11_01_03.png)
勾选Empty Project
![](/images/Directx11_01_04.png)
在Project的名字（Demo）上右键add->new item，新建main.cpp和Effects.fx两个文件
![](/images/Directx11_01_05.png)
详细代码从教程最下面的[Heres the full code](https://www.braynzarsoft.net/viewtutorial/q16390-5-color)复制

## 配置项目：
在Project的名字（Demo）上右键Properties进行配置
1. 找到自己安装dx的路径，把include和lib路径配置进去
	- 默认include地址：C:\Program Files (x86)\Microsoft DirectX SDK (June 2010)\Include
	- 默认library地址：C:\Program Files (x86)\Microsoft DirectX SDK (June 2010)\Lib\x86 	
	![](/images/Directx11_01_06.png)
1. Linker->System改成Windows
	- 如果报错为**X3501** 'main':entrypoint not found说明这里漏了没改
	![](/images/Directx11_01_07.png)
1. Shader Type改成Effect(/fx)
	- 教程里用的是SM4.0，所以相应的把SM改成4.0
	![](/images/Directx11_01_08.png)
1. 本次不用修改，但是跟着教程走下来很容易遇到的一个坑
	- 如果字符串用的是L"xxx"，选择Unicode Character Set
	- 如果字符串前面不带L，选择Multi-Byte Character Set
	![](/images/Directx11_01_09.png)

## 成功运行：
![](/images/Directx11_01_10.png)