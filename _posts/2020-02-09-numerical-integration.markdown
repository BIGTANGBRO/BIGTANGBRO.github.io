---
layout: post
title:  Methods for numerical integration with code
date:   2020-02-09 15:05:55 +0700
image:  /assets/images/blog/post-5.jpg
author: Jiaxuan Tang
tags:   - Matlab
        - Numerical Methods
---

# 前言

>此篇博客为Imperial College大二数值分析课程的作业1 代码会附上

# 积分方法介绍

#### 简述积分方法以及代码


- **Mid-point rule**


- **Trapezium Rule**
 

- **Simpson rule**

#### 代码

以sinx,|sinx| 和 sin(x)^2 爲例，計算n為[25,50,100,200]的四種情況。
Simpson Rule 為其中最精確的方法。

 ```matlab

clear
clc

%change n in each iteration
n=[25,50,100,200];

%initialize the integral
IntSinMid = zeros(4,1);
IntAbsSinMid = zeros(4,1);
IntSin2Mid = zeros(4,1);

IntAbsSinTrapz = zeros(4,1);
IntSinTrapz = zeros(4,1);
IntSin2Trapz = zeros(4,1);

IntSinSim = zeros(4,1);
IntAbsSinSim = zeros(4,1);
IntSin2Sim = zeros(4,1);


%mid-point rule;
for num = 1:4
    h1(num) = 3*pi/n(num);

    for i = 0:n(num)-1
        xmi(num) = 0 + h1(num)*((2*i + 1)/2);
    
        IntSinMid(num) = IntSinMid(num) + h1(num)*sin(xmi(num));
        IntAbsSinMid(num) = IntAbsSinMid(num) + h1(num)*abs(sin(xmi(num)));
        IntSin2Mid(num) = IntSin2Mid(num) + h1(num)*sin(xmi(num))^2;
    
    end

    %Trapezium rule
    xi=zeros(4,1);

    for i = 1:n(num)
        xi(num, i+1) = 0 + i*h1(num);
    end

    for i = 1:n(num)
        IntSinTrapz(num) = IntSinTrapz(num) + h1(num)*((sin(xi(num,i)) + sin(xi(num,i+1)))/2);
        IntAbsSinTrapz(num) = IntAbsSinTrapz(num) + h1(num)*((abs(sin(xi(num,i))) + abs(sin(xi(num,i+1))))/2);
        IntSin2Trapz(num) = IntSin2Trapz(num) + h1(num)*((sin(xi(num,i))^2 + sin(xi(num,i+1))^2)/2);
    end

    %simpson rule
    for i = 0:n(num)
        if mod(i,2) == 0
            if (i==0) || (i==n(num))
                IntSinSim(num) = IntSinSim(num) + sin(xi(num,i+1));
                IntAbsSinSim(num) = IntAbsSinSim(num) + abs(sin(xi(num,i+1)));
                IntSin2Sim(num) = IntSin2Sim(num) + sin(xi(num,i+1))^2;
            else
                IntSinSim(num) = IntSinSim(num) + 2*sin(xi(num,i+1));
                IntAbsSinSim(num) = IntAbsSinSim(num) + 2*abs(sin(xi(num,i+1)));
                IntSin2Sim(num) = IntSin2Sim(num) + 2*sin(xi(num,i+1))^2;
            end
    
        else
            IntSinSim(num) = IntSinSim(num) + 4*sin(xi(num,i+1));
            IntAbsSinSim(num) = IntAbsSinSim(num) + 4*abs(sin(xi(num,i+1)));
            IntSin2Sim(num) = IntSin2Sim(num) + 4*sin(xi(num,i+1))^2;
        end
    end
    IntSinSim(num) = h1(num)/3*IntSinSim(num);
    IntAbsSinSim(num) = h1(num)/3*IntAbsSinSim(num);
    IntSin2Sim(num) = h1(num)/3*IntSin2Sim(num);

end

enum = 4;%can be changed
fun1 = @(x) sin(x);
IntAnalyticalSin = integral(fun1,0,3*pi);

fun2 = @(x) abs(sin(x));
IntAnalyticalAbsSin = integral(fun2,0,3*pi);
IntAnalyticalAbsSin = 6;

fun3 = @(x) sin(x).^2;
IntAnalyticalSin2 = integral(fun3,0,3*pi);

 ```