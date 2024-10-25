## 鸿蒙NEXT+Flutter开发5-第一个鸿蒙应用

&emsp;&emsp;通过前面步骤的操作，开发所需的硬件设备，软件运行环境均已配备完毕，接下来我们创建第一个应用，并使其运行在鸿蒙NEXT系统的手机中。

### 1.创建鸿蒙项目
&emsp;&emsp;使用下面的命令，进入工作目录，并创建鸿蒙项目
```
cd ~/work/harmony
flutter create --platforms ohos --org com.cdrviewer demo1
```
#### 相关参数说明
&emsp;&emsp;--platforms ohos 表明创建的平台是鸿蒙系统，当然也可以添加其他平台支持，对应的命令如下所示：
```
# 需要在当前项目目录中运行下面的命令
# 例如：~/work/harmony/demo1
flutter create --platforms=windows . # 开启windows 桌面
flutter create --platforms=linux . # 开启linux 桌面
flutter create --platforms=macos . # 开启macos 桌面
flutter create --platforms=ohos . # 开启ohos
flutter create --platforms=ios . # 开启ios
flutter create --platforms=android .# 开启android
```
&emsp;&emsp;--org com.cdrviewer 为组织名称，一般为自已拥有的域名反过来，如果还没有自己的域名，建议申请一个，因为后期的应用备案之类的也需要，另外有个自己的域名，宣传之类的也更方便，拥有一个专业的域名也可以提升APP在用户眼中的品牌形象。如果不指定，默认为com.example，后期可以通过查找替换进行修改。
&emsp;&emsp;demo1为项目名称，可以为自己的app起一个更有意义的名称。该名称与组织名称一起组成了应用的包名。比如当前APP的包名即为：com.cdrviewer.demo1。包名是应用中商店的唯一标识，就像每个人都有一个独一无二的身份证号码一样，包名确保了每个应用都能被准确地区分。
### 2.连接开发手机
&emsp;&emsp;在《鸿蒙NEXT+Flutter开发2-开启手机调试》这篇文章中，开启了手机的无线调试功能，在设置->系统->开发者选项->无线调试页面，可以查看到手机的IP地址和端口，假如为：192.168.31.128:39759。则通过如下命令，连接调试手机：
```
hdc tconn 192.168.31.128:39759
```
连接成功后，返回Connect OK。（请确保手机与电脑在同一局域网）
### 3.项目签名授权
&emsp;&emsp;运行DevEco Studio，打开项目的ohos目录，即：~/work/harmony/demo1/ohos目录，并Trust Project。
![QQ_1729831206288.png](https://s2.loli.net/2024/10/25/85SQx7LAlHNTjUf.png)
&emsp;&emsp;打开菜单File->Project Structure...，选择signing configs页面，使用开发者帐号进行登陆之后，选中Automatically generate signature后，确定，即可完成对项目的调试签名工作。
![QQ_1729831547317.png](https://s2.loli.net/2024/10/25/syNTDAzVa1XiI4j.png)
### 4.启动自制的鸿蒙NEXT应用
&emsp;&emsp;使用VS Code打开项目目录，打开main.dart文件，选择连接的手机端，然后点击运行按钮，一切正常的情况下，第一个鸿蒙NEXT应用，就会在手机端运行起来啦。
![QQ_1729832320463.png](https://s2.loli.net/2024/10/25/K9ijLNGgs6bxeQa.png)
&emsp;&emsp;恭喜，第一个鸿蒙NEXT应用已经正常运行起来啦！
![screenshot_20241025_130145.jpg](https://s2.loli.net/2024/10/25/2CIlEf4QUkuN7sc.jpg)