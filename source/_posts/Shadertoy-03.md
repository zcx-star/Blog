---
title: Shadertoy - 03. Cubemap
date: 2019-04-19 15:11:46
tags: Shader
categories: Shader
---
```
void mainImage( out vec4 fragColor, in vec2 fragCoord )
{
    vec2 uv = ( fragCoord * 2.0 - iResolution.xy )/iResolution.y;
    
    vec3 cameraPos = vec3(0.0, 0.0, 0.0);
    vec3 targetPos = vec3(cos(iTime), 0.0, sin(iTime));

    // camera space    
    vec3 yAxis = vec3(0.0, 1.0, 0.0);    
    vec3 zAxis = normalize(targetPos - cameraPos);
    vec3 xAxis = normalize(cross(zAxis, yAxis));
    
    float fov = 90.0;    
    float a = radians(fov/2.0);

    // ==============================    

    // 1. cube
    vec3 pos = normalize(uv.x * xAxis + uv.y * yAxis + 1.0/tan(a) * zAxis); 
    
    // 2. sphere     
    /*float z = sqrt(1.0 - uv.x*uv.x - uv.y*uv.y);
    vec3 pos = normalize(uv.x * xAxis + uv.y * yAxis + z * zAxis); */

    // ==============================
    
    fragColor = vec4(texture(iChannel0, pos).xyz, 1.0);
}
```
![](/images/Shadertoy_03_01.png)
### 1. cube
世界坐标为$（ uv,  t = \frac{1.0}{tan(a)} ）$，camera space坐标系为$（xAxis, yAxis, zAxis ）$
坐标系转换world->camera: 
```
vec3 pos = normalize(uv.x*xAxis + uv.y*yAxis + 1.0/tan(a)*zAxis); 
```

### 2. sphere
sphere半径为1，求出z，得到世界坐标，转换到camera space