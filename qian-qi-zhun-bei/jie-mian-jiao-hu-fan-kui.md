---
description: 对于用户的操作进行反馈
---

# 界面交互反馈

### 1. 触摸反馈

通常页面上会摆放一些button按钮或者view区域，用户触摸按钮之后会触发下一步操作。这种情况下，我们要对触摸这个行为给予用户一定的响应。

![&#x89E6;&#x6478;&#x533A;&#x57DF;&#x5E95;&#x8272;&#x53D8;&#x6210;&#x7070;&#x8272;](../.gitbook/assets/image%20%2818%29.png)

小程序的view容器组件和button组件提供了hover-class属性，触摸时会往该组件加上对应的class改变组件的样式

```text
//page.wxss
.hover{

background-color : gray;

}


<!--page.wxml -->
<button hover-class = "hover">点击button</button>
<view hover-class="hover">点击view</view>


```

对于用户的操作及时响应是非常优秀的体验，有时在点击button按钮处理更耗时的操作时，我们也会使用button组件的loading属性，在按钮的文字前边出现Loading，让用户明确感觉到，这个操作会比较耗时，需要等待一小段时间

![button&#x6587;&#x5B57;&#x524D;&#x51FA;&#x73B0;loading](../.gitbook/assets/image%20%2825%29.png)

```text
<!--page.wxml-->
<button loading="{{loading}}" bindtap="tap">操作</button>

<!--page.js-->
Page({

  data: { loading: false },

  tap: function() {

    // 把按钮的loading状态显示出来

    this.setData({

      loading: true

    })

    // 接着做耗时的操作

  }

})
```

### 2. Toast和模拟对话框

在完成某个操作成功之后，我们希望告诉用户这次操作成功并且不打断用户接下来的操作。弹出式提示Toast就是用在这样的场景上，Toast提示默认1.5秒后自动消失，其表现形式如下图所示

![Toast&#x5F39;&#x51FA;&#x63D0;&#x793A;](../.gitbook/assets/image%20%286%29.png)

小程序提供了显示隐藏Toast接口，代码示例如下

```text
Page({
onLoad:function(){

    wx.showToast({
    //显示Toast
    title:"已发送",
    icon:"success",
    duration:1500
    })
    
    
    //wx.hideToast()//隐藏Toast
}
})
```

特别要注意，我们不应该把Toast用于错误提示，因为错误提示需要明确告诉用户具体原因，因此不适合用这种一闪而过的Toast弹出式提示。一般需要用户知晓操作状态的话，会使用模态对话框来提示，同时附带下一步操作的指引

![](../.gitbook/assets/image%20%2824%29.png)

```text
  tips:function(){
    //错误提示
    wx.showModal({
      title: '标题',
      content: '告知当前状态，信息和解决方法',
      confirmText:'主操作',
      cancelText:'次要操作',
      success:function(res){
        if(res.confirm){
          console.log('用户点击主操作')
        }else if(res.cancel){
          console.log('用户点击次要操作')
        }
      }
    })
  }
```

### 3. 界面滚动

往往手机屏幕是承载不了所有信息的，所以内容区域肯定会超出屏幕区域，用户可以通过滑动屏幕来查看下一屏的内容，这是非常常见的界面滚动的交互。为了让用户可以快速刷新当前界面的信息，一般在小程序里会通过下拉整个界面这个操作来触发

![&#x4E0B;&#x62C9;&#x5237;&#x65B0;](../.gitbook/assets/image%20%2815%29.png)

```text
//page.json
{"enablePullDownRefresh":true}


//page.js
Page({
  onPullDownRefresh: function () {
    if (this.data.loading == true) {
      this.setData({ loading: false })
    } else if(this.data.loading==false) {
      this.setData({ loading: true })
    }
    console.log(this.data.loading)
    wx.stopPullDownRefresh()
  }
})
```

多数购物小程序会在首页展示一个商品列表，用户滚动到底部的时候，会加载下一页的商品列表渲染到列表的下方，我们把这个交互操作叫为上拉触底。宿主环境提供了上拉的配置和操作触发的回调，如下代码所示

```text
//page.json
{"onReachBottomDistance":100}

//page.js
Page({
onReachBottom: function(){
//当界面下方距离页面底部距离小于100像素时触发回调
}
})
```

当然有时候我们并不想整个页面进行滚动，而是页面中某一小块区域需要可滚动，此时就要用到宿主环境所提供的scroll-view可滚动视图组件。可以通过组件的scroll-x和scroll-y属性决定滚动区域是否可以横向或者纵向滚动。scroll-view组件也提供了丰富的滚动回调触发事件，详细内容可以查询官方文档

