Update for Thinkbook 16P:  
已经能够确认16P的FAN1和FAN2与显卡和CPU的对应关系。  
thinkbook 16p可以 配置文件的前部分调整为  
$9_A FAN1= #7  
$9_B FAN2= #6  

然后下方$0开头的部分调整显卡侧风扇的策略，   
$2开头的部分调整CPU侧风扇的策略。  

  
#####然后可以在某一个温度之上将风扇数值改为#0， 这样会把这一区间的风扇策略交还给EC！！#####  

  
V12 Log:
1.当出现DTS_GPU温度为0的时候，使用DTS_CPU的温度作为参数进行调整。
($9_A FAN1 或者 $9_A FAN2 配置对应的温度任意为0的时候，使用另外一个的温度作为参数)

V11 Log:
1.刚才发现个问题，目前低于40度会调整成风扇停转，然后超过会到1000转，但是14P这个过程停转到1000转的过程会 停转到2000多转再慢慢恢复到1000转，这个过程就有明显的风扇启停声音，
所以建议MAX35和 MAX00两档都设置成10, 也就是1000转，这样可以避免这种启停出现。
2.设置到超过95度直接5000转（也就是忽略配置文件中的这个MAX95），避免设置出错的时候还是0转导致硬件损坏。

V10 Log:  
可以两个风扇分别进行控制（需要16P的朋友帮忙测试，确认是否能够根据CPU和显卡温度分别控制两个不同的风扇）。  
配置均可以通过配置文件进行操作。  
  
  
1.配置传感器：  
FanViewSetting中以下部分设置传感器：  
$9_A FAN1= #6  
$9_B FAN2= #6  
  
  
#6是程序界面中的DTS_CPU  
#7是程序界面中的DTS_GPU  
A.对于14P来说两个都设置为#6即可  
B.对于16P  
16P的朋友需要确认下程序中显示的DTS_GPU是否是显卡温度，如果DTS_GPU确实是GPU温度，则需要手动测试下是FAN1对应CPU风扇，还是FA2对应CPU风扇，反了的话交换下位置即可。  
  

另外配置文件中$2开头的就是FAN2的逻辑设置了。配置方法跟$0开头的FAN1是一样的。  
  

目前尚未经过16P测试，如有问题，请用默认的配置文件，即FAN1和FAN2都是#6。  
  

直接关闭程序不能恢复机器自己的散热逻辑，需要按ESC键。重要的话说三遍  
直接关闭程序不能恢复机器自己的散热逻辑，需要按ESC键。重要的话说三遍  
直接关闭程序不能恢复机器自己的散热逻辑，需要按ESC键。重要的话说三遍  
  
 
V8 Log:  
  
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
$0_X AUTOCONTROL_CYCLE= #40;//40个周期（4秒）自动检测下温度，如果温度区间发生变化则调整转速。  
$0_Y SUDDENRAISE_Counter_CYCLE= #10;  
$0_Z TEMPDIFF= #5;//持续10个周期（SUDDENRAISE_Counter_CYCLE）的快速升温或者降温5度（TEMPDIFF），则立即更改转速，避免突发性升温导致硬件损坏。  





  

代码仅供学习，硬件出了问题概不负责哈。  

关于如何开机自启动并保持后台运行的话，建议使用AlwaysUp来托管运行该exe。
