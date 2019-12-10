---
description: 小程序进行网络通信的组件
---

# 发起HTTPS网络通信

小程序经常要往服务器传递数据或者从服务器拉取信息，这个时候可以使用wx.request这个API，在这一节我们会重点讨论wx.request的使用和注意事项。为了叙述方便，假设我们的服务器域名是test.com

### 1. wx.request接口

如果我们需要从https://test.com/getinfo拉取用户信息，其代码示例如下所示，详细参数如下表所示

代码清单wx.request调用示例

```text
wx.request({
    url: 'http://test.com/getinfo',
    success: function(res){
    console.log(res)//服务器回包信息
    }
})
```



| **参数名** | **类型** | **必填** | **默认值** | **描述** |
| :--- | :--- | :--- | :--- | :--- |
| url | String | 是 |  | 开发者服务器接口地址 |
| data | Object/String | 否 |  | 请求的参数 |
| header | Object | 否 |  | 设置请求的 header，header 中不能设置 Referer，默认header\['content-type'\] = 'application/json' |
| method | String | 否 | GET | （需大写）有效值：OPTIONS, GET, HEAD, POST, PUT, DELETE, TRACE, CONNECT |
| dataType | String | 否 | json | 回包的内容格式，如果设为json，会尝试对返回的数据做一次 JSON解析 |
| success | Function | 否 |  | 收到开发者服务成功返回的回调函数，其参数是一个Object，见表4-2。 |
| fail | Function | 否 |  | 接口调用失败的回调函数 |
| complete | Function | 否 |  | 接口调用结束的回调函数（调用成功、失败都会执行） |

### 服务器接口

url 参数是当前发起请求的服务器接口地址,小程序宿主环境要求request发起的网络请求必须是https协议请求，因此开发者服务器必须提供HTTPS服务的接口，同时为了保证小程序不乱用任意域名的服务，wx.request请求的域名需要在小程序管理平台进行配置，如果小程序正式版使用wx.request请求未配置的域名，在控制台会有相应的报错

一般我们在开发阶段时，处于开发阶段的服务器接口还没部署到现网域名下，通常会有另一个域名来进行开发调试，考虑到这一点，为了方便开发者进行开发调试，开发者工具、小程序的开发版和小程序的体验版在某些情况下允许wx.request请求任意域名

由于我们一直在迭代更新小程序，那么就会有一个问题:在新版小程序发布的某段时间内，会有部分用户使用旧版的小程序。如果接口需要支持新的特性需要修改返回的数据格式，那接口的参数和返回字段至少向前兼容一个版本。

### 请求参数

通过wx.request这个API，有两种方法把数据传递到服务器：通过url上的参数以及通过data参数。举个例子：我们需要向服务器拿id为1的用户信息，同时我们把当前小程序版本带给服务器，让服务器可以做新旧版逻辑兼容，两种方法的代码示例如下所示

```text
//通过url参数传递数据
wx.request({
url:'http://test.com/getinfo?id=1&version=1.0.0',
success:function(res){
console.log(res)//服务器回包信息
}

})


//通过data参数传递数据
wx.request({
url:'https://test.com/getinfo',
data:{id:1,version:'1.0.0'},
success:function(res)//服务器回包信息
})
```

两种实现方式在HTTP GET请求下表现几乎是一样的，需要留意的是url是有长度限制的，其最大长度是1024字节，同时url上的参数需要拼接到字符串里，参数的值还要做一次urlEncode。向服务端发送的数据超过1024字节时，就要采用HTTP POST的形式，此时传递的数据就必须要使用data参数，基于这个情况，一般建议需要传递数据时，使用data参数来传递。

我们再来单独看看POST请求的情况,并不是所有的请求都是按照键值对的形式传到后台服务器的，有时候还会用到JSON。此时我们在wx.request的header参数设置content-type头部为application/json，小程序发起的请求的包体内容就是data参数对应的JSON字符串

