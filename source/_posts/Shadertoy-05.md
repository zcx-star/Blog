---
title: Shadertoy - 05. Raymarching Distance Fields
date: 2020-05-13 14:13:25
tags: Shader
categories: Shader
---

![](/images/Shadertoy_05_01.png)
[iq大神的教程](https://www.youtube.com/watch?v=Cfe5UQ-1L9Q&t=10672s)学会了一半，记录下基础场景代码
```
// ========== distance ==========
float sdSphere(in vec3 pos, float rad)
{
    return length(pos)-rad;
}

float sdPlane(in vec3 pos, float height)
{
    return pos.y-height;
}

// ========== get min distance ==========
vec2 map(in vec3 pos)
{ 
    // 1.0:ground 2.0:sphere
    vec2 dID1 = vec2(sdSphere(pos-vec3(0.0,0.5,0.0), 0.3),2.0);
    vec2 dID2 = vec2(sdPlane(pos,-0.46),1.0);
    
    return (dID1.x<dID2.x)?dID1:dID2;    
}

// ========== get min t ==========
vec2 castRay(in vec3 ro, vec3 rd)
{
    float t = 0.0;
    float id = -1.0;
    for(int i=0;i<100;i++)
    {
        vec3 pos = ro+t*rd;
        vec2 dID = map(pos);
        
        float d = dID.x;
        id = dID.y;        
        if (d<0.001)
        {
            break;
        }        
        t += d;
        if(t>20.0)
        {
            break;
            id = -1.0;
        }
    }     
    return vec2(t,id);
}

vec3 calNormal(in vec3 pos)
{
    vec2 e = vec2(0.0001,0.0);
    return normalize( vec3( map(pos+e.xyy).x-map(pos-e.xyy).x,
                           map(pos+e.yxy).x-map(pos-e.yxy).x,
                           map(pos+e.yyx).x-map(pos-e.yyx).x));
}

void mainImage( out vec4 fragColor, in vec2 fragCoord )
{
    vec2 p = (2.0*fragCoord-iResolution.xy)/iResolution.y;
    
    vec3 col = vec3(0.76); //sky color  
        
    float an = iMouse.x/iResolution.x*10.0;
    
    vec3 ta = vec3(0.0,0.65,0.0);    
    vec3 ro = ta + vec3(2.0*sin(an),0.0,2.0*cos(an));
    
    vec3 ww = normalize(ta-ro);
    vec3 uu = normalize(cross(ww,vec3(0.0,1.0,0.0)));
    vec3 vv = normalize(cross(uu,ww));
    
    vec3 rd = normalize(vec3(p.x*uu+p.y*vv+1.5*ww));
    
    vec2 tID = castRay(ro,rd);
    float t = tID.x;
    
    if(t<20.0)
    {
        // 1.0:ground 2.0:sphere 
        vec3 mate = vec3(0.87,0.84,0.86);
        
        if (tID.y<1.5)//ground
        {
            mate = vec3(0.59,0.61,0.58);  
            
        }
        else //sphere
        {
            mate = vec3(0.495,0.475,0.54);
        }       
       
        vec3 pos = ro+t*rd;        
        vec3 nor = calNormal(pos);
        
        vec3 sun_dir = normalize( vec3(0.8,0.6,0.2) ); 
        float sun_dif = clamp(dot(nor,sun_dir),0.0,1.0);
        float sun_sha = clamp(castRay(pos+0.001*nor,sun_dir).x,0.0,1.0);        
        
        float sky_dif = clamp( 0.5+0.5*dot(nor,vec3(0.0,1.0,0.0) ),0.0,1.0);
        float bou_dif = clamp( 0.5+0.5*dot(nor,vec3(0.0,-1.0,0.0) ),0.0,1.0);        
                        
        col  = mate*0.5*vec3(0.8,0.55,0.2)*sun_dif*sun_sha; 
        col += mate*vec3(0.86,0.94,0.95)*sky_dif;
        col += mate*0.3*vec3(0.7,0.3,0.2)*bou_dif; 
    }    

    fragColor = vec4(col,1.0);
}
```