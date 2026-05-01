---
title: 【MATLAB】信号与系统
tags:
  - 信号与系统
  - MATLAB
categories: 课程笔记
description: 本文主要包含了钱玲主编的《信号与系统(第6版)》教材中对应的Matlab程序以及相关说明，供大家参考！
cover: pic/system_signal/Matlab.jpg
katex: true
abbrlink: 4dc6f9d9
date: 2023-03-18 21:03:22
---
<p style="text-indent:2em">
本文所提供的程序均可在<b>Matlab R2022b</b>下测试运行，不同版本之间程序的运行情况可能会有所差异，请读者自行判断选择使用！
<p style="text-indent:2em">
<b>因作者水平有限，编写过程中难免出现疏漏，敬请各位读者指正！</b>


# 第1章 连续时间信号的时域分析
<p style="text-indent:2em">
在后续需要使用单位阶跃函数时，建议使用Matlab自带的heaviside()函数，在R2022b版本下使用书上所提供的Heaviside()函数有时会出现莫名其妙的报错！可能原因：后者的函数输出值类型是逻辑值(logical)


```matlab
%mat101.m

%绘制单位阶跃信号
t = -5:0.01:5;                         %横坐标-5到5，量化值为0.01
y1 = Heaviside(t); subplot(1,4,1);     %调用函数Heavisid，图形窗口四等分
plot(t,y1);axis([-5,5,-0.5,1.5]);      % plot函数绘制连续曲线,定义坐标范围
xlabel('t');ylabel('$ u(t) $','Interpreter','latex');title('单位阶跃信号');

%绘制指数信号
t=0:0.01:10;A=1;a= -0.4;y2 =A*exp(a*t);
subplot(1,4,2); plot(t,y2);axis([0,10,-0.5,1.5]);
xlabel('t');ylabel('$ e^{-0.4t} $','Interpreter','latex');title('指数信号');

%绘制正弦信号
A =5;w=0.5*pi;t = 0:0.01:16;y3 = A*sin(w*t);
subplot(1,4,3);plot(t,y3);title('正弦信号');
xlabel('t');ylabel('$ 5\sin \left( \omega t \right) $','Interpreter','latex');axis([0,16,-5,5]);line([0,16],[0,0]);   %画横坐标

%绘制抽样信号
t=-15:0.01:15;t1 =t/pi;y4 = sinc(t1);
subplot(1,4,4);plot(t,y4);title('抽样信号');
xlabel('t');ylabel('$ sinc(t) $','Interpreter','latex');axis([-15,15,-0.3,1.1]);line([-15,15],[0,0]);
```

```matlab
%mat102.m

syms t;
f1 = 0.5*t*(heaviside(t)-heaviside(t-4));
f2 = sin(4*pi*t);
f3 = f1 + f2;
f4 = f1 * f2;
subplot(1,4,1),ezplot(f1,[-1 6]);title('$ f_1(t) = 0.5t[u(t)-u(t-4)] $','Interpreter','latex');axis([-1,6,-0.2,2.2]);
subplot(1,4,2),ezplot(f2);title('$ f_2(t) = sin(4 \pi t) $','Interpreter','latex');
subplot(1,4,3),ezplot(f3);title('$ f_1(t) + f_2(t) $','Interpreter','latex');
subplot(1,4,4),ezplot(f4);title('$ f_1(t)*f_2(t) $','Interpreter','latex');axis([-6,6,-2,2]);
```

