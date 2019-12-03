---
description: 小程序知识基础记录
---

# 小程序基础 - 程序与页面

### 1. 程序

> 程序构造器 App\(\)

宿主环境提供了一个App\(\)构造器来注册一个程序的App，需要留意的是App\(\)构造器必须卸载app.js中

#### 其他JS脚本中可以通过getApp\(\)获取App实例

```text
// other.js

var appInstance = getApp()
```

App 的调用方式如下所示

> App构造器接受一个Object参数，参数说明如下表，其中onLauch/onShow/onHide三个回调函数是App实例的生命周期函数

```text
// app.js

App({
  onLauch : function(options){},
  onShow: function(options) {},
  onHide: function() {},
  onError: function(msg) {},
  globalData: 'I am global data'
})
```

| 参数属性 | 类型 | 描述 |
| :--- | :--- | :--- |
| onLauch | Function | 当小程序初始化完成时，会触发onLauch\(全局只触发一次\) |
| onShow | Function | 当小程序启动时，从后台进入前台显示，会触发onShow |
| onHide | Function | 当小程序从前台进入后台，会触发onHide |
| onError | Function | 当小程序发生脚本错误，或者API调用失败时，会触发onError并带上信息 |
| 其他字段 | 任意 | 可以添加任意的函数或数据到Object参数中，在App实例回调this可以访问 |

#### 1.2. 程序的生命周期和打开场景

初次进入小程序-&gt;微信客户端初始化好宿主环境-&gt;从网络上下载或本地缓存中拿到代码包-&gt;注入到宿主环境中-&gt;初始化-&gt;onLaunch-&gt;用户关闭-&gt;onHide-&gt;用户打开onShow

> 小程序打开是有很多途径的，为了做不同的业务处理，小程序客户端会把打开方式带给onLaunch和onShow的调用参数options

```text
//onLaunch 和 onShow带参数

App({
    onLaunch: function(options){ console.log(options)},
    onShow: function(options){ console.log(options)}
})
```

> 表3-2 onLaunch,onShow参数

| 字段 | 类型 | 描述 |
| :--- | :--- | :--- |
| path | String | 打开小程序的页面路径 |
| query | Object | 打开小程序的页面参数query |
| scene | Number | 打开小程序的场景值，详细场景值请参考小程序官方文档 |
| shareTicket | String | shareTicket，详见小程序官方文档 |
| referrerInfo | Object | 当场景为由从另一个小程序或公众号或App打开时，返回此字段 |
| referrerInfo.appId | String | 来源小程序或公众号或App的 appId，详见下方说明 |
| referrerInfo.extraData | Object | 来源小程序传过来的数据，scene=1037或1038时支持 |

> 表3-3 以下场景支持返回 referrerInfo.appId

| 场景值 | 场景 | appId信息含义 |
| :--- | :--- | :--- |
| 1020 | 公众号 profile | 页相关小程序列表 返回来源公众号 appId |
| 1035 | 公众号自定义菜单 | 返回来源公众号 appId |
| 1036 | App 分享消息卡片 | 返回来源应用 appId |
| 1037 | 小程序打开小程序 | 返回来源小程序 appId |
| 1038 | 从另一个小程序返回 | 返回来源小程序 appId |
| 1043 | 公众号模板消息 | 返回来源公众号 appId |

#### 1.3. 小程序全局共享数据

小程序的JS运行在JSCore中，而小程序的每个页面各自有一个WebView线程进行渲染，所以小程序切换页面的时候，小程序的逻辑层依旧在同一个JSCore中，所以可以通过App实例下的属性来共享数据

```text
//app.js

App({
globalData ： 'I am global data' //全局共享数据
})

// 其他页面脚本other.js

var appInstance = getApp()
console.log(appInstance.globalData)//输出 I am global data
```

### 2.页面

#### 2.1 文件构成和路径

| 设置项 | 对应文件 |
| :--- | :--- |
| 界面 | WXML，WXSS |
| 配置 | JSON |
| 逻辑 | JS |

页面路径需要在app.json中的pages字段声明，否则这个页面不会被注册到宿主环境中。例如两个页面的文件相对路径分别为pages/index/page 和 page/other/other.

```text
// app.json

{

"pages" : [
"pages/index/page",//第一项默认为首页
"pages/other/other"
]

}
```

#### 2.2 页面构造器Page\(\)

宿主环境提供了Page\(\)构造器

```text
Page({
  data: { text: "This is page data." },
  onLoad: function(options) { },
  onReady: function() { },
  onShow: function() { },
  onHide: function() { },
  onUnload: function() { },
  onPullDownRefresh: function() { },
  onReachBottom: function() { },
  onShareAppMessage: function () { },
  onPageScroll: function() { }
})
```

