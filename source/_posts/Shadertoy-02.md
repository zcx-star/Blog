---
title: Shadertoy - 02. Interior Mapping
date: 2019-01-30 14:51:45
tags: Shader
categories: Shader
---
![](/images/Shadertoy_02_01.png)
[Shadertoy地址](https://www.shadertoy.com/view/tsX3Dr)

---

Interior Mapping是一种伪造室内场景的实时渲染技术
不需要建模，在一个平面上就可以渲染出房间的体积感


这里是该技术开发者的[博客](http://interiormapping.oogst3d.net/)，里面有paper及demo
需要了解光线追踪的基础知识，可以看我[上一篇](https://zcx-star.github.io/2019/01/28/Shadertoy-01/)或者[Scratchapixel](http://www.scratchapixel.com/lessons/3d-basic-rendering/minimal-ray-tracer-rendering-simple-shapes)

实际应用中有两种思路，一种是生成单个房间，然后在房间内划分区域
另一种是将房间拆分成xyz三个方向的平面，本文实现的是这种

---

#### 实现思路
1. 设置参数搭建场景
2. 发射光线，检测是否和物体相交
3. 计算最终颜色

#### 如何检测相交
首先只考虑xz方向的平面，平面上任意一点可以表示为（0，h，0）
如何检测和平面相交在[前一篇](https://zcx-star.github.io/2019/01/28/Shadertoy-01/)里说过了，$ t = \frac{h - ro.y}{rd.y} $
假设两个平面之间间隔为$d$，那么平面的位置$h$可以表示为$n * d$（ $n$为任意整数）
但是因为平面是无限延伸的，如果直接做检测相交，光线碰到的永远只有最近的两个平面
![](/images/Shadertoy_02_02.png)
所以我们需要判断每个像素应该对应的平面坐标$h$，而不是找到最近的相交平面

当摄像机是仰视的时候，我们看到的是高于像素的天花板
当摄像机是俯视的时候，我们看到的是低于像素的地板
![](/images/Shadertoy_02_03.png)
那么我们可以通过判断$rd$的方向来确认是需要向上取整还是向下取整得到$h$

```
vec2 intersect(in vec3 ro, in vec3 rd, in vec3 Axis, in float d, in float id1, in float id2)
{
    float rd_weight = dot( rd, Axis );
    float ro_weight = dot( ro ,Axis );
    float pointer = ceil( ro_weight / d );
    
    if( rd_weight > 0.0 ) // look up
    {
        float h = pointer * d;
        float t = ( h - ro_weight ) / rd_weight;
        return vec2( t, id1 );  
    }
    else // look down
    {
        float h = ( pointer - 1.0 ) * d;
        float t = ( h - ro_weight ) / rd_weight;
        return vec2( t, id2 );  
    }   
}

```

对三个轴向都完成相交检测后，比较得到最近的$t$，完成相交检测

```
vec2 nearest(in vec2 c1,in vec2 c2,in vec2 c3)
{
    if( c1.x < c2.x )
    {
        return c1.x<c3.x? c1:c3;
    }
    else
    {
        return c2.x<c3.x? c2:c3; 
    }
}
```

#### 设置参数搭建场景
基本跟[上一篇](https://zcx-star.github.io/2019/01/28/Shadertoy-01/)一致，构造正确的摄像机视锥

#### 计算最终颜色
把坐标位置换算成uv，读取贴图得到最终颜色

#### 参考资料
[Interior Mapping](http://interiormapping.oogst3d.net/)