## 鸿蒙NEXT+Flutter开发9-实现协议提醒页B

上篇文章中讲述了协议页面的基本框架代码，接下来将呈现页面的实现细节。
### 1.构建logo图标
页面的最上方放置应用的logo，为提升显示效果，为logo加入阴影，具体实现代码为：
```
  // logo
  Widget _buildLogo() {
    return Container(
      width: 144,
      margin: const EdgeInsets.only(top: (60)), 
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.center,
        children: [
          Container(
            height: 96,
            width: 96,
            decoration: const BoxDecoration(
              color: Color.fromARGB(255, 255, 255, 255),
              boxShadow: [
                BoxShadow(
                  color: Color.fromARGB(37, 22, 22, 23),
                  offset: Offset(5, 5),
                  blurRadius: 5,
                ),
              ],
              borderRadius:
                  BorderRadius.all(Radius.circular((96 * 0.5))), // 父容器的50%
            ),
            clipBehavior: Clip.antiAlias,
            child: Image.asset(
              "assets/images/logo.png",
              fit: BoxFit.fill,
            ),
          ),
        ],
      ),
    );
  }
```
![QQ_1730263851611.png](https://s2.loli.net/2024/10/30/cdv5KNPEFJos6Dx.png)
### 2.构建标题和文字
通过_buildPageHeadTitle和_buildPageHeaderDetail两个函数来完成文字部分的构建，实现代码如下：
```
  /// 页头标题
  Widget _buildPageHeadTitle() {
    return Container(
      margin: const EdgeInsets.only(top: 40),
      child: const Text(
        "温馨提醒",
        textAlign: TextAlign.center,
        style: TextStyle(
          fontWeight: FontWeight.normal,
          fontSize: 24,
          height: 1,
        ),
      ),
    );
  }

  /// 页头说明
  Widget _buildPageHeaderDetail(BuildContext context) {
    return Container(
      margin: const EdgeInsets.only(top: 20, bottom: 20),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: <Widget>[
          const Text(
            '    欢迎使用鸿蒙NEXT版演示1!',
            style: TextStyle(
              fontSize: 16,
            ),
          ),
          const SizedBox(height: 5),
          Text.rich(TextSpan(children: [
            const TextSpan(
              text: '    为了更好地保护您的权益，同时遵守相关监管的要求，请在使用前查阅',
              style: TextStyle(
                fontSize: 16,
              ),
            ),
            TextSpan(
                text: '《隐私政策》',
                style: TextStyle(
                  color: Theme.of(context).primaryColor,
                  fontSize: 16,
                ),
                recognizer: TapGestureRecognizer()
                  ..onTap = () {
                    UrlLauncher.launchInBrowser(
                        "https://www.cdrviewer.com/privacy");
                  }),
            TextSpan(
                text: '《用户协议》',
                style: TextStyle(
                  color: Theme.of(context).primaryColor,
                  fontSize: 16,
                ),
                recognizer: TapGestureRecognizer()
                  ..onTap = () {
                    UrlLauncher.launchInBrowser(
                        "https://www.cdrviewer.com/vip");
                  }),
            const TextSpan(
              text: '，并向您特别说明如下：',
              style: TextStyle(
                fontSize: 16,
              ),
            ),
          ])),
          const SizedBox(height: 5),
          const Text(
            '    为向您提供服务并保障账号安全 ，我们会申请系统权限收集设备信息。',
            style: TextStyle(
              fontSize: 16,
            ),
          ),
          const SizedBox(height: 5),
          const Text(
            '    点击[同意并继续]按钮代表您已同意前述协议。',
            style: TextStyle(
              fontSize: 16,
            ),
          ),
        ],
      ),
    );
  }

```
![QQ_1730263981620.png](https://s2.loli.net/2024/10/30/NDnLkgcB2GHOyzx.png)
### 3.构建操作按钮

```
  // 不同意
  Widget _buildRejectButton(BuildContext context) {
    return SizedBox(
      width: double.maxFinite,
      height: 44,
      child: TextButton(
        onPressed: () {
          showPrivacyDialog(context);
        },
        child: const Text('不同意'),
      ),
    );
  }

  // 同意
  Widget _buildAcceptButton(BuildContext context) {
    return SizedBox(
      width: double.maxFinite,
      height: 44,
      child: ElevatedButton(
        onPressed: controller.handleAccepted,
        child: const Text('同意并继续'),
      ),
    );
  }
```
![QQ_1730264087273.png](https://s2.loli.net/2024/10/30/MuVKvbEsrkZyaLF.png)
通过前面的步骤，协议提醒页的代码基本实现完成。其中《隐私政策》和《用户协议》两项内容，是通过打开外部连接，来呈现具体内容的。如何在鸿蒙NEXT系统下，实现打开外部连接，将在接下来的文章中，进行具体讲解。