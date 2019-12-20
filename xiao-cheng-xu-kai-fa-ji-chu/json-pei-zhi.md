---
description: 小程序的配置文件
---

# JSON 配置

### 全局配置

### app.json

```text
{
  "pages": [
    "wxlearn/wxlearn/begin",
    "wxlearn/index/index"
  ],
  "window": {
    "backgroundColor": "#111111",
    "backgroundTextStyle": "light",
    "navigationBarBackgroundColor": "#ffffff",
    "navigationBarTitleText": "MyTest",
    "navigationBarTextStyle": "black"
  },
  "sitemapLocation": "sitemap.json",
  "tabBar":{
    "list":[{
      "pagePath":"wxlearn/wxlearn/begin",
      "text":"首页"
    },{
      "pagePath":"wxlearn/index/index",
      "text":"副页"
    }]
  },
  "networkTimeout": {
    "request": 10000,
    "downloadFile": 20000,
    "debug":true
  }
}
```

### 页面配置 page.json