```text
//请求的包体为{"a":{"b":[1,2,3],"c":{"d":"test"}}}
wx.request({
url:'https://test.com/postdata',
method:'POST',
header:{'content-type':'application/json'},
data:{
    a:{
    b:[1,2,3],
    c:{d:"test"}
    }
},
success:function(res){
console.log(res)//服务器回包信息
}
})
```

### 收到回包

通过wx.request发送请求后，服务器处理请求并返回HTTP包，小程序端收到回包后会触发success回调，同时回调会带上一个Object信息

success返回参数

| **参数名** | **类型** | **描述** |
| :--- | :--- | :--- |
| data | Object/String | 开发者服务器返回的数据 |
| statusCode | Number | 开发者服务器返回的 HTTP 状态码 |
| header | Object | 开发者服务器返回的 HTTP Response Header |

尤其注意，只要成功收到服务器返回，无论HTTP状态码是多少都会进入success回调。因此开发者自己通过对回包的返回码进行判断后再执行后续的业务逻辑

success回调的参数字段data字段类型是根据header\['content-type'\]决定的，默认header\['content-type'\]是application/json,在触发success回调之前，小程序宿主环境会对data字段的值做JSON解析，如果解析成功，那么data字段的值会被设置成解析后的object对象，其他情况data字段都是String类型，其值为HTTP回包包体。

### 一般使用技巧

1. 设置超时时间

小程序发出一个HTTPS网络请求，有时网络存在一些异常或者服务器存在问题，在经过一段时间后仍然没有收到网络回包，我们把这一段等待的最长时间称为请求超时时间。小程序的request默认超时时间是60，一般来说，我们不需要这么长的等待时间才收到回包，可能在等待3秒后还没收到回包就需要给用户一个明确的服务不可用的提示。在小程序项目根目录里面的app.json可以指定request超时时间

```text
{
"networkTimeout":{
"request":3000
}
}
```

2. 请求前后的状态处理

大部分场景可能是这样的，用户点击一个按钮，界面出现“加载中...”的Loading界面，然后发送一个请求到后台，后台返回成功直接进入下一个业务逻辑处理，后台返回失败或者网络异常等情况则显示一个“系统错误”的Toast,同时一开始的Loading界面会消失。我们给出一个常见的wx.request的示例代码，如下所示

```text
var hasClick = false;

Page({

  tap: function() {

    if (hasClick) {

      return

    }

    hasClick = true

    wx.showLoading()



    wx.request({

      url: 'https://test.com/getinfo',

      method: 'POST',

      header: { 'content-type':'application/json' },

      data: { },

      success: function (res) {

        if (res.statusCode === 200) {

          console.log(res.data)// 服务器回包内容

        }

      },

      fail: function (res) {

        wx.showToast({ title: '系统错误' })

      },

      complete: function (res) {

        wx.hideLoading()

        hasClick = false

      }

    })

  }

})  
```

为了防止用户极快速度触发两次tap回调，我们还加了一个hasClick的锁，在开始请求前检查是否已经发起过请求，如果没有才发起这次请求，等到请求返回之后再把锁的状态恢复回去

### 排查异常的方法



在使用wx.request接口我们会经常遇到无法发起请求或者服务器无法收到请求的情况，我们罗列排查这个问题的一般方法：

1. 检查手机网络状态以及wifi连接点是否工作正常。
2. 检查小程序是否为开发版或者体验版，因为开发版和体验版的小程序不会校验域名。
3. 检查对应请求的HTTPS证书是否有效，同时TLS的版本必须支持1.2及以上版本，可以在开发者工具的console面板输入showRequestInfo\(\)查看相关信息。
4. 域名不要使用IP地址或者localhost，并且不能带端口号，同时域名需要经过ICP备案。
5. 检查app.json配置的超时时间配置是否太短，超时时间太短会导致还没收到回报就触发fail回调。
6. 检查发出去的请求是否302到其他域名的接口，这种302的情况会被视为请求别的域名接口导致无法发起请求。





