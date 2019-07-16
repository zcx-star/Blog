---
title: Directx11 - 02.流程梳理
date: 2018-09-03 17:19:31
tags: Directx11
categories: Directx11
thumbnail: /images/Directx11_02_01.png
---

```
WinMain()
  InitWindow()       //创建Windows窗口
  InitD3D11()        //创建Directx的设备接口
  InitScene()        //调用设备接口，创建渲染pipeline要用到的各种接口
    InitIAStage()    //因为IAStage在渲染中不变，所以创建完后直接set了
  MessageLoop()      //循环函数
    UpdateScene()    //把矩阵，光照的update放在这里，纯数学计算
    DrawScene()      //填充数据，set pipeline要用到的接口，Draw（）
  CleanUp()          //循环结束后释放内存
```

## InitD3D11() 
- 通过D3D11CreateDeviceAndSwapChain()创建swapChain，d3d11Device和d3d11DevCon。
- 创建renderTargetView和depthStencilView，执行OMSetRenderTargets()
![](/images/Directx11_02_01.png)

## InitScene() 
- 创建渲染pipeline要用到的各种接口
- 创建顺序并不是依照渲染顺序，先搭好框架，后面再实际填入数据
- 不需要更新的可以在这里填入数据并set
![](/images/Directx11_02_02.png)

##  DrawScene()
- 这个函数会依照实际渲染需求而改动，但整体流程都是clear->update->set->draw
![](/images/Directx11_02_03.png)

## 总图
![](/images/Directx11_02_04.png)