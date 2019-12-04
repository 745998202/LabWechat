---
description: 小程序 - 事件
---

# 事件

### 1. 什么是事件

在小程序里边，我们把“用户在渲染层的行为反馈”以及“组件的部分状态反馈”抽象为渲染层传递给逻辑层的**“事件”**

![&#x6E32;&#x67D3;&#x5C42;&#x4EA7;&#x751F;&#x7528;&#x6237;&#x4EA4;&#x4E92;&#x4E8B;&#x4EF6;&#x4F20;&#x9012;&#x7ED9;&#x903B;&#x8F91;&#x5C42;](.gitbook/assets/image%20%283%29.png)

一个简单的事件处理小程序代码

```text
<!-- page.wxml -->
<view id="tapTest" data-hi="WeChat" bindtap="tapName"> Click me! </view>

// page.js
   Page({
  tapName: function(event) {
    console.log(event)
  }
})
```

事件是通过bindtap这个属性绑定在组件上的，同时在当前页面的Page构造器中定义对应的事件处理函数tapName,当用户点击该view区域时，达到触发条件生成事件tap,该事件处理函数tapName会被执行，同时还会收到一个事件对象event。

### 2. 事件类型和事件对象

前边说到触发事件是由“用户在渲染层的行为反馈"以及"组件的部分状态反馈"引起的。

由于不同组件的状态不一样，所以这里不讨论组件相关的事件（组件的事件可以参考其参数说明），详情见官方文档 [https://mp.weixin.qq.com/debug/wxadoc/dev/component/ ](https://mp.weixin.qq.com/debug/wxadoc/dev/component/%20)

常见的事件类型

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x7C7B;&#x578B;</th>
      <th style="text-align:left">&#x89E6;&#x53D1;&#x6761;&#x4EF6;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">touchstart</td>
      <td style="text-align:left">&#x624B;&#x6307;&#x89E6;&#x6478;&#x52A8;&#x4F5C;&#x5F00;&#x59CB;</td>
    </tr>
    <tr>
      <td style="text-align:left">touchmove</td>
      <td style="text-align:left">&#x624B;&#x6307;&#x89E6;&#x6478;&#x540E;&#x79FB;&#x52A8;</td>
    </tr>
    <tr>
      <td style="text-align:left">touchcancel</td>
      <td style="text-align:left">&#x624B;&#x673A;&#x89E6;&#x6478;&#x88AB;&#x6253;&#x65AD;&#xFF0C;&#x5982;&#x6765;&#x7535;&#x63D0;&#x9192;&#x6216;&#x5F39;&#x7A97;</td>
    </tr>
    <tr>
      <td style="text-align:left">touchend</td>
      <td style="text-align:left">&#x624B;&#x6307;&#x89E6;&#x6478;&#x52A8;&#x4F5C;&#x7ED3;&#x675F;</td>
    </tr>
    <tr>
      <td style="text-align:left">tap</td>
      <td style="text-align:left">&#x624B;&#x6307;&#x89E6;&#x6478;&#x540E;&#x9A6C;&#x4E0A;&#x79BB;&#x5F00;</td>
    </tr>
    <tr>
      <td style="text-align:left">longpress</td>
      <td style="text-align:left">
        <p>&#x624B;&#x6307;&#x89E6;&#x6478;&#x540E;&#xFF0C;&#x8D85;&#x8FC7;350ms&#x518D;&#x79BB;&#x5F00;&#xFF0C;&#x5982;&#x679C;&#x6307;&#x5B9A;&#x4E86;&#x4E8B;&#x4EF6;</p>
        <p>&#x56DE;&#x8C03;&#x51FD;&#x6570;&#x5E76;&#x89E6;&#x53D1;&#x4E86;&#x8FD9;&#x4E2A;&#x4E8B;&#x4EF6;&#xFF0C;tap&#x4E8B;&#x4EF6;&#x5C06;&#x4E0D;&#x518D;&#x88AB;&#x89E6;&#x53D1;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">longtap</td>
      <td style="text-align:left">&#x624B;&#x6307;&#x89E6;&#x6478;&#x540E;&#xFF0C;&#x8D85;&#x8FC7;350ms&#x518D;&#x79BB;&#x5F00;</td>
    </tr>
    <tr>
      <td style="text-align:left">transitioned</td>
      <td style="text-align:left">&#x4F1A;&#x5728;WXSS transition &#x6216; wx.createAnimation&#x52A8;&#x753B;&#x7ED3;&#x675F;&#x540E;&#x89E6;&#x53D1;</td>
    </tr>
    <tr>
      <td style="text-align:left">animationstart</td>
      <td style="text-align:left">&#x4F1A;&#x5728;WXSS animation&#x52A8;&#x753B;&#x5F00;&#x59CB;&#x65F6;&#x89E6;&#x53D1;</td>
    </tr>
    <tr>
      <td style="text-align:left">animationiteration</td>
      <td style="text-align:left">&#x4F1A;&#x5728;&#x4E00;&#x4E2A; WXSS animation&#x4E00;&#x6B21;&#x8FED;&#x4EE3;&#x7ED3;&#x675F;&#x65F6;&#x89E6;&#x53D1;</td>
    </tr>
    <tr>
      <td style="text-align:left">animationend</td>
      <td style="text-align:left">&#x4F1A;&#x5728;&#x4E00;&#x4E2A; WXSS animation&#x52A8;&#x753B;&#x5B8C;&#x6210;&#x65F6;&#x89E6;&#x53D1;</td>
    </tr>
  </tbody>
</table>当事件回调触发的时候，会收到一个事件对象，对象的详细属性如下

| 属性 | 类型 | 说明 |
| :--- | :--- | :--- |
| type | String | 事件类型 |
| timeStamp | Integer | 页面打开到触发事件所经过的毫秒数 |
| target | Object | 触发事件的组件的一些属性值集合 |
| currentTarget | Object | 当前组件的一些属性值集合 |
| detail | Object | 额外的信息 |
| touches | Array | 触摸事件，当前停留在屏幕中的触摸点信息的数组 |
| changedTouches | Array | 触摸事件，当前变化的触摸点信息的数组 |

{% hint style="info" %}
这里需要注意的是target和currentTarget的区别

currentTarget为当前事件所绑定的组件，而target则是触发该事件的源头组件
{% endhint %}

```text
<!-- page.wxml -->
<view id="outer" catchtap="handleTap">
    <view id="inner">点击我</view>
</view>
```