> 表2.2 - 1 Page构造器的参数

| 参数属性 | 类型 | 描述 |
| :--- | :--- | :--- |
| data | Object | 页面的初始数据  |
| onLoad | Function | 生命周期函数--监听页面加载，触发时机早于onShow和onReady |
| onReady | Function | 生命周期函数--监听页面初次渲染完成,只会执行一次 |
| onShow | Function | 生命周期函数--监听页面显示，触发事件早于onReady |
| onHide | Function | 生命周期函数--监听页面隐藏 |
| onUnload | Function | 生命周期函数--监听页面卸载 |
| onPullDownRefresh | Function | 页面相关事件处理函数--监听用户下拉动作 |
| onReachBottom | Function | 页面上拉触底事件的处理函数 |
| onShareAppMessage | Function | 用户点击右上角转发 |
| onPageScroll | Function | 页面滚动触发事件的处理函数 |
| 其他 | Any | 可以添加任意的函数或数据，在Page实例的其他函数中用 this 可以访问 |

#### 2.3 页面生命周期和打开参数

onLoad 和 onReady只会调用一次

onShow每次打开都会调用

{% hint style="info" %}
在第一次打开时，三者的顺序为 onLoad &gt; onShow &gt; onReady
{% endhint %}

页面打开参数query

```text
// pages/list/list.js
// 列表页使用navigateTo跳转到详情页
wx.navigateTo({ url: 'pages/detail/detail?id=1&other=abc' })

// pages/detail/detail.js
Page({
  onLoad: function(option) {
        console.log(option.id)
        console.log(option.other)
  }
})

```

传入参数 在页面路径后使用英文 ? 分隔path和query部分，query部分的多个参数使用 & 进行分隔，参数的名字和值使用 key=value 的形式声明。



2.4 页面的数据

小程序的页面通过WXML进行描述，WXML可以通过数据绑定的语法绑定从逻辑层传过来的数据字段，这里所说的数据其实就是来自于页面Page构造器的Data字段，data参数是页面第一次渲染时从逻辑层传到渲染层的数据

```text
<!-- page.wxml -->
<view>{{text}}</view>
<view>{{array[0].msg}}</view>


// page.js
Page({
    data :{
        text : 'init data',
        arrag:[{msg:'1'}，{msg:'2'}]
    }
})
```

更新数据：宿主环境所提供的Page实例的原型中有setData函数，我们可以在Page实例下的方法调用this.setData把数据传递给渲染层，从而达到更新界面的目的。由于小程序的逻辑层和渲染层是在两个线程中运行，所以setData传递数据实际上是一个异步的过程，所以setData的第二个参数是一个callback回调，在这次setData对界面渲染完毕后触发

setData其一般调用格式是 setData\(data, callback\)，其中data是由多个key: value构成的Object对象。 代码清单3-11 使用setData更新渲染层数据

```text
// page.js
Page({
  onLoad: function(){
    this.setData({
      text: 'change data'
    }, function(){
      // 在这次setData对界面渲染完毕后触发
    })
  }
})
```

在实际开发中并不需要每次都将所有字段重新设置一遍，只需要把改变的值进行设置即可，宿主环境会自动把新改动的字段合并到渲染层对应的字段中。

```text
// page.js
Page({
  data: {
    a: 1, b: 2, c: 3,
    d: [1, {text: 'Hello'}, 3, 4]
  }
  onLoad: function(){
       // a需要变化时，只需要setData设置a字段即可
    this.setData({a : 2})
  }
})
```

{% hint style="info" %}
1. 直接修改 Page实例的this.data 而不调用 this.setData 是无法改变页面的状态的，还会造成数据不一致。
2. 由于setData是需要两个线程的一些通信消耗，为了提高性能，每次设置的数据不应超过1024kB。
3. 不要把data中的任意一项的value设为undefined，否则可能会有引起一些不可预料的bug
{% endhint %}

2.5 页面的用户行为

小程序宿主环境提供了四个和页面相关的用户行为回调：

1. 下拉刷新 onPullDownRefresh 监听用户下拉刷新事件，需要在app.json的window选项中或页面配置page.json中设置enablePullDownRefresh为true。当处理完刷新数据后，wx.stopPullDownRefresh可以停止当前页面的下拉刷新。
2. 上拉触底 onReachBottom 监听用户上拉触底事件。可以在app.json的window选项中或页面配置page.json中设置触发距离onReachBottomDistance。在触发距离内滑动期间,本事件只会被触发一次
3. 页面滚动onPageScroll 监听用户滑动页面事件，参数为Object，包含scrollTop字段，表示页面在垂直方向已滚动的距离（单位px）
4. 用户转发 onShareAppMessage 只有定义了此事件函数，右上角菜单才会显示转发按钮，在用户点击转发按钮的时候会调用，此事件需要return一个Object，包含title和path两个字段，用于自定义转发内容

