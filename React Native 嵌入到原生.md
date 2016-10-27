# React Native 嵌入到原生

以下均以 LiveSDKTest 项目为例

#### 一、npm install

在iOS原生项目根目录下，新建 **package.json** 文件，例子内容如下：

```json
{
  "name": "LiveSDKTest",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "start": "node node_modules/react-native/local-cli/cli.js start"
  },
  "dependencies": {
    "react": "15.3.2",
    "react-native": "0.34.1"
  }
}
```

> *其中，“react”  “react-native” 为其版本号*

在终端cd到该目录，执行 `npm install`



#### 二、pod install

执行 **pod init** 创建 *Podfile* 文件，例子内容如下：

```ruby
target 'LiveSDKTest' do

  pod 'React', :path => './node_modules/react-native', :subspecs => [ 
    'Core',
    'RCTActionSheet',  #参照RN项目所包含的freamwork
    'RCTGeolocation',
    'RCTImage',
    'RCTNetwork',
    'RCTSettings',
    'RCTText',
    'RCTVibration',
    'RCTWebSocket', # needed for debugging
    # Add any other subspecs you want to use in your project
  ]
end
```

执行 `pod install` 

> 成功后尝试编译，若失败，进入 *LiveSDKTest  => Build Phases => Link Binary With Libraries* 添加 **libReact.a** framework文件



#### 三、index.ios.js

执行 **touch index.ios.js** 创建 React Native 入口文件 （注意自定义下面的项目名 "LIveSDKTest"）

```javascript
AppRegistry.registerComponent('LiveSDKTest', () => DemoClass);
```

执行 `mkdir RNComponent` 创建文件夹存放js文件（RN组件）

至此，你的项目文件结构应该是这样的：

![](http://ww4.sinaimg.cn/mw690/8c7512f7gw1f96olh21k2j207y09mjs3.jpg)

#### 四、 代码编写

RN组件由一个 `UIView` 作为容器，初始化方法如下(需要布局)，自行使用。

```objective-c
#import <RCTRootView.h>
...
  
NSURL *jsCodeLocation = [NSURL URLWithString:@"http://localhost:8081/index.ios.bundle?platform=ios"];
    
RCTRootView *rootView = [[RCTRootView alloc] initWithBundleURL:jsCodeLocation moduleName:@"LiveSDKTest" initialProperties:nil launchOptions:nil];
```

> 可直接赋值`ViewController.view`，也可作为子视图 `addSubview` ，等等

在 *info.plist*  添加 `App Transport Security Settings` 设置 `Allow Arbitrary Loads` 布尔值为 **YES**



### 至此，大公告成！接下来运行调试步骤与纯RN项目相同。