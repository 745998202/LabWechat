# 本地数据缓存

本地数据缓存就是把数据存储在用户本地，以提高小程序的获取数据速度，在特定场景下可以提高页面的渲染速度

### 读写本地数据缓存

小程序提供了读写本地数据缓存的接口。

wx.getStorage/wx.getStorageSync读取本地数据缓存

wx.setStorage/wx.setStorageSync写到数据缓存

其中Sync后缀接口表示是同步接口，执行完毕之后会立马返回，示例代码和参数说明如下所示

读取本地数据缓存

```text
wx.getStorage({
key:'key1',
success:function(res){
var value = res.data
},
fail : function(){
console.log('读取key1发生错误')
}
})

try{
var value2 = wx.getStorageSync('key2')
}catch(e){
    console.log('读取key2发生错误')
}
```

wx.getStorage/wx.getStorageSync详细参数

| **参数名** | **类型** | **必填** | **描述** |
| :--- | :--- | :--- | :--- |
| key | String | 是 | 本地缓存中指定的 key |
| success | Function | 否 | 异步接口调用成功的回调函数，回调参数格式: {data: key对应的内容} |
| fail | Function | 否 | 异步接口调用失败的回调函数 |
| complete | Function | 否 | 异步接口调用结束的回调函数（调用成功、失败都会执行） |

wx.setStorage/wx.setStorageSync写入本地数据缓存

```text
//异步接口在success/fail回调才知道写入成功与否
wx.getStorage({
key:"key",
data:"value1",
success:function(){
console.log('写入value成功')
}

fail:function(){
console.log('写入value发生错误')
}
})

try{
//同步接口立即写入
wx.setStorageSync('key','value2')
console.log('写入value2成功')
}catch(e){
console.log('写入value2发生错误')
}
```

wx.setStorage/wx.setStorageSync详细参数

| **参数名** | **类型** | **必填** | **描述** |
| :--- | :--- | :--- | :--- |
| key | String | 是 | 本地缓存中指定的 key |
| data | Object/String | 是 | 需要存储的内容 |
| success | Function | 否 | 异步接口调用成功的回调函数 |
| fail | Function | 否 | 异步接口调用失败的回调函数 |
| complete | Function | 否 | 异步接口调用结束的回调函数（调用成功、失败都会执行） |

### 缓存限制和隔离

不同小程序具有不同的缓存空间

每个小程序的缓存上限为10MB，再通过wx.setStorage写入缓存会触发fail回调

小程序的本地缓存不仅仅通过小程序这个纬度来隔离空间，考虑到同一个设备可以登陆不同微信用户，宿主环境还对不同用户的缓存进行了隔离，避免用户间的数据泄露。

由于本地缓存是放在当前设备，用户换设备之后无法从另一个设备读取到当前设备数据，因此用户的关键信息不建议只存在本地缓存，应该把数据放到服务器进行持久化存储。

### 利用本地缓存提前渲染界面

讨论一个需求：我们要实现一个购物商城小程序，首页是展示一堆商品的列表。一般的实现方法就是在页面onLoad回调之后通过wx.request向服务器发起一个请求去拉取首页列表数据，等到wx.request的success回调之后把数据通过setData渲染到界面上，如下代码所示

```text
Page({
onLoad: function(){
var that = this
wx.request({
url:'https://test.com/getproductlist',
success:function(res){
if(res.statusCode === 200)
    {
        this.setData({
            list:res.data.list
            })
    }
    }
})
}
})
```

利用缓存提前渲染界面

```text
Page({
onLoad : function(){
var that = this
var list = wx.getStorageSync("list")

if(list){
this.setData({
list:list
})
}


wx.request({
url:'https://test.com/getproductlist',
success:function(res){
    if(res.statusCode === 200){
    list = res.data.list

          that.setData({ // 再次渲染列表

            list: list

          })
    }
    
    wx.setStorageSync("list",list)//覆盖缓存数据
}
})
}
})
```

这种做法可以让用户体验你的小程序时感觉加载速度非常快

### 缓存用户登陆态SessionId

在4.4节我们说到处理用户登录态的一般方法，通常用户在没有主动退出登录前，用户的登录态会一直保持一段时间，就无需用户频繁地输入账号密码

如果我们把SessionId记录在JavaScript中的某个内存变量，当用户关闭小程序再进来小程序时，之前内存的SessionId已经丢失，此时我们就需要利用本地数据缓存的能力来持久化存储SessionId

```text
//Page.js
var app = getApp()
Page({
onLoad : function(){
//page.js

var app = getApp()

Page({

  onLoad: function() {

    // 调用wx.login获取微信登录凭证

    wx.login({

      success: function(res) {

        // 拿到微信登录凭证之后去自己服务器换取自己的登录凭证

        wx.request({

          url: 'https://test.com/login',

          data: { code: res.code },

          success: function(res) {

            var data = res.data

            // 把 SessionId 和过期时间放在内存中的全局对象和本地缓存里边

            app.globalData.sessionId =data.sessionId

            wx.setStorageSync('SESSIONID',data.sessionId)



            // 假设登录态保持1天

            var expiredTime = +new Date() +1*24*60*60*1000

            app.globalData.expiredTime =expiredTime

            wx.setStorageSync('EXPIREDTIME',expiredTime)

          }

        })

      }

    })

  }

})
```

在重新打开小程序的时候，我们把上次存储的SessionId内容取出来，恢复到内存

利用本地缓存恢复用户登陆态sessionId

```text
//app.js

App({

  onLaunch: function(options) {

    var sessionId =wx.getStorageSync('SESSIONID')

    var expiredTime =wx.getStorageSync('EXPIREDTIME')

    var now = +new Date()



    if (now - expiredTime <=1*24*60*60*1000) {

      this.globalData.sessionId = sessionId

      this.globalData.expiredTime = expiredTime

    }

  },

  globalData: {

    sessionId: null,

    expiredTime: 0

  }

})
```