```text
// page.js
Page({
onShareAppMessage: function () {
 return {
   title: '自定义转发标题',
   path: '/page/user?id=123'
 }
}
})
```

2.6 import !!! 页面的跳转和路由

一个小程序拥有多个页面，我们可以通过wx.navigateTo推入一个新的页面，如3-6所示，在首页使用2次wx.navigateTo后，页面层级会有三层，我们把这样的页面层级称之为页面栈

![&#x9875;&#x9762;&#x6808;](../.gitbook/assets/image%20%281%29.png)

后续为了表述方便，我们采用这样的方式进行描述页面栈：\[ pageA, pageB, pageC \]，其中pageA在最底下，pageC在最顶上，也就是用户所看到的界面，需要注意在本书编写的时候，小程序宿主环境限制了这个页面栈的最大层级为10层 ，也就是当页面栈到达10层之后就没有办法再推入新的页面了。我们下面来通过上边这个页面栈描述以下几个和导航相关的API。 使用 wx.navigateTo\({ url: 'pageD' }\) 可以往当前页面栈多推入一个 pageD，此时页面栈变成 \[ pageA, pageB, pageC, pageD \]。 使用 wx.navigateBack\(\) 可以退出当前页面栈的最顶上页面，此时页面栈变成 \[ pageA, pageB, pageC \]。 使用wx.redirectTo\({ url: 'pageE' }\) 是替换当前页变成pageE，此时页面栈变成 \[ pageA, pageB, pageE \]，当页面栈到达10层没法再新增的时候，往往就是使用redirectTo这个API进行页面跳转。 小程序提供了原生的Tabbar支持，我们可以在app.json声明tabBar字段来定义Tabbar页（注：更多详细参数见Tabbar官方文档 ）。

```text
{
  "tabBar": {
    "list": [
      { "text": "Tab1", "pagePath": "pageA" },
      { "text": "Tab1", "pagePath": "pageF" },
      { "text": "Tab1", "pagePath": "pageG" }
    ]
  }
}
```

我们可以在刚刚的例子所在的页面栈中使用wx.switchTab\({ url: 'pageF' }\)，此时原来的页面栈会被清空（除了已经声明为Tabbar页pageA外其他页面会被销毁），然后会切到pageF所在的tab页面，页面栈变成 \[ pageF \]，此时点击Tab1切回到pageA时，pageA不会再触发onLoad，因为pageA没有被销毁。 补充一下，wx.navigateTo和wx.redirectTo只能打开非TabBar页面，wx.switchTab只能打开Tabbar页面。 我们还可以使用 wx. reLaunch\({ url: 'pageH' }\) 重启小程序，并且打开pageH，此时页面栈为 \[ pageH \]。表3-5罗列了详细的页面路由触发方式及页面生命周期函数的对应关系。



页面路由触发方式及页面生命周期函数的对应关系

| 路由方式 | 触发时机 | 路由前页面生命周期 | 路由后页面生命周期 |
| :--- | :--- | :--- | :--- |
| 初始化 | 小程序打开的第一个页面 |  | onLoad, onShow |
| 打开新页面 调用 | API wx.navigateTo | onHide | onLoad, onShow |
| 页面重定向 调用 | API wx.redirectTo | onUnload | onLoad, onShow |
| 页面返回 调用 | API wx.navigateBack | onUnload | onShow |
| Tab | 切换 调用 API wx.switchTab | 请参考表3-6 | 请参考表3-6 |
| 重启动 | 调用 API wx.reLaunch | onUnload | onLoad, onShow |

页面路由触发方式及页面生命周期函数的对应关系

| 当前页面 | 路由后页面 | 触发的生命周期（按顺序） |
| :--- | :--- | :--- |
| A | A | 无 |
| A | B | A.onHide\(\), B.onLoad\(\), B.onShow\(\) |
| A | B\(再次打开\) | A.onHide\(\), B.onShow\(\) |
| C | A | C.onUnload\(\), A.onShow\(\) |
| C | B | C.onUnload\(\), B.onLoad\(\), B.onShow\(\) |
| D | B | D.onUnload\(\), C.onUnload\(\), B.onLoad\(\), B.onShow\(\) |
| D\(从转发进入\) | A | D.onUnload\(\), A.onLoad\(\), A.onShow\(\) |
| D\(从转发进入\) | B | D.onUnload\(\), B.onLoad\(\), B.onShow\(\) |

