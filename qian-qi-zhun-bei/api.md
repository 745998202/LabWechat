---
description: 小程序API
---

# API

API的官方文档：[ https://mp.weixin.qq.com/debug/wxadoc/dev/api/](%20https://mp.weixin.qq.com/debug/wxadoc/dev/api/)

宿主环境提供了丰富的API，可以很方便调起微信提供的能力。

例如，wx.navigateTo可以保留当前页面，然后跳转到新的页面。这里的wx对象实际上就是小程序的宿主环境所提供的全局对象，**几乎所有的小程序API都挂载在wx对象下。**

**小程序提供的API按照功能主要分为几大类：**

* 网络
* 媒体
* 文件
* 数据缓存
* 位置
* 设备
* 界面
* 界面节点信息

#### API调用约定

1. wx.on\*开头的API是监听某个事件发生的API接口，接受一个Callback函数作为参数。当该事件触发时，会调用Callback函数
2. 如未特殊约定，多数API接口为异步接口，都接受一个Object作为参数
3. API的Object参数一般由success,fail,complete三个回调来接收接口调用结果
4. wx.get\* 开头的API是获取宿主环境数据的接口
5. wx.set\*开头的API是写入数据到宿主环境的接口

> 通过wx.request发起网络请求

```text
wx.request({
url: 'test.php',
data:{},
header:{'content-type':'application/json'},
success:function(res){
//收到http服务成功后触发
console.log(res.data)
},
fail:function(){
//发生网络错误等情况触发
},
complete:function(){
//成功或者失败后触发
}
})
```

                                                         API接口回调说明

| 参数名字 | 类型 | 必填 | 描述 |
| :--- | :--- | :--- | :--- |
| success | Function | 否 | 接口调用成功的回调函数 |
| fail | Function | 否 | 接口调用失败的回调函数 |
| complete | Function | 否 | 接口调用结束的回调函数（调用成功、失败都会执行） |

{% hint style="info" %}
还有需要注意的是API调用大多数都是异步的，其次，有部分API会拉起微信的原生界面，此时会触发Page的onHide方法，当用户从原生界面返回到小程序时，会触发Page的onShow方法
{% endhint %}

API的官方文档：[ https://mp.weixin.qq.com/debug/wxadoc/dev/api/](%20https://mp.weixin.qq.com/debug/wxadoc/dev/api/)

