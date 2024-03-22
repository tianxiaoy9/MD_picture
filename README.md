# 关于F28379xD在simulink中epwm的设计
## Module选择epwm通道
## 频率设置
up或者down模式下：$T_{CTR}=(T_{BPRD}+1)*T_{BCLK}$ 其中：$T_{BPRD}$为time-base period，$T_{BCLK}$为time-base clock,默认为5ns。

up-down模式下：$T_{CTR}=2*T_{BPRD}*T_{BCLK}$

需要给的参数为timer period即$T_{BPRD}$。

如采用down模式，需要200kHz的频率，$T_{CTR}=1/200k=5\mu s$,那么$T_{BPRD}=T_{CTR}/T_{BCLK}-1=5\mu s/5ns-1=999$。

## epwm占空比设置
下两张图是epwm原理。
![](https://github.com/Flow0312/MD_picture/blob/epwm/1.png?raw=true)
![](https://github.com/Flow0312/MD_picture/blob/epwm/2.png?raw=true)
在设置epwm的占空比时，需要设置的是CPMA或CPMB的值，这个值可以给定，也可以采用强化学习模块实时变化给定。

通过Counter Compare里Specify CMPA via设置为Inport port可以在外部给定，在输入模块前需要加一个增益模块，数值设置为timer period的值。增益模块前面的模块给定一个0~1的数值即为占空比。设置如下图。
![](https://github.com/Flow0312/MD_picture/blob/epwm/3.png?raw=true)

## 死区设置
一般在电力电子器件控制中只需要单个pwm信号的情况下，不需要加入死区的设置，死区通常在电机控制中需要设置。

注意，如果仅需要一个信号，另一个信号始终为0的情况下，不能设置死区。一旦设置epwma的死区，那么epwmb必定为一组互补信号。

## ET触发AD转换
AD可以设置epwm触发，需要在ET模块里开启。

![](https://github.com/Flow0312/MD_picture/blob/epwm/4.png?raw=true)
