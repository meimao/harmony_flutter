## 鸿蒙NEXT+Flutter开发10-打开网页连接

在上一个章节中，《隐私政策》和《用户协议》两项内容，是通过打开外部连接，来呈现具体内容的。在Flutter中可以利用url_launcher插件来完成此项工作，OpenHarmony SIG组织对该插件做了鸿蒙NEXT系统的适配，接下来将详细讲解如何利用url_launcher插件打开网页连接。
### 1.安装url_launcher插件
在pubspec.yaml文件中，dependencies下面添加如下代码：
```
dependencies:
  url_launcher_platform_interface: ^2.0.3
  url_launcher:
    git:
      url: "https://gitee.com/openharmony-sig/flutter_packages.git"
      path: "packages/url_launcher/url_launcher"
```
然后同步插件，执行命令：
```
flutter pub get
```
### 2.导入url_launcher插件
在需要使用url_launcher插件的dart文件中，使用下面的代码导入插件：
```
import 'package:url_launcher_platform_interface/url_launcher_platform_interface.dart';
```
### 3.使用外部浏览器打开网页
直接使用外部浏览器打开网页是最简单的一种方式，代码如下：
```
final UrlLauncherPlatform _launcher = UrlLauncherPlatform.instance;
  Future<void> launchInBrowser(String url) async {
    if (!await _launcher.launch(
      url,
      useSafariVC: false,
      useWebView: false,
      enableJavaScript: false,
      enableDomStorage: false,
      universalLinksOnly: false,
      headers: <String, String>{},
    )) {
      throw Exception('Could not launch $url');
    }
  }
```
比如打开百度网站，可以使用下面的代码：
```
launchInBrowser("https://www.baidu.com");
```
### 4.使用系统WebView打开网页
dart代码与使用外部浏览器方式相似，参数略有不同。
```
final UrlLauncherPlatform _launcher = UrlLauncherPlatform.instance;
  Future<void> launchInWebViewWithJavaScript(String url) async {
    if (!await _launcher.launch(
      url,
      useSafariVC: true,
      useWebView: true,
      enableJavaScript: true,
      enableDomStorage: false,
      universalLinksOnly: false,
      headers: <String, String>{
        'harmony_browser_page': 'pages/LaunchInAppPage'
      },
    )) {
      throw Exception('Could not launch $url');
    }
  }
```
使用WebView的页面，是在鸿蒙NEXT侧创建的，页面路径为上面代码中的'pages/LaunchInAppPage'。
创建页面文件：ohos/entry/src/main/ets/pages/LaunchInAppPage.ets，其内容为：
```
import { InAppBrowser } from 'url_launcher_ohos/src/main/ets/components/plugin/InAppBrowser';

@Entry
@Component
struct LaunchInAppPage {

  build() {
    Column() {
      InAppBrowser()
    }
  }
}
```
在ohos/entry/src/main/resources/base/profile/main_pages.json中加入新建的页面，内容如下：
```
{
  "src": [
    "pages/Index",
    "pages/LaunchInAppPage"
  ]
}
```
在dart代码中通过下面的方式，就可以在App内打开百度网站：
```
launchInWebViewWithJavaScript("https://www.baidu.com");
```
通过上面的步骤，实现了打开网页连接的目的。共有两种实现方式，其中直接使用外部浏览器打开网页的方式，实现较简单，只需要写Flutter代码即可。第二种方式，在App内部使用WebView方式打开，可控性更高一些，复杂度也相应提高，需要在鸿蒙NEXT端建立页面来显示网页内容，该页面可以根据需要进行调整，提供了更高的可定制性。