---
typora-root-url: ./
---

# day2

## 任务

1. 画一下电路图：CMOS反相器、与非门、或非门、三态输出门、漏极开路门。
2. 解释一下Vih，Vil，Vol，Voh，Vt，Iddq
3. CMOS反相器的速度与哪些因素有关？什么是转换时间（transition time）和传播延迟（propagation delay）？
4. CMOS反相器的功耗主要包括哪几部分？分别与哪些因素相关？
5. 什么是latch-up(闩锁效应)？
6. 相同面积的cmos与非门和或非门哪个更快？

## 解答

1. 画一下电路图：CMOS反相器、与非门、或非门、三态输出门、漏极开路门
   + CMOS反相器
![CMOS_Inverter](./CMOS_Inverter.svg)
	
	+ NMOS与非门
![Nmos_enhancement_saturated_nand](/Nmos_enhancement_saturated_nand.svg)

	+ CMOS或非门
   ![Cmosunbuff](/Cmosunbuff.png)

	+ 三态门
![d009b3de9c82d158e72e1130800a19d8bd3e4286](/d009b3de9c82d158e72e1130800a19d8bd3e4286.jpg)
	
	+ 漏极开路门
![201203192130166732](/201203192130166732.png)
额外说一句，开漏输出主要用在IIC的接口上面，因为IIC的协议需要依靠外部上拉提供高电平。当多个IIC设备挂载在同一个总线上面的时候SoC就是使用开漏输出实现“线与”逻辑。实现对总线的占用

2. 解释一下Vih，Vil，Vol，Voh，Vt，Iddq
    + Vih： 保证逻辑门的输入位高电平的时候所允许的最小输入高电平，当输入电平高于Vih的时候，认为输入电平是高电平
    + Vil：保证逻辑门的输入位电平的时候所允许的最大输入电平，当输入电平低于vil的时候，则认为输入电平为低电平
    + Voh：保证逻辑门的输出为高电平的输出电平的最小值。逻辑门的输出位高电平时的电平都必须大于Voh
    + Vol： 逻辑门的输出为低电平的输出电平的最大值，逻辑门的输出位低电平时的电平都必须小于Vol
    + Vt：MOS管的反转电压
    
3. CMOS反相器的速度与哪些因素有关？什么是转换时间（transition time）和传播延迟（propagation delay）？

    

4. CMOS反相器的功耗主要包括哪几部分？分别与哪些因素相关？

    静态功耗：CMOS在PN结会产生一个反向漏电流，这个产生静态功耗

    动态功耗：CMOS在反转的时候产生的功耗

    短路功耗：这个是MOS管在反转的时候N管和P管都导通的时候产生的功耗

    一半来说芯片的工艺在40ns以上的时候，动态功耗会比较重要，但是在40ns以下的工艺里面，静态功耗的占比会明显的提升。在系统设计中，我门一半会把芯片的供电电压降低，来减小系统的功耗，但是同时也会降低系统的稳定性，更容易被静电击穿。低功耗的设计中，主要是工艺级别的以及系统级别的低功耗设计对降低功耗影响比较大，但是我们日常的设计中接触到比较多的是RTL级的低功耗计数以及门级的低功耗计数。

5. 什么是latch-up(闩锁效应)？

    Latch up 是指[cmos](https://baike.baidu.com/item/cmos)晶片中，在电源power VDD和[地线](https://baike.baidu.com/item/地线/9752703) GND(VSS)之间由于寄生的[PNP](https://baike.baidu.com/item/PNP)和NPN双极性BJT相互影响而产生的一低阻抗通路

6. 相同面积的cmos与非门和或非门哪个更快？

    pmos比nmos的迁移率小，标准的与非门和或非门中的nmos和pmos宽长比不一样，因此pmos比nmos的寄生电容大，因此pmos充电时间长，比较最长路径的延时，或非门是两个pmos串联，与非门是两个nmos串联，因此与非门的速度更快。