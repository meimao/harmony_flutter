## 鸿蒙NEXT+Flutter开发11-自动抢微信测试名额A

鸿蒙NEXT系统公测之后，微信/QQ作为最常用App之一，一直处于分批放量之中，用户想要获取试用名额，经常需要花大量时间查询，能抢到的仍然是少部分。
随后想到做个实验，完成使用自动化测试框架，自动搜索是否存在可用测试名额，代替手动操作的任务。下面将整个实验过程做一个记录。
### 1.选择hmdriver2自动化测试框架
鸿蒙官方提供了自动化框架hypium，但是其安装和使用相对繁杂，对小白用户不是很友好。
经过搜索发现hmdriver2，其是一款支持HarmonyOS NEXT系统的UI自动化框架，无侵入式，提供应用管理，UI操作，元素定位等功能，轻量高效，上手简单，快速实现鸿蒙应用自动化测试需求。故笔者推荐使用hmdriver2来完成此次任务。
### 2.安装hmdriver2基础裤
因为hmdriver2的自动化脚步语言为python，故使用下面命令安装hmdriver2。
```shell
pip3 install -U hmdriver2
```
### 3.通过无线调试连接手机
笔者默认读者已经具备鸿蒙NEXT的开发环境，如果还没有，请查看之前的环境配置相关文章，完成电脑环境配置。手机开启无线调试，并进行连接。使用下面的命令查询连接设备信息：
```shell
hdc list targets
```
如果一切正常，会返回手机连接信息，读者的IP地址和端口可能会有所不同。
```
192.168.31.129:45897
```
### 4.测试脚步是否正常
新建python文件，其代码如下：
```python
from hmdriver2.driver import Driver

d = Driver("192.168.31.129:45897")  # 需要根据实际进行替换
print(d.device_info)
```
运行后如果能出现如下的设备信息，代表准备工作结束，可以正式开始关键任务啦。
```
DeviceInfo(productName='HUAWEI Mate 60 Pro', model='ALN-AL00', sdkVersion='13', sysVersion='ALN-AL00 5.0.0.102(SP3C00E73R4P17log)', cpuAbi='arm64-v8a', wlanIp='192.168.31.129', displaySize=(1260, 2720), displayRotation=<DisplayRotation.ROTATION_0: 0>)
```
下篇文章将讲解如何利用上面的准备工作，自动完成搜索微信/QQ测试名额的工作。
![QQ_1730376899524.png](https://s2.loli.net/2024/10/31/3IHspWNwqkfY75l.png)