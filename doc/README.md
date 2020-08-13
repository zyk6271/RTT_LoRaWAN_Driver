# 1 LoRaWAN_Driver软件包使用说明

## 1.1 依赖
	- 依赖
	  -  lora_radio_driver 软件包
		- 用于提供RF驱动
		- 用于提供timerserver
	  -  本软件包在2级优化下占用约9.4KB RAM + 60KB ROM(请注意所用MCU的内存情况)

## 1.2 获取软件包
使用 LoRaWAN_Driver软件包，需要在 RT-Thread 的包管理中选中它，具体路径如下：<br />

```
RT-Thread online packages --->
    iot --->
        [*] LoRaWAN_Driver: Semtech LoRaMac driver. --->
                Version (latest)  --->
```

	使用须知：
	1. 在选择并配置好lora_radio_driver以后，通过radio的主从机测试，方可使用本软件包
	2. 根据实际情况，进行网络参数的配置
	3. 在非必要的情况下请勿对频段参数(RegionCN470.h)进行修改
	4. radio包的example与当前例程，两者频段定义有冲突，只可存在一个频段定义
	5. 进行初始化后方可进行数据收发
	6. 如果编译后代码太大，可以开启Level 2优化，经测试可正常使用
	7. Studio开发工具请勾选libC组件
	8. 如果选择不开启sample，请自行调用LoRaWANInit方法进行初始化

## 2.1 LoRaWAN_Driver 软件包组织结构
	- boards
		- \NvmCtxMgmt.c
			- 非易失性存储器相关
	- mac
		- \LoRaMac.c
			- LoRaWAN协议栈
		- \LoRaMacAdr.c
			- Adr速率自适应相关
		- \LoRaMacClassB.c
			- Class B模式相关
		- \LoRaMacCommands.c
			- LoRaWAN协议栈自带网络命令相关
		- \LoRaMacConfirmQueue.c
			- LoRaWAN数据确认队列
		- \LoRaMacCrypto.c
			- LoRaWAN加密相关
		- \LoRaMacParser.c
			- LoRaWAN数据解析相关
		- \LoRaMacSerializer.c
			- LoRaWAN数据序列化相关
		- \Region.c
			- 总频段集合
		- \RegionCN470.c
			- CN470-510频段相关
		- \RegionCommon.c
			- 频段共用函数相关
		- \LoRaMacFunc.c
			- 各类回调函数入口
		- \LoRaMacConfig.h
			- 网络参数
	- samples
		- \sample.c
			- LoRaWAN初始化及主状态机例程
	- softse
		- \ase.c
			- ase加密
		- \cmac.c
			- cmac加密
		- \soft-se.c
			- 加密函数
		- \utilities.c
			- 相关工具
## 2.2 参数修改介绍
	- \mac\LoRaMacConfig.h
		- ACTIVE_REGION
			- 频段选择
		- APP_TX_DUTYCYCLE
			- 循环发送间隔时间
		- APP_TX_DUTYCYCLE_RND
			- 循环发送间隔时间随机值大小
		- LORAWAN_DEFAULT_DATARATE
			- 默认发送速率
		- LORAWAN_CONFIRMED_MSG_ON
			- 消息发送是否需要服务器下发确认
		- LORAWAN_ADR_ON
			- 速率自适应开关
		- LORAWAN_APP_DATA_MAX_SIZE
			- 单条上发数据的最大长度，具体参照频段规范
		- LORAWAN_APP_PORT
			- 端口号
		- OVER_THE_AIR_ACTIVATION
			- 入网方式选择，0为ABP，1为OTAA
		- DEVICE_CLASS
			- 工作模式选择，0为Class A,2为Class C
		- ABP_ACTIVATION_LRWAN_VERSION_V10x
			- 版本
		- ABP_ACTIVATION_LRWAN_VERSION
			- 版本
		- LORAWAN_PUBLIC_NETWORK
			- 是否为公开网络
		- IEEE_OUI
			- 自定ID
		- LORAWAN_DEVICE_EUI
			- OTAA入网ID
		- LORAWAN_JOIN_EUI
			- 对应TTN平台的Application EUI
		- LORAWAN_APP_KEY
			- OTAA入网密钥
		- LORAWAN_GEN_APP_KEY
			- OTAA入网密钥(保持与LORAWAN_APP_KEY同步)
		- LORAWAN_NWK_KEY
			- OTAA网络密钥
		- LORAWAN_NETWORK_ID
			- 网络ID
		- LORAWAN_DEVICE_ADDRESS
			- ABP设备地址
		- LORAWAN_F_NWK_S_INT_KEY
			- ABP网络密钥
		- LORAWAN_S_NWK_S_INT_KEY
			- ABP网络密钥(保持与上方密钥同步)
		- LORAWAN_NWK_S_ENC_KEY
			- ABP网络密钥(保持与上方密钥同步)
		- LORAWAN_APP_S_KEY
			- ABP应用程序密钥

