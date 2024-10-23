## 鸿蒙NEXT+Flutter开发3-电脑环境配置

&emsp;&emsp;在macos电脑上配置鸿蒙 NEXT + Flutter 开发环境主要包括**安装 DevEco Studio、下载 Flutter SDK、配置环境变量、安装VS Code**等几大步骤：

### 1.下载并安装 DevEco Studio

-   打开华为下载中心，网址为：​[​https://developer.huawei.com/consumer/cn/download/​](https://developer.huawei.com/consumer/cn/download/)​，下载对应的Mac（ARM）版本。
-   运行安装程序，按照安装向导的提示完成 DevEco Studio 的安装。
-   安装完成后，打开 DevEco Studio，它会自动进行一些初始化配置。

![image.png](https://s2.loli.net/2024/10/22/sdB8RTMwEDHOlF9.png)

### 2.下载 Flutter SDK

&emsp;&emsp;目前谷歌并没有官方宣布 Flutter 对鸿蒙 NEXT 的支持。想要在鸿蒙NEXT系统上使用Flutter进行开发，需要使用社区定制版的Flutter，网址为：​[​https://gitee.com/openharmony-sig/flutter_flutter​](https://gitee.com/openharmony-sig/flutter_flutter%E2%80%8B%E2%80%8B)​

-   终端使用命令​`​mkdir work & cd work​`​，创建并进入目录，flutter SDK将下载在该目录中。
-   通过代码工具下载当前仓库代码​`​git clone https://gitee.com/openharmony-sig/flutter_flutter.git​`​，可指定dev或master分支，一般使用master分支即可。

### 3.配置环境变量

-   打开终端，输入env，查找得到SHELL=/bin/zsh，笔者使用的shell是zsh
-   输入命令：nano ~/.zshrc ，修改对应配置信息，添加下面的数据

```
# mac环境
        export TOOL_HOME=/Applications/DevEco-Studio.app/Contents # 
        export DEVECO_SDK_HOME=$TOOL_HOME/sdk # command-line-tools/sdk
        export PATH=$DEVECO_SDK_HOME/default/openharmony/toolchains:$PATH # hdc
        export PATH=$TOOL_HOME/tools/ohpm/bin:$PATH # command-line-tools/ohpm/bin
        export PATH=$TOOL_HOME/tools/hvigor/bin:$PATH # command-line-tools/hvigor/bin
        export PATH=$TOOL_HOME/tools/node/bin:$PATH # command-line-tools/tool/node/bin
        # 鸿蒙flutter端
        export PATH=/Users/******/work/flutter_flutter/bin:$PATH
        export PUB_HOSTED_URL=https://pub.flutter-io.cn
        export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
        export FLUTTER_GIT_URL=https://gitee.com/openharmony-sig/flutter_flutter.git
        export PATH="$PATH":"$HOME/.pub-cache/bin"
        # git代理，根据需要确定是否配置
        git config --global http.proxy socks5://127.0.0.1:1234
        git config --global https.proxy socks5://127.0.0.1:1234
        # 终端代理，根据需要确定是否配置
        export http_proxy="http://127.0.0.1:1234"
        export https_proxy="http://127.0.0.1:1234"
```

### 4.检测设置是否正常

-   运行 ​`​flutter doctor -v​`​ 检查环境变量配置是否正确
-   显示如下图所示，均为☑️，表示环境变量配置正确
-   如果存在✖️项，仔细对照上面步骤进行配置，直至全部☑️为止

![image.png](https://s2.loli.net/2024/10/22/sXQtgUrnB7bGM53.png)

### 5.安装VS Code

&emsp;&emsp;VS Code作为一款功能强大的代码编辑器，支持多种编程语言，并提供了丰富的插件生态系统，可以满足你的各种开发需求。故笔者使用VS Code进行Flutter代码的开发调试，其安装步骤如下。读者也可以选择自己熟悉的代码编辑器。

-   打开浏览器，访问 Visual Studio Code 官方网站：​[​https://code.visualstudio.com/​](https://code.visualstudio.com/)​。
-   在官网首页，你会看到 “Download for Mac” 按钮，点击它开始下载安装程序。
-   下载完成后，双击安装文件，将 VS Code 图标拖动到 “Applications” 文件夹中，完成安装。

&emsp;&emsp;完成上面的步骤之后，电脑端的配置工作基本完成。

&emsp;&emsp;至此，使用Flutter进行鸿蒙NEXT开发的软硬件环境已经全部备齐，其中主要包括硬件部分（Mate60Pro手机+MacBook Pro电脑）、软件部分（DevEco Studio+Flutter SDK+VS Code）。