## 鸿蒙NEXT+Flutter开发7-存储应用设置项

存储设置项可以让应用记住用户的个性化偏好。例如，用户可以根据自己的习惯设置应用的主题（如亮色模式或暗色模式）、字体大小、语言等。当用户下次打开应用时，这些设置能够自动应用，提供熟悉且舒适的使用体验。如果没有存储这些设置，用户每次打开应用都需要重新配置，这会极大地影响用户体验。
另外，APP要上架应用市场，在第一次下载并进入首页前，需要展示用户协议和隐私政策提醒，同意之后，之后运行就不需要提醒，这就需要记录一下，APP是否是第一次运行。
### 1.GetX与shared_preferences插件
GetX 是一个功能强大的 Flutter 插件，它提供了状态管理、路由管理、依赖注入等多种功能。其主要优势在于简单易用、性能高效，可以大大简化 Flutter 应用的开发流程。GetX插件作为一款纯dart语言实现的插件，在鸿蒙NEXT系统中可以直接使用。
Flutter插件中，能够提供存储功能的有很多，笔者更推荐使用shared_preferences进行简单设置的存储。鸿蒙NEXT社区对该插件做了适配工作，使其可以应用于鸿蒙NEXT系统。
#### 插件配置
在pubspec.yaml中添加下面的代码，加入getx和shared_preferences的引用：
```
  get: ^4.6.5
  shared_preferences:
    git:
      url: "https://gitee.com/openharmony-sig/flutter_packages.git"
      path: "packages/shared_preferences/shared_preferences"
```
#### 同步插件
在终端输入如下命令，同步新加入的插件：
```
flutter pub get
```
### 2.存储功能的代码实现
我们中lib目录下新建common目录，将一些常用功能实现放入其中。其中再新建services目录存放服务功能，store目录里保存存储相关功能的文件。目录结构如下图所示：
![QQ_1730171420260.png](https://s2.loli.net/2024/10/29/DeIO5bjaR6YTi4M.png)
其中services.dart主要功能为导出所有服务项，代码如下：
```
library services;

export './storage.dart';
```
storage.dart提供了基础的存储服务，代码如下：
```
import 'package:get/get.dart';
import 'package:shared_preferences/shared_preferences.dart';

class StorageService extends GetxService {
  static StorageService get to => Get.find();
  late final SharedPreferences _prefs;

  Future<StorageService> init() async {
    _prefs = await SharedPreferences.getInstance();
    return this;
  }

  Future<bool> setString(String key, String value) async {
    return await _prefs.setString(key, value);
  }

  Future<bool> setBool(String key, bool value) async {
    return await _prefs.setBool(key, value);
  }

  Future<bool> setList(String key, List<String> value) async {
    return await _prefs.setStringList(key, value);
  }

  String getString(String key, {String def = ""}) {
    return _prefs.getString(key) ?? def;
  }

  bool getBool(String key, {bool def = false}) {
    return _prefs.getBool(key) ?? def;
  }

  List<String> getList(String key, {List<String> def = const []}) {
    return _prefs.getStringList(key) ?? def;
  }

  Future<bool> remove(String key) async {
    return await _prefs.remove(key);
  }
}
```
store.dart代码如下：
```
library store;

export './config.dart';
```
config.dart提供配置项管理功能，代码如下：
```
import 'package:get/get.dart';

import '../services/storage.dart';

class ConfigStore extends GetxController {
  static ConfigStore get to => Get.find();

  // 用户是否第一次打开app
  static const String keyStorageIsFirstOpen = 'is_first_open';
  bool _isFirstOpen = true;
  bool get isFirstOpen => _isFirstOpen;

  // 标记用户已打开APP
  Future<bool> saveAlreadyOpen() {
    return StorageService.to.setBool(keyStorageIsFirstOpen, false);
  }

  @override
  void onInit() {
    super.onInit();
    _isFirstOpen = StorageService.to.getBool(keyStorageIsFirstOpen, def: true);
  }
}
```
### 3.存储功能的初始化
通过上面的步骤，存储的功能已经实现了。在main.dart中，添加下面的代码，将对存储功能进行初始化，以便后期调用对应的功能。
```
import 'common/services/services.dart';
import 'common/store/store.dart';

Future init() async {
  WidgetsFlutterBinding.ensureInitialized();
  // 初始化存储服务
  await Get.putAsync<StorageService>(() => StorageService().init());
  // 创建配置库
  Get.put<ConfigStore>(ConfigStore());
}

void main() async {
  await init();

  runApp(const MyApp());
}
```
本章简单介绍了为什么要存储设置项、GetX与shared_preferences插件配置，以及存储功能的实现与初始化。接下来的文章继续结合GetX插件，利用上面的配置项存储功能，实现用户协议和隐私政策提醒页面。