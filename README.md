![](logo.png)
># MCBLE介绍

>>####结构：corebluetooth.framework层 + 命令收发层 + 数据处理层

>>####特点：耦合性低，每一层都可以单独使用，方便自行处理数据指令收发和解析，如果觉得本解析不够恰当您也可以自己解析数据；下面对每一层做详细解释。

---

>###corebluetooth.framework层
* 1.根据项目实际情况采用单例的设计模式；
* 2.数据回调采用block和通知的方式来回调数据；
* 3.支持后台和非后台模式，只要打开BackgroudMedoes相应的蓝牙设置，会自动打开；
* 4.支持断开自动重连的模式；
* 5.支持5秒尝试等待模式；

---

>###命令收发层
* 1.为了对所有回调数据进行集中处理，也采用的是单利模式；
* 2.指令存储模式，为了防止漏发，发无回调的情况，对指令进行集中处理即指令排队发送；
* 3.对数据回调进行两种模式回调处理即成功和失败；
* 4.对较长回调的数据进行了数据验证，如果获取的数据通过比对不正确，会重发一次指令；如果数据还是不正确，直接回调失败不会再次发送；


---
>###数据处理层
* 采用model模式来处理数据，如果觉得提供的处理方式不合理，可以自行对数据进行分析处理；

---

>#使用介绍

1.开始扫描

````objc
[[MCBLEManager shareMCBluetooth] scanPeripherals];
````
***
2.扫描到的外设

````objc
[[MCBLEManager shareMCBluetooth] setBlockOnDiscoverToPeripherals:^(CBCentralManager *central, CBPeripheral *peripheral, NSDictionary *advertisementData, NSNumber *RSSI) {
}];````
***
3.连接外设

````objc [[MCBLEManager shareMCBluetooth] connectToPeripheral:peripheral];````

4.停止扫描

````objc [[MCBLEManager shareMCBluetooth] cancelPeripheralConnection:p];````

---
>##指令层使用介绍

>1.手机发送基准时间给设备:用户设定公/英制 12H/24H 制，经常 活动地,本地日期,本地时间给设备

>````objc - (void)sendBasicSetOfInformationData:(BOOL)metric
withActivityTimeZone:(NSInteger)timeZone
success:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---


>2.手机发送显示时间:本地(手机所在地)日期,时间,时区信息给设备

>````objc  - (void)sendLocalTimeInformationData:(NSDate *)date
success:(void (^)(NSData *data))success
fail:(void (^)(void))fail````

---


>3.手机发送用户基本参数: 体重,年龄,身高性别,步距给设备 性别,步距给设备

