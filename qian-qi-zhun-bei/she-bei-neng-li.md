---
description: 几种移动端特殊的设备能力使用
---

# 设备能力

### 微信扫码能力

为了让用户减少输入，我们可以把复杂的信息编码成一个二维码，利用宿主环境wx.scanCode这个API调起微信扫一扫，用户扫码之后，wx.scanCode的success回调会收到这个二维码对应的字符串信息

例如餐厅点餐小程序，我们给餐厅每个餐桌编号1-100号，把这个数字编码到二维码中，扫码获得编号之后，就可以知道是哪一桌点的菜，大大提高了点餐体验和效率

```text
//page.js

Page({

  // 点击“扫码订餐”的按钮，触发tapScan回调

  tapScan: function() {

    // 调用wx.login获取微信登录凭证

    wx.scanCode({

      success: function(res) {

        var num = res.result // 获取到的num就是餐桌的编号

      }

    })

  }

})
```

还有很多场景可以结合微信扫码能力做到很好的体验，例如通过扫商品上的一维码做一个商品展示的小程序；通过扫共享单车上的二维码去开启单车。我们可以多思考如何利用这个扫码能力去替代一些繁琐的输入操作，让我们的小程序变得更加便捷。

### 获取网络状态

对于用户的不同需求可以对当前网络状态提供获取网络状态的的能力，做一些友好的体验提示

代码清单

```text
//page.js

Page({

  // 点击“预览文档”的按钮，触发tap回调

  tap: function() {

    wx.getNetworkType({

      success: function(res) {

        // networkType字段的有效值：

        // wifi/2g/3g/4g/unknown(Android下不常见的网络类型)/none(无网络)

        if (res.networkType == 'wifi') {

          // 从网络上下载pdf文档

          wx.downloadFile({

            url:'http://test.com/somefile.pdf',

            success: function (res) {

              // 下载成功之后进行预览文档

              wx.openDocument({

                filePath: res.tempFilePath

              })

            }

          })

        } else {

          wx.showToast({ title: '当前为非Wifi环境' })

        }

      }

    })

  }

})
```

某些情况下，我们的手机连接到网络的方式会动态变化，例如手机设备连接到一个信号不稳定的Wifi热点，导致手机会经常从Wifi切换到移动数据网络。小程序宿主环境也提供了一个可以动态监听网络状态变化的接口wx.onNetworkStatusChange，让开发者可以及时根据网络状况去调整小程序的体验，wx.onNetworkStatusChange这个接口的使用场景留给读者来思考。















