## 鸿蒙NEXT+Flutter开发8-实现协议提醒页A

为了确保应用的使用符合相关法律法规要求，明确告知用户其在使用应用过程中的权利和义务。需要通过清晰展示用户协议内容，让用户了解应用的数据处理方式、服务条款等重要信息，增强用户对应用的信任。所以在用户开始使用应用之前，获取用户对用户协议的明确同意，以建立合法有效的使用关系。
### 1.协议页内容
具体协议内容，可以借鉴常见APP的内容，再根据自己的实际情况调整而来。比如中百度中搜索“app启动用户协议页”，查看对应图片，就会发现很多用户协议相关的图片。下图是百度App相关页面的截图：
![QQ_1730177003008.png](https://s2.loli.net/2024/10/29/njEH1bGImRcFOr5.png)
### 2.构建协议页面
我们根据百度App的页面，构建我们自己的用户协议页面。我们仍然使用GetX插件来实现相关功能。
首先中lib目录下新建pages目录，用于存放app的所有页面。新建welcome目录，用于存放用户协议的提醒页面文件。目录结构如下图所示：
![QQ_1730193254261.png](https://s2.loli.net/2024/10/29/W3srEOy1nMXUZGj.png)
### 3.页面代码解析
#### binding.dart
完成controller和page页面的绑定，其代码如下：
```
import 'package:get/get.dart';
import 'controller.dart';

class WelcomeBinding implements Bindings {
  @override
  void dependencies() {
    Get.lazyPut<WelcomeController>(() => WelcomeController());
  }
}
```
#### controller.dart
处理同意按钮的逻辑功能，代码如下：
```
import '/common/routers/routes.dart';
import '/common/store/store.dart';
import 'package:get/get.dart';

class WelcomeController extends GetxController {
  WelcomeController();

  // 同意协议，跳转到主页面
  handleAccepted() async {
    await ConfigStore.to.saveAlreadyOpen();
    Get.offAllNamed(AppPages.keyApplication);
  }
}
```
#### index.dart
为了方便引用，代码为：
```
library welcome;

export 'controller.dart';
export 'bindings.dart';
export 'view.dart';
```
#### view.dart
```
import 'package:flutter/gestures.dart';
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import 'package:get/get.dart';

import '/common/utils/utils.dart';
import 'index.dart';

class WelcomePage extends GetView<WelcomeController> {
  const WelcomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SafeArea(
        child: SingleChildScrollView(
          child: Center(
            child: Padding(
              padding: const EdgeInsets.all(40.0),
              child: Column(
                mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                mainAxisSize: MainAxisSize.max,
                children: <Widget>[
                  _buildLogo(),
                  _buildPageHeadTitle(),
                  _buildPageHeaderDetail(context),
                  _buildRejectButton(context),
                  _buildAcceptButton(context),
                ],
              ),
            ),
          ),
        ),
      ),
    );
  }
}
```
通过前面的步骤，利用GetX插件提供的框架，构建了协议提醒页的基础代码，最终显示效果如下所示：
![f498a625fa60fe2c4eaddd20789fcd0e.jpeg](https://s2.loli.net/2024/10/29/OZuhXNPkimr7bCx.jpg)