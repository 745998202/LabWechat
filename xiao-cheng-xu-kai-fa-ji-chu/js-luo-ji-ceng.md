# JS 逻辑层

### 注册App\(\)方法

在逻辑层，App\(\)方法用来注册一个小程序。App\(\)仅接受一个object参数,用于指定小程序的生命周期函数等。App\(\)方法有且仅有一个，存在于app.js中。object参数说明参见下表

![](../.gitbook/assets/image%20%281%29.png)

{% hint style="info" %}
onLauch只触发一次
{% endhint %}

```text
// wxlearn/index/index.js
Page({

  /**
   * 页面的初始数据
   */
  data: {

  },

  /**
   * 生命周期函数--监听页面加载
   */
  onLoad: function (options) {

  },

  /**
   * 生命周期函数--监听页面初次渲染完成
   */
  onReady: function () {

  },

  /**
   * 生命周期函数--监听页面显示
   */
  onShow: function () {

  },

  /**
   * 生命周期函数--监听页面隐藏
   */
  onHide: function () {

  },

  /**
   * 生命周期函数--监听页面卸载
   */
  onUnload: function () {

  },

  /**
   * 页面相关事件处理函数--监听用户下拉动作
   */
  onPullDownRefresh: function () {

  },

  /**
   * 页面上拉触底事件的处理函数
   */
  onReachBottom: function () {

  },

  /**
   * 用户点击右上角分享
   */
  onShareAppMessage: function () {

  }
})
```

微信团队为开发者提供了全局的getApp\(\)函数，可以用来获取小程序实例。使用getApp\(\)

### 注册Page\(\)方法

| 参数 | 类型 | 描述 |
| :--- | :--- | :--- |
| data | Object | 页面的初始数据 |
| onLoad | Function | 生命周期函数——监听页面加载 |
| onReady | Function | 生命周期函数——监听页面初次渲染完成 |
| onShow | Function | 生命周期函数——监听页面显示 |
| onHide | Function | 生命周期函数——监听页面 隐藏 |
| onUnload | Function | 生命周期函数——监听页面卸载 |
| onPullDownRefresh | Function | 页面相关事件处理函数——监听用户下拉动作 |
| onReachBottom | Any | 开发者可以添加任意的函数或数据到object参数中，用this可以访问 |

```text
// wxlearn/index/index.js
Page({

  /**
   * 页面的初始数据
   */
  data: {

  },

  /**
   * 生命周期函数--监听页面加载
   */
  onLoad: function (options) {

  },

  /**
   * 生命周期函数--监听页面初次渲染完成
   */
  onReady: function () {

  },

  /**
   * 生命周期函数--监听页面显示
   */
  onShow: function () {

  },

  /**
   * 生命周期函数--监听页面隐藏
   */
  onHide: function () {

  },

  /**
   * 生命周期函数--监听页面卸载
   */
  onUnload: function () {

  },

  /**
   * 页面相关事件处理函数--监听用户下拉动作
   */
  onPullDownRefresh: function () {

  },

  /**
   * 页面上拉触底事件的处理函数
   */
  onReachBottom: function () {

  },

  /**
   * 用户点击右上角分享
   */
  onShareAppMessage: function () {

  }
})
```