>````objc  - (void)sendUserInformationBodyDataWithAge:(NSInteger)age
withWeight:(NSInteger)weight
height:(NSInteger)height
sex:(BOOL)sex
withTarget:(NSInteger)target
withStepAway:(NSInteger)step
withSleepTarget:(NSInteger)time
success:(void (^)(NSData *data))success
fail:(void (^)(void))fail```

---
>4.手机请求设备发送公/英制,12H/24H 制,经常活动地时区,本地日期,本地时间

>````objc - (void)sendRequestBasicSetOfInformationDataSuccess:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>5.手机请求设备发送体重,生日,步距

>````objc - (void)sendLookBodyInformationDataWithUpdateSuccess:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>6.手机发送用户选择黑/白屏,是否待机显示的信息到设备

>````objc - (void)sendSetHardwareScreenDataWithDisplayFlag:(BOOL)displayFlag
waitingFlag:(BOOL)waitingFlag
success:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>7.机发送用户是/否需要数据隐私保护

>````objc - (void)sendPasswordProtectionDataWithOpen:(BOOL)open
withPassword:(NSString *)password
success:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>8.手机请求设备的复位信息

>````objc - (void)sendDeviceResetSignalSuccess:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>9.手机发送设定睡眠，午休时间

>````objc - (void)setSleepTimeOpenFlag:(BOOL)myFlag
planSleepTime:(NSDate *)planSleepTime openPlanSleepTimeFlag:(BOOL)openPlanSleepTimeFlag
remindSleepTime:(NSDate *)remindSleepTime remindSleepTimeFlag:(BOOL)remindSleepTimeFlag
awakeTime:(NSDate *)awakeTime awakeTimeFlag:(BOOL)awakeTimeFlag
noonSleepTime:(NSDate *)noonSleepTime noonSleepTimeFlag:(BOOL)noonSleepTimeFlag
noonSleepOverTime:(NSDate *)noonSleepOverTime noonSleepOverTimeFlag:(BOOL)noonSleepOverTimeFlag
noonSleepRemindTime:(NSDate *)noonSleepRemindTime noonSleepRemindTimeFlag:(BOOL)noonSleepRemindTimeFlag
success:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>10.手机查询设备的睡眠时间，午休时间

>````objc - (void)sendCheckDeviceSleepTimeSuccess:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>11.设备接收用户自定义的工作界面及免打扰功能后发送

>````objc - (void)sendSetDistrubTheFunction:(BOOL)logoFlag
calorieFlag:(BOOL)calorieFlag
sportDistance:(BOOL)sportDistance
sportTimeFlag:(BOOL)sportTimeFlag
progressFlag:(BOOL)progressFlag
faceFlag:(BOOL)faceFlag
alarmFlag:(BOOL)alarmFlag
messageFlag:(BOOL)messageFlag
callRemindFlag:(BOOL)callRemindFlag
messageRemindViewFlag:(BOOL)messageRemindViewFlag
messageRemindType:(MessageRemindType)messageRemindType
lifeTimeOverFlag:(BOOL)lifeTimeOverFlag
sportCountDownFlag:(BOOL)sportCountDownFlag
success:(void (^)(NSData *data))success
fail:(void (^)(void))fail````

---
>12.手机查询设备的工作界面及免打扰功能

>````objc - (void)sendQueryDeviceInterfaceAndDoNotDistrubTheFunctionSuccess:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>13.手机发送闹铃时间及使能

>````objc - (void)sendSetAlarmData:(NSArray <NSDictionary *>*)dataArray
success:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>14.手机查询闹铃时间及使能

>````objc - (void)sendDeviceAlarmTimeAndUseFuctionSuccess:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>15.手机发送用户佩戴方式信息

>````objc - (void)sendSetWearingWayDataWithRightHand:(BOOL)right
success:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>16.手机发送事件信息到设备(久坐提醒)

>````objc - (void)sendSetDeviceSedentaryRemindOpenState:(BOOL)stateFlag
setTimeArrary:(NSArray<NSDictionary *>*)setTimeArrary
quietTime:(NSInteger)quietTime
success:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>17.手机查询久坐提醒

>````objc - (void)sendDeviceSedentaryRemindSuccess:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>18.手机发送自动测试心率的时间

>````objc - (void)sendAutoHRTest:(BOOL)enable
time:(NSArray<NSDictionary *> *)time
success:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>19.手机查询设备定时心率测试时间

>````objc - (void)sendQuaryHeartRateMeasureTimeSuccess:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>20.手机请求设备传输计步/睡眠数据(XX 日期 XX 时间点开始的数据)

>````objc - (void)sendRequestHistorySportDataWithDate:(NSDate *)date
success:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>21.手机发送删除运动数据

>````objc - (void)sendDeleteSportDataWithDate:(NSDate *)date
success:(void (^)(NSData *data))success
fail:(void (^)(void))fail````



---
>22.手机请求设备开启实时传输数据-----APP 自动发送该指令

>````objc - (void)sendRealTimeGetDeviceDataSuccess:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>23.手机请求设备传输历史数据的开始日期和结束日期

>````objc - (void)sendHistoricalDataStorageDateWithUpdateSuccess:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>24.手机请求设备传输当天的数据--无日期要求---------该指令废弃，问题点：数据包不做校验；每一个index只返回一个数据，如果要求整天的数据得发48次，期间数据丢包处理比较麻烦

>````objc - (void)requestDetailsAtIndex:(NSInteger)index
success:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>25.手机请求设备关闭/开启设备功能,其他剩余的字节自动补齐

>````objc - (void)requestDeviceOpenOrCloseFunctionCameraFlag:(BOOL)cameraFlag//1Byte
lockViewFlag:(BOOL)lockViewFlag
shakeFlag:(BOOL)shakeFlag
findViewFlag:(BOOL)findViewFlag
stepALGFlag:(BOOL)stepALGFlag
musicViewFlag:(BOOL)musicViewFlag
blueControlViewFlag:(BOOL)blueControlViewFlag
privacyFlag:(BOOL)privacyFlag
haveBackViewFlag:(BOOL)haveBackViewFlag//2Byte
HRShakeFlag:(BOOL)HRShakeFlag
callFlag:(BOOL)callFlag
HRFullFlag:(BOOL)HRFullFlag
pinNumberFlag:(BOOL)pinNumberFlag
calariShowFlag:(BOOL)calariShowFlag
calariMeasureType:(CalariCaculateTyte)calariMeasureType
sleepFunctionFlag:(BOOL)sleepFunctionFlag
remindHaveIconFlag:(BOOL)remindHaveIconFlag
success:(void (^)(NSData *data))success
fail:(void (^)(void))fail````

---
>26.手机强制手环充电

>````objc - (void)sendDeviceConstraintChargingSuccess:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>27.启动手机拍照功能(设备发给手机)

>````objc - (void)sendOpenCameraOrderState:(BOOL)state
success:(void(^)(NSData *data))success
fail:(void(^)(void))fail````



---
>28.手机接收成功后此指令不用回调

>````objc - (void)sendAnswerDeviceMusicFuction````



---
>29.手机请求设备的版本信息

>````objc - (void)requestHardwareVirsionSuccess:(void (^)(NSData *data))success
fail:(void (^)(void))fail````



---
>30.手机请求设备启动 ANCS 功能(ios 手机专用)

>````objc - (void)requestDeviceOpenANCSSuccess:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>31.手机请求设备启动防丢功能

>````objc - (void)requestOpenLostFunctionSuccess:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>32.手机请求设备关闭防丢功能

>````objc - (void)requestCloseLostFunctionSuccess:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>33.手机请求设备震动马达

>````objc - (void)requestDeviceShakeSuccess:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>34.手机收到报警指令后返回,此指令不需要回调

>````objc - (void)sendReceiveDeviceAlarm````

---
>35.手机向设备发送蓝牙广播的名字

>````objc - (void)sendDeviceName:(NSString *)name
isBLE:(BOOL)isBLE
success:(void (^)(NSData *data))success
fail:(void (^)(void))fail````



---
>36.设备直接进入出厂模式

>````objc - (void)requestEnterFactoryModelSuccess:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>37.手机首次连接设备成功后，通知设备震动提醒并显示勾

>````objc - (void)sendAlertWhenConnectShakeCount:(int)count
hook:(BOOL)hook
success:(void (^)(NSData *data))success
fail:(void (^)(void))fail````



---
>38.手机发送特定指令和密码

>````objc - (void)sendPerpareRestDeviceAtIndex:(int)index
success:(void (^)(NSData *data))success
fail:(void (^)(void))fail````


---
>39.手机再发送确认的数据到设备

>````objc - (void)sendPerpareRestDeviceAgain````


---
>40.手机告知设备，准备空中升级

>````objc - (void)requestDeviceEnterOTA````


---
>41.获取心率监测数据

>````objc - (void)getHeartRateMeasureData:(void(^)(NSInteger bpmValue))dData````



---
>42.获取实时数据

>````objc - (void)getRealTimeData:(void(^)(NSData *data))dData````
