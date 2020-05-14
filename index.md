# WebGL坐标系统：
在webgl中 坐标(0,0,0)会在画布的c-c-c，这是与canvas l-t为(0,0)的坐标系统是不同的,同时在webgl中 x,y,z中的取值范围 均为 -1~1之间，可以理解为x,y,z的取值不是一个绝对坐标而是基于画布宽高的一个相对坐标。例如：
- (-1,0,0) l-c-c
- (1,0,0) r-c-c
- (1,1,0) r-t-c
- (-1,-1,0) l-b-c
- (-1,-1,1) l-b-near me
- (-1,-1,-1) l-b-far me

# 变量声明：
attribute | vec4 | a_position
----------| ---- | ---------
存储限定符 | 数据类型 | 变量名

## 存储限定符：
- **attribute**: 存储的是与顶点有关的数据
- **uniform**: 存储的是对于所有顶点都相关（或与顶点无关）的数据（全局变量）
- **textures**: 纹理是一个数据序列，可以在着色程序运行中随意读取其中的数据。 大多数情况存放的是图像数据，但是纹理仅仅是数据序列
- **varying**: 可变变量，一种顶点着色器给片源着色器传值的方式

## 数据类型：

类别 | GLSL ES数据类型 | 描述
---- | -------------- | ----
矢量 | vec2、vec3、vec4 | 具有2/3/4个浮点数元素的矢量
矢量 | ivec2、ivec3、ivec4 |具有2/3/4个整形元素的矢量
矢量 | bvec2、bvec3、bvec4 | 具有2/3/4个布尔值元素的矢量 
矩阵 | mat2、mat3、mat4 | 具有2*2、3*3、4*4的浮点数元素的矩阵(分别具有4、9、16个元素)

## 内置变量：
类型 | 变量 | 描述
----|------------|---------------
vec4 | gl_Position | 表示顶点位置（必须被赋值）
float | gl_PointSize | 表示点的尺寸，默认为1.0
vec4 | gl_FlagColor | 指定片元颜色(RGBA)

# 着色器行为
1. 逐个操作（即假设有若干顶点和若干片元，着色器并非先把所有顶点处理好，再处理所有片元，而是一个顶点处理好后，随后处理对应该顶点的片元）。
2. 先顶点后片元
3. 顶点传参给片元(传递的参数：gl_Position,gl_PointSize)

# 纹理对象pname参数表
gl.texParameteri(target,**pname**,param)
值 | 注解
--- |---
gl.TEXTURE_MAG_FILTER | 当纹理的绘制范围比纹理本身更大时，如何获取纹素颜色。例如将16×16的纹理图像映射到32×32的像素的空间时，纹理的尺寸就变成了原来的两倍。WEBGL需要填充由于放大而造成的像素间的空隙，该参数就表示填充这些空隙的具体方法
gl.TEXTURE_MIN_FILTER | 当纹理绘制范围比纹理本身更小时，如何获取纹素颜色。例如将32×32的纹理图像映射倒16×16的像素空间时。纹理的尺寸就只有原来的一半。为了将纹理缩小，WEBGL需要剔除纹理图像中的部分像素，该参数表示剔除像素的具体方法
gl.TEXTURE_WRAP_S     | 表示如何对纹理图像左侧或右侧的区域进行填充
gl.TEXTURE_WRAP_T     | 表示如何对纹理图像上方或下方的区域进行填充

# 纹理对象param参数表
gl.texParameteri(target,pname,**param**)
值 | 注解
---|----
gl.NEAREST | 可赋值给gl.TEXTURE_MAG_FILTER, gl.TEXTURE_MIN_FILTER，使用原纹理上距离映射后像素中心最近的那个像素的颜色值，例如16×16的纹理图像采用gl.TEXTURE_MAG_FILTER映射至32×32的图形上时，纹理图像上的一个像素点需要占据图形4个像素点，图形上的(0,0)的颜色等于纹理的(0,0)，而图形上的(0,1)、(1,0)、(1,1)将依然取纹理的(0,0)的颜色值
gl.LINEAR | 取的是距离新像素中心最近的四个像素的加权平均值，作为新像素的值，说白了就是取中心点附近像素的颜色的平均值(与gl.LINEAR相比，该方法图像质量更好，但是会有较大的开销)
gl.REPEAT | 平铺式的重复纹理
gl.MIRRORED_REPEAT | 镜像对称式的重复纹理
gl.CLAMP_TO_EDGE | 使用纹理图像边缘值