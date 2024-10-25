## 鸿蒙NEXT+Flutter开发6-应用属性个性化

&emsp;&emsp;为了进一步表明应用的归属，需要对应用的各种属性进行调整，一般包括应用的图标、应用名称等，并且会加入欢迎屏改善用户打开应用时的使用体验。
### 1.修改应用图标
&emsp;&emsp;鸿蒙应用的图标文件为app_icon.png，其存储路径为：ohos/AppScope/resources/base/media/appicon.png，大小为114×114像素。将其替换为自己的图标文件即可。
![QQ_1729843991967.png](https://s2.loli.net/2024/10/25/HcK7y8F5qlDfPst.png)
### 2.修改应用标题为中文
&emsp;&emsp;修改文件ohos/entry/src/main/resources/zh_CN/element/string.json，将其中名称为EntryAbility_label的值修改为应用的中文标题：演示1。
```
    {
      "name": "EntryAbility_label",
      "value": "演示1"
    }
```
![QQ_1729849200984.png](https://s2.loli.net/2024/10/25/2I9dDMb815qyegm.png)
### 3.修改应用切换时显示的标题
&emsp;&emsp;创建的Flutter应用，其默认标题为“Flutter Demo”。为了得到更好的一致性，在VS Code中打开main.dart文件，将其中的title项修改为“演示1”，在对应用进行多语言处理时，将会讲到如何根据系统语言动态调整标题名称。
&emsp;&emsp;需要修改的代码如下：
```
  Widget build(BuildContext context) {
    return MaterialApp(
      title: '演示1', // 默认为：Flutter Demo
      theme: ThemeData(
        // This is the theme of your application.
```
![1b2743d47b844ea3b989998fb6942d6a.jpeg](https://s2.loli.net/2024/10/25/SGrZQRTt3zPdDkf.jpg)
### 4.修改应用启动时显示图标
&emsp;&emsp;在应用启动时，鸿蒙NEXT应用默认显示应用图标作为欢迎屏，需要将其修改为自己的图标。文件路径为：ohos/entry/src/main/resources/base/media/icon.png。
![QQ_1729848502183.png](https://s2.loli.net/2024/10/25/eNW562pEt1TnxKb.png)
### 5.添加渐变欢迎屏
&emsp;&emsp;由于Flutter窗口加载需要一定的时间，步骤4显示的图标欢迎屏消失之后，在Flutter主窗口出现之前，还有一个短暂的时间显示为空白屏，比较影响用户体验。故可以在Flutter主窗口显示之前，加入一个渐进渐出的处理，使得主窗口显示不是那么突兀。
&emsp;&emsp;使用DevEco Studio打开ohos目录，找到ohos/entry/src/main/ets/pages/index.ets，修改build函数代码如下：

```
  build() {
      Stack() {
        FlutterPage({ viewId: this.viewId })
        // 是否需要显示欢迎屏
        if(this.showSplash)
        {
          // 白底
          Rect()
            .fill(Color.White)
            .width('100%')
            .height('100%')
          // 图标
          Image($r('app.media.icon'))
            .objectFit(ImageFit.None)
            .borderRadius(500)
            .rotate({ angle: this.rotateValue })
            .opacity(this.opacityValue)
            .offset({ y: `-${'15%'}` })
            .animation({curve: Curve.EaseOut })
          // 应用名称
          Column() {
            Text($r('app.string.EntryAbility_label'))
              .fontColor(Color.Black)
              .fontSize('24fp')
              .fontWeight(FontWeight.Medium)
            // 公司名称
            Text($r('app.string.vendor_name'))
              .fontSize('16fp')
              .fontColor(Color.Black)
              .margin({ top: '15vp' })
            // 网址
            Text('www.cdrviewer.com')
              .fontSize('14fp')
              .fontColor(Color.Black)
              .margin({ top: '15vp' })
          }
          .rotate({ angle: this.rotateValue })
          // 控制透明度
          .opacity(this.opacityValue)
          .offset({ y: '25%' })
          // 控制动画曲线
          .animation({curve: Curve.EaseOut })
        }
      }
  }
```
&emsp;&emsp;其中this.showSplash用来控制是否显示欢迎屏，this.opacityValue用来控制显示的透明度。在aboutToAppear函数中启动定时器，aboutToDisappear函数中关闭定时器。这样就可以在Flutter主窗口出现之前，有2秒钟的渐进渐出动画，相对平滑的过渡到Flutter的主页面。主要代码如下：
```
  @State countdown: number = 2;
  @State showSplash: boolean = true;
  private timer: number = -1;

  @State animate: boolean = false;
  private opacityValue: number = 0;
  @State rotateValue: number = 0; // Rotation angle of component 1.

  aboutToAppear(): void {
    this.startTiming();
  }

  aboutToDisappear() {
    this.clearTiming();
  }

  startTiming() {
    this.timer = setInterval(() => {
      this.countdown--;
      if (this.countdown === 0) {
        this.clearTiming();
        this.showSplash = false;
      }
      this.animate = !this.animate;
      this.opacityValue = this.animate ? 1 : 0.3;
      this.rotateValue = this.animate ? 0.1 : -0.1;
    }, 1000);
    setTimeout(()=>{
      this.animate = !this.animate;
      this.opacityValue = this.animate ? 1 : 0;
      this.rotateValue = this.animate ? 0.1 : -0.1;
      }, 0
    );
}

  clearTiming() {
    if (this.timer !== -1) {
      clearInterval(this.timer);
      this.timer = -1;
    }
  }
```
&emsp;&emsp;欢迎屏页面如下所示：
![e78ae04e528bccf7bc45aad35e3c43e5.jpeg](https://s2.loli.net/2024/10/25/dyCc4O9lSz7U5BE.jpg)
&emsp;&emsp;通过前面的步骤，这样一个个性化的鸿蒙NEXT应用框架就做好了。其中欢迎屏中图标的大小，以及文字大小位置等，可以根据自己的需要进行调整。