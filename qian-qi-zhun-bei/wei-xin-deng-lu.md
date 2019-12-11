# 微信登陆

已经有的互联网产品在接入小程序会面临一些和登陆态相关的问题：怎么获取微信登陆态；怎么把微信账号和自己的账号进行打通。在这一节中，我们来介绍一下如何把微信小程序应用到你的小程序中。

我们先来看一看微信登陆的整个过程，如图所示

![&#x5FAE;&#x4FE1;&#x767B;&#x9646;&#x7684;&#x6574;&#x4E2A;&#x8FC7;&#x7A0B; ](../.gitbook/assets/image%20%286%29.png)

我们依次来分解一下图中的七个步骤，其中，第一步到第四步我们分别用一个小节来讲述，第五步到第七部都和SessionId相关，我们放在4.5.5节一起讨论

### 获取微信登陆凭证Code

凭证Code是用户的临时身份证，可以用来获取用户的个人信息，具有时效性，防止出现安全问题

### 发送Code到开发者服务器

在wx.login的success回调中拿到微信登陆凭证，紧接着会通过wx.request把Code传到开发者服务器，为了后续可以换取微信用户身份id。如果当前微信用户还没有绑定当前小程序业务的用户身份，那么这次请求应该顺便把用户输入的账号密码一起带到后台，然后开发者服务器就可以校验账号密码之后再和微信用户id进行绑定，小程序的示例代码如下所示

wx.login获取到code后

```text
Page({
tapLogin:function(){
wx.login({
success:function(res){
if(res.code){
wx.request
({
    url:'https://test.com/login',
    data:{
    username:'zhangsan',
    password:'pwd123456',
    code:res.code
    },
    success: function(res)
        {
        if(res.statusCode == 200)
            {
                console.log(res.data.sessionId)//服务器回包内容
            }
        }
})
}else
    {
        console.log('获取用户登陆态失败！'+res.errMsg)
    
    }
}
});
}
})
```

#### 到微信服务器换取微信用户身份id

到了第三步，开发者的后台就拿到了前边wx.login\(\)所生成的微信登陆凭证code，此时就可以拿这个code到微信服务器换取微信用户的身份。微信服务器为了确保拿code过来换取身份信息的人就是刚刚对应的小程序开发者，到微信服务器的请求的同时带上AppI和AppSecret，这两个信息在小程序管理平台的开发设置界面可以看到，由此可以看出，AppId和AppSecret是微信鉴别开发者身份的重要信息，AppId是公开信息，泄露AppId不会带来安全风险，但是AppSecret是开发者的隐私数据不应该泄露，如果发现泄露需要小程序管理平台进行重置AppSecret，而code在成功换取一次信息之后也会立即失效，即便code生成时间还没过期

开发者服务器和微信服务器通信也是https协议，微信服务器提供的接口地址是[https://api.weixin.qq.com/sns/jscode2session?appid=&lt;AppId&gt;&secret=&lt;AppSecret&gt;&js\_code=&lt;code&gt;&grant\_type=authorization\_code](https://api.weixin.qq.com/sns/jscode2session?appid=<AppId>&secret=<AppSecret>&js_code=<code>&grant_type=authorization_code)

URL的query部分参数中&lt;AppId&gt;,&lt;AppSecret&gt;,&lt;code&gt;就是前文所提到的三个信息，请求参数合法的话，接口会返回一下字段

jscode2session接口返回字段



| **字段** | **描述** |
| :--- | :--- |
| openid | 微信用户的唯一标识 |
| session\_key | 会话密钥 |
| unionid | 用户在微信开放平台的唯一标识符。本字段在满足一定条件的情况下才返回。 |

我们暂时只需要关注前两个字段即可，openId就是前文一直提到的微信用户Id，可以用这个id来区分不同的微信用户。session\_key则是微信服务器给开发者服务器颁发的身份证明，开发者可以用session\_key请求微信服务器的其他接口来获取一些其他信息，由此可以看到，session\_key不应该泄露或者下发到小程序前端。

可能我们会好奇为什么要设计session\_key,如果我们每次都通过小程序前端wx.login\(\)生成微信登陆凭证code去微信服务器请求信息，步骤太多造成整体耗时比较严重，因此对于一个比较可信的服务端，给开发者服务器颁发一个时效性更长的绘画密钥就显得很有必要了。session\_key也存在过期时间，因为篇幅的原因不展开

### 绑定微信用户身份id和业务身份id



