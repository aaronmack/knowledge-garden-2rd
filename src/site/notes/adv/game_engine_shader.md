---
{"dg-publish":true,"permalink":"/adv/game-engine-shader/","title":"Game Engine Shader","noteIcon":""}
---


# 基本概念

1. Properties中定义的属性，需要在下方SubShader中写出对应的名称。e.g.

```c
Properties
{
	_Color ("Color", Color) = (1,1,1,1)
}

   ...
   
fixed4 _Color;
```

[Unity - Manual: ShaderLab: defining material properties](https://docs.unity3d.com/Manual/SL-Properties.html)

2. Shader的类型为`#pragma surface surf Standard fullforwardshadows`则对应`void surf (Input IN, inout SurfaceOutputStandard o)`

|  | |  |
| :------: | :------: | :------: |
|Standard|SurfaceOutputStandard|
|Lambert|SurfaceOutput|
|||

[Unity - Manual: Writing Surface Shaders](https://docs.unity3d.com/Manual/SL-SurfaceShaders.html)

3. 内置函数的使用

- `tex2D`
- `UnpackNormal`
- `saturate`
- `…`

4. 透明物体Shader的设置

```
Tags { 
	"RenderType" = "Transparent" 
	"Queue" = "Transparent"
	"IgnoreProjector" = "True"
}
#pragma surface surf Standard alpha:fade nolighting
```

[Unity - Manual: Writing Surface Shaders](https://docs.unity3d.com/Manual/SL-SurfaceShaders.html)

5. Unity Shader中的Variables

[Unity - Manual: Built-in shader variables](https://docs.unity3d.com/Manual/SL-UnityShaderVariables.html)

内置变量

- `_Time`
- `…`

输入结构

- `worldNormal`
- `viewDir`
- `…`

6. Surface Shader 示例

[Unity - Manual: Surface Shader examples](https://docs.unity3d.com/Manual/SL-SurfaceShaderExamples.html)

7. 一些内置的调用函数与变量

- `_LightColor0 will be the primary directional light color`
- `#pragma surface surf Toon - LightingToon`
- `#pragma surface surf SimpleLambert - LightingSimpleLambert`
- `#pragma surface surf Phong - LightingPhong`
- `#pragma surface surf CustomBlinnPhong - LightingCustomBlinnPhong`
- `#pragma surface surf Anisotropic - LightingAnisotropic`
- `…`

https://docs.unity3d.com/Manual/SL-SurfaceShaderLighting.html

8. Unity Surface Shader的管线

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.6gz2yh2yo800.webp" alt="image" />

# Shader效果

## Border (Rim Effect) - Silhouette

```c
float border = 1 - (abs(dot(IN.viewDir, IN.worldNormal)));
float alpha = (border * (1 - _DotProduct) + _DotProduct);
o.Alpha = c.a * alpha;
```

点乘拿到视角边缘的过渡值，然后使用`_DotProduct`控制边缘的效果强弱

## Lambert

```c
half4 LightingSimpleLambert (SurfaceOutput s, half3 lightDir, half atten) {
    half NdotL = dot (s.Normal, lightDir);
    half4 c;
    c.rgb = s.Albedo * _LightColor0.rgb * (NdotL * atten);
    c.a = s.Alpha;
    return c;
}

void surf (Input IN, inout SurfaceOutput o)
{
    o.Albedo = tex2D (_MainTex, IN.uv_MainTex).rgb;
}
```

模型法线与灯光的点乘

## Toon

```c
fixed4 LightingToon (SurfaceOutput s, fixed3 lightDir, fixed atten)
{
    half NdotL = dot(s.Normal, lightDir);
    NdotL = tex2D(_RampTex, fixed2(NdotL, 0.5));

    fixed4 c;
    c.rgb = s.Albedo * _LightColor0.rgb * NdotL * atten;
    c.a = s.Alpha;
    return c;

}

void surf (Input IN, inout SurfaceOutput o)
{
    o.Albedo = tex2D(_MainTex, IN.uv_MainTex).rgb;
}
```

使用了一张ToonRamp贴图，再根据法线与灯光的点乘值来采样这张贴图

## Phone Specular

```c
// I = D + S
// D = N·L
// S = (R·V)^p
// R = 2N(N·L) - L
fixed4 LightingPhone(SurfaceOutput s, fixed3 lightDir, half3 viewDir, fixed atten)
{
	float NdotL = dot(s.Normal, lightDir);
	float3 reflectionVector = normalize(2.0 * s.Normal * NdotL - lightDir);
	float spec = pow(max(0, dot(reflectionVector, viewDir)), _SpecPower); 
	float3 finalSpec = _SpecularColor.rgb * spec;
	
	fixed4 c;
	c.rgb = (s.Albedo * LightColor0.rgb * max(0, NdotL) * atten) + (LightColor0.rgb * finalSpec);
	c.a = s.Alpha;
	
	return c;
}
void surf (Input IN, inout SurfaceOutput o)
{
    fixed4 c = tex2D(_MainTex, IN.uv_MainTex) * _Color;
    o.Albedo = c.rgb;
    o.Alpha = c.a;
}
```

[Implementing a Phong Shader in Unity - Jan's Place](https://janhalozan.com/2017/08/12/phong-shader/)

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.2pj57hmwnrg0.webp" alt="image" />

首先，我们知道投影的计算，但是我们不知$cos(\theta)$,但通过点乘我们可以替换掉得到下方的投影计算。

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.39rhduioys60.webp" alt="image" />

再去计算反射向量，代入投影的值，其中法线一般是unit vertor，所以最终可得`r = n * 2(l·n) - l`也就是Phone中计算的反射。

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.1slfvpca6k68.webp" alt="image" />

[3d - How to prove r = n \* 2(l·n) - l in specular reflection? - Mathematics Stack Exchange](https://math.stackexchange.com/questions/4335380/how-to-prove-r-n-2l-n-l-in-specular-reflection)

> 在LearnOpenGL中，也是一样的，https://learnopengl.com/Lighting/Basic-Lighting，是调用的reflect函数计算的反射方向

```c
vec3 viewDir = normalize(viewPos - FragPos); 
vec3 reflectDir = reflect(-lightDir, norm);
float spec = pow(max(dot(viewDir, reflectDir), 0.0), 32); vec3 specular = specularStrength * spec * lightColor;

vec3 result = (ambient + diffuse + specular) * objectColor;
```

## Blinn-Phong

```c
vec3 lightDir   = normalize(lightPos - FragPos);
vec3 viewDir    = normalize(viewPos - FragPos);
vec3 halfwayDir = normalize(lightDir + viewDir);

float spec = pow(max(dot(normal, halfwayDir), 0.0), shininess);
vec3 specular = lightColor * spec;
```

与Phong模型思路上是一样的，不过这里用的是halfDir，而没有去计算反射方向。

[LearnOpenGL - Advanced Lighting](https://learnopengl.com/Advanced-Lighting/Advanced-Lighting)

## Anisotropic Specular Shader

```c
Properties{
	_MainTint("Diffuse Tint", Color) = (1,1,1,1)
	_MainTex("Base(RGB)", 2D) = "white" {} 
	_SpecularColor("Specular Color", Color) = (1,1,1,1)
	_Specular("Specular Amount", Range(0,1)) = 0.5 
	_SpecPower("Specular Power", Range(0,1)) = 0.5
	_AnisoDir("Anisotropic Direction", 2D) = "" {}
	_AnisoOffset("Anisotropic Offset", Range(-1,1)) = -0.2
}

struct Input{
	float2 uv_MainTex;
	float2 uv_AnisoDir;
}

struct SurfaceAnisoOutput{
	fixed3 Albedo;
	fixed3 Normal;
	fixed3 Emission;
	fixed3 AnisoDirection;
	half Specular;
	fixed Gloss;
	fixed Alpha;
};

fixed4 LightingAnisotropic(SurfaceAnisoOutput s, fixed3 lightDir, half3 viewDir, fixed atten){
	fixed3 halfVector = normalize(normalize(lightDir) + normalize(viewDir)); 
	float NdotL = saturate(dot(s.Normal, lightDir));
	
	// Anisotropic:
	//// HdotA is 0 when perpendicular to the Anisotropic normal map, 1 when parallel.
	fixed HdotA = dot(normalize(s.Normal + s.AnisoDirection), halfVector); 

	//// modify the value with sin() so that we get a darker
	//// middle highlight and a ring effect based off of the halfVector.
	float aniso = max(0, sin(radians((HdotA + _AnisoOffset) * 180)));
	
	// Specular:
	//// scale the aniso value by taking it to a power of s.Gloss,
	//// then globally decrease its strengthby multiplying it by s.Specular.
	float spec = saturate(pow(aniso, s.Gloss * 128) * s.Specular);
	
	fixed4 c;
	c.rgb = ((s.Albedo * _LightColor0. rgb * NdotL) + (_LightColor0.rgb * _SpecularColor.rgb * spec)) * atten;
	c.a = s.Alpha;
	return c;
}
void surf(Input IN, inout SurfaceAnisoOutput o){
	half4 c = tex2D(_MainTex, IN.uv_MainTex) * _MainTint;
	float3 anisoTex = UnpackNormal(tex2D(_AnisoDir, IN.uv_AnisoDir));
	
	o.AnisoDirection = anisoTex;
	o.Specular = _Specular;
	o.Gloss = _SpecPower;
	o.Albedo = c.rgb;
	o.Alpha = c.a;
}
```

其中最主要的效果来自sin那里，用sin()修改这个值，我们就可以得到一个更暗的中间高光和一个基于halfVector的环形效果。

[Shader Programming, Volume 8. Anisotropic Specular Lighting | by Sebastian Monroy | Medium](https://medium.com/@smokelore/shader-programming-volume-8-cfc8c2fe3659)

## Wave (Vertices Animation)

```c
void vert(inout appdata_full v, out Input o){
    UNITY_INITIALIZE_OUTPUT(Input, o);
    float time =_Time * _Speed;
    float waveValueA = sin(time + v.vertex.x * _Frequency) * _Amplitude;
    v.vertex.xyz = float3(v.vertex.x, v.vertex.y + waveValueA, v.vertex.z);  
    v.normal = normalize(float3(v.normal.x + waveValueA, v.normal.y, v.normal.z));
    o.vertColor.rgb = float3(waveValueA, waveValueA, waveValueA);
}

void surf (Input IN, inout SurfaceOutput o)
{
    half4 c = tex2D(_MainTex, IN.uv_MainTex);
    float3 tintColor = lerp(_ColorA, _ColorB, IN.vertColor).rgb;
    o.Albedo = c.rgb * (tintColor *_tintAmount);
    o.Alpha = c.a;
}
```

sin(pos.x)得到一个振幅波，加到vertex.y的上面。

## Snow

```c
fixed4 _Color;
void vert(inout appdata_full v)
{
    // Wrong?
    // float4 sn = mul(UNITY_MATRIX_IT_MV, _SnowDirection);
    float4 sn = _SnowDirection;
    if (dot(v.normal, sn.xyz) >= _Snow)
        v.vertex.xyz += (sn.xyz + v.normal) * _SnowDepth * _Snow;
}

void surf (Input IN, inout SurfaceOutputStandard o)
{
    half4 c = tex2D(_MainTex, IN.uv_MainTex);
    o.Normal = UnpackNormal(tex2D(_Bump, IN.uv_Bump));

    if (dot(WorldNormalVector(IN, o.Normal), _SnowDirection.xyz) >= _Snow)
        o.Albedo = _SnowColor.rgb;
    else
        o.Albedo = c.rgb * _MainColor;
    o.Alpha = 1;
}
```

通过snowDir与Normal方向点乘与snow的阈值控制哪些区域产生雪，以及在vertex中朝着法线+snowDir的方向偏移顶点位置，来实现一个非常简单的雪的效果。


## Parallax

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.27jn8epa5um8.webp" alt="image" />

Parallax(视差)是移动当前视角，画面中的效果还是一直朝向摄像机不产生变化，这个效果可以使用AmplifyShaderEditor中的Parallax Node或者把Screen Position作为UV去采样Texture，两者方法都可以使用。
热扭曲：屏幕空间位置 + 采样纹理（使用Panner节点(随时间流动的UV值)）
边缘光：Fresnel效果 + 纹理扰动

https://www.bilibili.com/video/BV1YL4y1b7bB/?vd_source=ce02e1fd8ede3b92cc4f879b568541e2