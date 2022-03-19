作为开发入手了ThinkBook 14p, 发现插电状态CPU温度也不是很高, 但风扇动不动就2500转，太激进了。

之前用的Thinkpad P1 Gen1，8850H+4K的机器，CPU经常到90度，但风扇转一下很快就会静音，一般维持在60度左右。

现在这个Thinkbook 14P散热噪音太扰人了。 前阶段看到有个Fanview能够手动更改转速，然后里面还有源代码，所以我就自己稍微修改了下，现在能够自动根据温度来调整风扇转速了。


默认为：
CPU温度>90 : 5000转(实际上最大4300转)
CPU温度>90 : 3800转
CPU温度>85 : 3300转
CPU温度>80 : 3300转
CPU温度>75 : 2500转
CPU温度>70 : 2500转
CPU温度>65 : 2500转
CPU温度>60 : 2000转
CPU温度>55 : 1900转
CPU温度>50 : 1500转
CPU温度>45 : 1500转
CPU温度>40 : 1000转
CPU温度>35 : 500转（实际为0转）

不过可以通过修改FanViewSetting.cfg进行自定义

比如
$0_9 MAX50= #19;
$0_A MAX45= #15;
$0_B MAX40= #10;
$0_C MAX35= #5;
就是
40到45是1500转
45到50是1900转
35到40是1000转
需要注意的是，数值0 会恢复默认转速，所以需要风扇不转，则改为5.

另外FanViewSetting.cfg里有另外三个参数：
$0_X AUTOCONTROL_CYCLE= #20;
$0_Y SUDDENRAISE_Counter_CYCLE= #2;
$0_Z TEMPDIFF= #10;


AUTOCONTROL_CYCLE： 多少周期来自动调整温度，FANVIEW里的进度条一个就是一个周期。
SUDDENRAISE_Counter_CYCLE ，结合TEMPDIFF： 如果在SUDDENRAISE_Counter_CYCLE 个周期内，温度与上一个变化周期的温度相差 达到TEMPDIFF时，立即进行风扇转速的调整。



代码仅供学习，硬件出了问题概不负责哈。
