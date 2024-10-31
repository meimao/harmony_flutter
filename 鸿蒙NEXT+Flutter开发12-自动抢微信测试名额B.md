## 鸿蒙NEXT+Flutter开发12-自动抢微信测试名额B

上篇文章已经完成了自动化框架的配置，以及自动化测试脚本的基本工作。接下来将实现自动抢名额的任务。
### 1.人工操作
打开鸿蒙NEXT应用市场，搜索“微信”，点击“抢先体验”。
![QQ_1730376517250.png](https://s2.loli.net/2024/10/31/MYl52hVLQAGTwsg.png)
因为每次测试名额放量很少，不出意外，就会出现名额已满的提示：
![QQ_1730376630212.png](https://s2.loli.net/2024/10/31/xsAbuGL5oPEQ6Sd.png)
如果运气爆棚，就会出现下面的界面，微信没有截图，用QQ的替代一下。
![QQ_1730376899524.png](https://s2.loli.net/2024/10/31/3IHspWNwqkfY75l.png)
点击下面的安装按钮，就可以进行安装测试啦。
上面就是人工操作所需的步骤，我们利用自动化框架，让上面的步骤自动执行。
### 2.获取需要操作的控件
想要自动化上面的人工操作，需要知道每次操作的控件对象，也就是需要找到操作中的各个按钮。幸运的是有大神已经做了相关的工作，名字叫做ui-viewer，可以查看鸿蒙NEXT系统的UI控件树，获取控件详情。可以通过下面的命令安装：
```
pip3 install -U uiviewer
```
安装完毕之后，可以使用下面的命令启动：
```
python3 -m uiviewer
```
正常启动后，会打开网址http://127.0.0.1:8000，出现如下页面：
![QQ_1730375644698.png](https://s2.loli.net/2024/10/31/zoBak4b2iUXGMfS.png)
### 3.编写自动脚本
利用uiviewer，找到各个按钮的信息，编写脚本如下，具体思路见注释。
```python
from time import sleep
from hmdriver2.driver import Driver
# 需要根据实际进行替换
d = Driver("192.168.31.129:45897")
isFind = False
while not isFind:
    # 如果搜索按钮存在，则点击
    d(type="Button", text="搜索").click_if_exists()
    # 等待1s
    sleep(1)
    # 如果存在抢先体验按钮，则点击
    d(type="RelativeContainer", text="抢先体验").click_if_exists()
    # 如果名额已满，点击知道啦按钮
    d(type="Button", text="知道了").click_if_exists()
    # 如果有开始测试按钮，则测试
    d(type="Button", text="开始测试").click_if_exists()
    # 如果有需要同意的按钮，则同意
    d(type="Button", text="同意").click_if_exists()
    # 如果进入到应用信息页面
    if d(id="app_name").exists():
        # 点击安装按钮
        d(id="download_button").click_if_exists()
```
上面的脚本没有自动输入App的名称，可以根据需要，自己输入“微信”或“QQ”等，来自动查找测试名额，希望更多的鸿蒙NEXT用户可以尽快使用上微信或者QQ。
最后说明一下，文章的主要目的是讲述如何使用自动化框架，自动获取测试名额只是用到啦很少的功能，更详细的功能，请查询对应的自动化框架文档。