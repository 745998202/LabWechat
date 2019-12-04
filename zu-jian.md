---
description: 小程序组件
---

# 组件

组件属性官方文档：[https://mp.weixin.qq.com/debug/wxadoc/dev/component/](https://mp.weixin.qq.com/debug/wxadoc/dev/component/)

一个小程序页面由多个部分组成，组件就是小程序的基本组成单元。

组件是在WXML模板文件声明中使用的，WXML的语法和HTML语法相似，小程序使用标签名来引用一个组件，通常包含**开始标签**和**结束标签**，该标签的属性用来描述该组件

```text
<!-- page.xml -->

<image mode = "scaleToFile" src="img.png"></image>
```

{% hint style="info" %}
所有组件都是小写，多个英文单词之间以英文横杠进行连接“-”
{% endhint %}

```text
<!-- page.wxml -->
<view>
  <image mode="scaleToFill" src="img.png"></image>
  <view>
    <view>1</view>
    <view>2</view>
    <view>3</view>
  </view>
</view>
```

所有组件都具有下表列出的属性，主要涉及样式和事件绑定



| 属性名 | 类型 | 描述 | 其他说明 |
| :--- | :--- | :--- | :--- |
| id | String | 组件的唯一标示 | 保持整个页面唯一 |
| class | String | 组件的样式类 | 在对应的WXSS中定义的样式类 |
| style | String | 组件的内联样式 | 可以通过数据绑定进行动态设置的内联样式 |
| hidden | Boolean | 组件是否显示 | 所有组件默认显示 |
| data-\* | Any | 自定义属性 | 组件上触发的事件时，会发送给事件处理函数 |
| bind _/ catch_ | EventHandler | 事件 | 详情见**事件**章节 |

组件都有各自自定义的属性，可以对该组件的功能或者样式进行修饰，以image图片组件为例



| 属性名 | 类型 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- |
| src | String |  | 图片资源地址 |
| mode | String | 'scaleToFill' | 图片裁剪、缩放的模式 |
| lazy-load | Boolean | false | 图片懒加载。只针对page与scroll-view下的image有效 1.5.0 |
| binderror | HandleEvent |  | 当错误发生时触发事件，事件对象event.detail = {errMsg: 'something wrong'} |
| bindload | HandleEvent |  | 当图片载入完毕时触发事件，事件对象event.detail = {height:'图片高度px', width:'图片宽度px'} |

其他组件属性官方文档：[https://mp.weixin.qq.com/debug/wxadoc/dev/component/](https://mp.weixin.qq.com/debug/wxadoc/dev/component/)