------------

	\mac\RegionCN470.h
		- CN470_MAX_NB_CHANNELS
			- 频段最大频道数
		- CN470_TX_MIN_DATARATE
			- 频段最小发射速率
		- CN470_TX_MAX_DATARATE
			- 频段最大发射速率
		- CN470_RX_MIN_DATARATE
			- 频段最小接收速率
		- CN470_RX_MAX_DATARATE
			- 频段最大接收速率
		- CN470_DEFAULT_DATARATE
			- 频段默认接收速率
		- CN470_MIN_RX1_DR_OFFSET
			- 频段最小接收速率偏移
		- CN470_MAX_RX1_DR_OFFSET
			- 频段最大接收速率偏移
		- CN470_DEFAULT_RX1_DR_OFFSET
			- 频段默认接收速率偏移
		- CN470_MIN_TX_POWER
			- 频段最小发射功率
		- CN470_MAX_TX_POWER
			- 频段最大发射功率
		- CN470_DEFAULT_TX_POWER
			- 频段默认发射功率
		- CN470_DEFAULT_MAX_EIRP
			- 频段默认辐射功率
		- CN470_DEFAULT_ANTENNA_GAIN
			- 频段默认天线增益
		- CN470_ADR_ACK_LIMIT
			- 速率自适应最小ack次数
		- CN470_ADR_ACK_DELAY
			- 速率自适应ack延迟
		- CN470_DUTY_CYCLE_ENABLED
			- 发射占空比开关
		- CN470_MAX_RX_WINDOW
			- 频段最大窗口时间
		- CN470_RECEIVE_DELAY1
			- 频段窗口1延迟时间
		- CN470_RECEIVE_DELAY2
			- 频段窗口2延迟时间
		- CN470_JOIN_ACCEPT_DELAY1
			- 频段入网窗口1延迟时间
		- CN470_JOIN_ACCEPT_DELAY2
			- 频段入网窗口2延迟时间
		- CN470_MAX_FCNT_GAP
			- 最大允许下行丢包数
		- CN470_ACKTIMEOUT
			- ack超时时间
		- CN470_ACK_TIMEOUT_RND
			- ack超时时间随机数大小
		- CN470_RX_WND_2_FREQ
			- 窗口2频率
		- CN470_RX_WND_2_DR
			- 窗口2速率

------------


	\mac\RegionCN470.c
		- Line 377
			NvmCtx.ChannelsDefaultMask[0] = 0x00FF;
			NvmCtx.ChannelsDefaultMask[1] = 0x0;
			NvmCtx.ChannelsDefaultMask[2] = 0x0;
			NvmCtx.ChannelsDefaultMask[3] = 0x0;
			NvmCtx.ChannelsDefaultMask[4] = 0x0;
			NvmCtx.ChannelsDefaultMask[5] = 0x0;

		CN470频段分为96个频点，每8个频点对应一个BAND,如需修改设备所属的BAND，应对此进行修改，一共六组掩码，对应BAND1-BAND12.当前配置所显示的为BAND1,若NvmCtx.ChannelsDefaultMask[0]为0xFFFF则为BAND1,2同时选中，若NvmCtx.ChannelsDefaultMask[0]与NvmCtx.ChannelsDefaultMask[1]分别为0xFFFF，0xFF00则是选中BAND1,2,4，以此类推。
# 3 使用示例
## 3.1 测试平台
	当前MCU：STM32L431CBT6
	当前RF：ASR6500S(SX1262)
	当前服务器：LoRaServer(老版本的ChirpStack)
## 3.2 Finish测试命令
| finish命令 | 说明 |
|  --- | --- |
| lorawan init | lorawan初始化 |
| lorawan send <para1>| 数据发送 <para1>:要发送的数据|
| lorawan cycle <para1> | 循环发送测试 <para1>:时间（ms)（至少5秒）|
| lorawan restart   | 重新开始配置LoRaWAN,状态机置初始位 |
## 3.3 使用接口
### 3.3.1 发送完成回调
samplec : Line 271 --> void SendDoneCallback(uint8_t *buffer,uint8_t size)
说明：buffer内为已经发送的数据，size为数据大小
### 3.3.2 接收完成回调
samplec : Line 271 --> void ReceiveDoneCallback(uint8_t *buffer,uint8_t size)
说明：buffer内为已经接收到的数据，size为数据大小
### 3.3.3 数据发送接口
LoRaMacFunc.c : Line 220 --> bool DataSend( uint8_t *buffer, uint8_t size)
说明：buffer内为即将要发送的数据，size为数据大小
## 3.4 实际测试示例
图1：软件包命令help界面

![](https://s1.ax1x.com/2020/07/14/UUnwgx.png)

图2：ABP模式下初始化

![](https://s1.ax1x.com/2020/07/14/UUnaCR.png)

图3：OTAA模式下初始化

![](https://s1.ax1x.com/2020/07/14/UUn0v6.png)

图4：Class A模式下发送数据

![](https://s1.ax1x.com/2020/07/14/UUnN59.png)

图5：Class A模式下发送并接收数据

![](https://s1.ax1x.com/2020/07/14/UUnd81.png)

图6：Class C模式下发送数据

![](https://s1.ax1x.com/2020/07/14/UUntUJ.png)

图7：Class C模式下接收数据

![](https://s1.ax1x.com/2020/07/14/UUnYE4.png)

# 4 问题和建议

如果有什么问题或者建议欢迎提交 [Issue](https://github.com/zyk6271/LoRaWAN_Driver/issues) 进行讨论。