```matlab
%mat103.m

syms t;
f = heaviside(t+1)-t*heaviside(t)+(t-1)*heaviside(t-1);
f1 = 2*subs(f,t,0.5*t);
subplot(1,4,1);ezplot(f1,[-3,3]);title('$ f_1(t) = 2f(0.5t) $','Interpreter','latex');
f2 = subs(f,t,3-2*t);
subplot(1,4,2);ezplot(f2,[0,3]);title('$ f_2(t) = f(-2t+3) $','Interpreter','latex');
f3 = diff(f);
subplot(1,4,3);ezplot(f3,[-2,2]);
line([-1,-1],[0,1]);axis([-2,2,-1.5,1.5]);title('$ f_3(t)=\frac{df\left( t \right)}{dt} $','Interpreter','latex');
f4 = int(f);
subplot(1,4,4);ezplot(f1,[-2,3]);title('$ f_4(t)=\int_{-\infty}^t{f(\tau )d\tau} $','Interpreter','latex');
```
函数说明：[subs(s,old,new)函数](https://ww2.mathworks.cn/help/symbolic/subs.html)主要用来完成**变量替换**的工作，其中参数**s**代表需要替换变量的表达式，**old**代表旧的变量，**new**代表新的变量。若需要更详细的函数说明请参见Mathworks官网（提示：点击本段文字开头的超链接即可跳转~~）。

**1.6-1** 用**向量表示法**绘制时域波形
```matlab
%e101.m
%向量表示法绘制

% x1(t)=u(t+2)-u(t-3)
t = -4 : 0.01 : 4;
x1 = heaviside(t + 2) - heaviside(t - 3);
subplot(2, 2, 1); plot(t, x1);
axis([-4, 4, -0.5, 1.5]); xlabel('t'); ylabel('$ x_1(t) $', 'Interpreter', 'latex');
title('$ x_1(t)=u(t+2)-u(t-3) $', 'Interpreter', 'latex');

% x2(t)=cos(2pi*t+pi/3)
t = -2 : 0.01 : 2;
x2 = cos(2 * pi * t + pi / 3);
subplot(2, 2, 2); plot(t, x2);
axis([-2, 2, -1.2, 1.2]); line([-2, 2], [0, 0]);
xlabel('t'); ylabel('$ x_2(t) $', 'Interpreter', 'latex');
title('$ x_2(t)=cos(2\pi t+\frac{\pi}{3}) $', 'Interpreter', 'latex');

% x3(t)=(2e^{-t}-e^{-2t})u(t-1)
t = -1 : 0.01 : 7;
x3 = (2 * exp(-t) - exp(-2 * t)) .* heaviside(t - 1);
subplot(2, 2, 3); plot(t, x3);
axis([-1, 7, -0.3, 0.8]); line([3, 7], [0, 0]);
xlabel('t'); ylabel('$ x_3(t) $', 'Interpreter', 'latex');
title('$ x_3(t)=(2e^{-t}-e^{-2t})u(t-1) $', 'Interpreter', 'latex');
```


# 第2章 连续时间系统的时域分析

**例2.7-1**
```matlab
%mat201.m

a = [1 2 2];b = [1 3];sys = tf(b,a);
t = 0:0.01:6;
f = exp(-t);lsim(sys,f,t);
gtext('系统激励');gtext('系统响应');
```
函数说明：
![lsim_tf](/pic/system_signal/lsim_tf.png)

[gtext(str)](https://ww2.mathworks.cn/help/matlab/ref/gtext.html): 在您使用鼠标选择的位置插入文本 str。当您将鼠标指针悬停在图窗窗口上时，指针变为十字准线。gtext 将等待您选择位置。将鼠标指针移至所需位置并点击图窗或按任意键（Enter 键除外）。


**例2.7-2** 
```matlab
%mat202.m

a = [1 2 2 1];b = [3 2];sys = tf(b,a);
subplot(1,2,1);impulse(b,a,8);
subplot(1,2,2);step(b,a,10);
```
函数说明：
impulse函数可以求得系统的**单位冲激响应**，参数为sys和t，其中sys为系统对应的微分方程，t为持续时间。

step函数可以求得系统的**单位阶跃响应**，其用法与impulse函数类似。

 **例2.7-3** 本题需要绘制  $f_1(t)=u(t-1)-u(t-4),f_2(t)=0.5t[u(t)-u(t-2)],s(t)=f_1(t)*f_2(t)$  三者的时域波形。

 参考程序如下：
```matlab
%mat203.m

k1 = 0 : 0.01 : 5; k2 = -1 : 0.01 : 3; p = 0.01; 
f1 = heaviside(k1 - 1) - heaviside(k1 - 4); 
f2 = 0.5 * k2 .* (heaviside(k2) - heaviside(k2 - 2)); 
f = conv(f1, f2); f = f * p; 
k0 = k1(1) + k2(1); 
k3 = length(f1) + length(f2) - 2; 
k = k0 : p : k0 + k3 * p; subplot(1, 4, 1); 
plot(k1, f1); axis([0, 5, -0.2, 1.2]); title('$ f_1(t) $', 'Interpreter', 'latex'); 
subplot(1, 4, 2); plot(k2, f2); title('$ f_2(t) $', 'Interpreter', 'latex'); 
axis([-1, 3, -0.2, 1.2]); subplot(1, 4, 3); plot(k, f); 
h = get(gca, 'position'); h(3) = 2.4 * h(3); 
set(gca, 'position', h); 
title('$ f(t)=f_1(t)*f_2(t) $', 'Interpreter', 'latex'); axis([0, 7, -0.2, 1.2]); 
```


# 第4章 连续时间信号的频域分析

**例4.7-2** 绘制抽样信号$ f_1(t) = Sa(t) $和矩形脉冲信号$ f_2(t) = \pi [u(t+1)-u(t-1)] $的时域波形和频谱，并验证$Fourier$变化的对偶性

```matlab
%mat402


```