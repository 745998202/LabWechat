---
description: 小程序对于不同环境如何兼容
---

# 兼容

小程序的宿主环境一直在迭代更新，提供更多的能力给开发者去完成更多的事情，所以你的小程序会运行在不同版本的宿主环境下。为了让你的小程序在不同环境下都能提供相应的服务，我们需要来了解一下在小程序中如何实现兼容办法。



我们可能需要针对不同手机进行程序上的兼容，此时可以使用 **wx.getSystemInfo** 或者 **wx.getSystemInfoSync** 来获取手机品牌、操作系统版本号、微信版本号以及小程序基础库版本号等，通过这个信息，我们可以针对不同平台做差异化的服务。

代码清单

```text
wx.getSystemInfoSync()
/*
{
brand : "iPhone",
model : "iPhone6",
platform: "ios",
system : "ios 9.3.4",
version : "6.5.23",
SDKVersion : "1.7.0"
language: "zh_CN",    // 微信设置的语言
pixelRatio: 2,        // 设备像素比
screenWidth: 667,    // 屏幕宽度
screenHeight: 375,     // 屏幕高度
windowWidth: 667,    // 可使用窗口宽度
windowHeight: 375,     // 可使用窗口高度
fontSizeSetting: 16   // 用户字体大小设置
}
*/
```

随着新版本宿主环境的更新，新版本的宿主环境会提供一些新的API，可以通过判断此API是否存在来做程序上的兼容

```text
if(wx.openBluetoothAdapter){
    wx.openBluetoothAdapter()
}else{
    //如果希望用户在新版本上体验你的小程序，可以这样子提示
    wx.showModel({
    title : '提示',
    content : '当前微信版本过低，无法使用该功能，请升级到最新版本后重试'
    })
}
```

小程序还提供了wx.canIUse这个API，用于判断接口或者组件在当前宿主环境是否可用

其参数格式为 ${API}.${method}.${param}.${options}或者

${component}.${attribute}.${option}

各个段的含义如下

* ${API}代表API名字
* ${method}代表调用方式，有效值为return ,success,object,callback
* ${param}代表参数或返回值
* ${options}代表参数可选值
* ${component}代表组件名字
* ${attribute}代表组件属性
* ${option}代表组件属性的可选值

示例代码：

```text
//判断其接口在宿主环境是否可用
wx.canIUse(openBluetoothAdapter)
wx.canIUse('getSystemInfoSync.return.screenWidth')
wx.canIUse('getSystemInfo.success.screenWidth')
wx.canIUse('showToast.object.image')
wx.canIUse('onCompassChange.callback.direction')
wx.canIUse('request.object.method.GET')

//判断组件及其属性在宿主环境是否可用
wx.canIUse('contact-button')
wx.canIUse('text.selectable')
wx.canIUse('button.open-type.contact')
```

我们可以选择合适的判断方法来做小程序的向前兼容，以保证我们的小程序在旧版本的微信客户端也能工作正常。在不得已的情况下（小程序强依赖某个新的API或者组件时），还可以通过在小程序管理后台设置“基础库最低版本设置”来达到不向前兼容的目的。例如你选择设置你的小程序只支持1.5.0版本以上的宿主环境，那么当运行着1.4.0版本宿主环境的微信用户打开你的小程序的时候，微信客户端会显示当前小程序不可用，并且提示用户应该去升级微信客户端。

