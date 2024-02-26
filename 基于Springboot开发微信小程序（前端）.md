# 基于Springboot开发微信小程序（前端）

# 前端

## 预览

​		前端：微信小程序；后端：java Springboot；数据库：mysql

​		本项目实现了简易个人markdown仓库（小程序版），效果如下：

![image-20240226143350372](WXMiniProgarmAboveSpringboot.assets/image-20240226143350372.png)

点进条目是对应的markdown文档：

![image-20240226143520305](WXMiniProgarmAboveSpringboot.assets/image-20240226143520305.png)

## 项目结构

![image-20240226143642237](WXMiniProgarmAboveSpringboot.assets/image-20240226143642237.png)

pages有5项，其中4项对应tabbar的4项，`pages`中的`markdown`用于展示markdown文档。

## 项目依赖

​		`mobx-miniprogram`，`mobx-miniprogram-building`，`vant`以及`towxml`

​		项目使用了`vant`组件库的tabbar，可以按需引入。`towxml`用于在小程序预览markdown。`mobx-miniprogram`用于实现tabbar的跳转（如果使用原生的tabbar可以只引入`towxml`）

​		引入`mobx-miniprogram`，`mobx-miniprogram-building`如下：

​		在终端输入命令：

```
npm install mobx-miniprogram mobx-miniprogram-building --save
```

​		然后点击小程序开发者工具的顶部“工具”->"构建npm"。

​		`vant`与`towxml`的引入查阅官方文档：

[vant]: https://vant-contrib.gitee.io/vant-weapp/#/home	"Vant Weapp"
[towxml]: https://github.com/sbfkcel/towxml

> 引入完后需要“构建npm”

## 页面实现

```html
<!-- 每个页面的wxml一样 -->
<view class="container">
  <!-- 使用wx:for指令遍历后端发送的数据 -->
  <view wx:for="{{dataList.data}}" wx:key="id" class="box" bindtap="gotoMarkdown" data-id="{{item.id}}">
    <view class="box-header">{{item.tittle}}</view>
    <view class="box-content">更新时间：{{item.updateTime}}</view>
  </view>
</view>
```

```css
/* 每个页面的wxss一样 */
.container {
  border-top: 1px solid #000;
  display: flex;
  flex-direction: column;
  align-items: center;
 justify-content: flex-start;
  height: 100%;  
  padding-top: 10rpx;
  padding-bottom: 140rpx;
}
.box {
  height: 78rpx;
  width: 88%;
  margin-bottom: 5rpx;
  margin-top:5rpx;
  padding: 20px;
  padding-top: 15rpx;
  border-radius: 5px;
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
}
.box-header {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 5px;
}
.box-content {
  font-size: 14px;
  color: #666666;
}
```

​		以上的wxml与wxss用于实现页面的基本布局与样式，页面的数据来自后端，js文件如下：

```js
const app = getApp();
Page({
  data: {
    dataList:[]
  },
  gotoMarkdown: function(event) {
    const id = event.currentTarget.dataset.id; // 获取点击的box的id属性值
    wx.navigateTo({
      url: '/pages/markdown/markdown?id=' + id, // 将id作为参数传递到markdown页面
    })
},
  onLoad: function () {
      // 页面加载时从后端获取数据
    wx.request({
      url: 'http://你的后端服务器/getPage/1',
      success: (res) => {
        this.setData({
          dataList: res.data // 将从后端获取的数据赋值给dataList属性      
        });
        console.log(this.data.dataList);
      },
      fail: (err) => {
        console.error('请求失败', err); // 输出错误信息
      }
    });
  },
})
```

​		微信小程序向后端发起请求，后端返回页面对应的文档的基本信息，在页面上渲染。（具体请求与相应见文档末尾）

​		json文件如下：

```json
{
  "usingComponents": {
    "van-empty": "@vant/weapp/empty/index",
    "van-button": "@vant/weapp/button/index",
    "towxml":"/towxml/towxml"
  }
}
```

## “markdown”页面实现

​		wxml：

```html
<!-- 在对应的WXML文件中 -->
<scroll-view scroll-y style="height: auto;">
    <towxml nodes="{{article}}" />
</scroll-view>
```

​		不用额外写wxss文件

​		js文件如下：

```js
const app = getApp();
Page({
  data: {
    isLoading: false,
    article: {},
  },
  onLoad: function (options) {
    const id = options.id; // 获取从上一个页面传递过来的id参数
    console.log('接收到的id参数为', id);
    // 页面加载时从后端获取数据
    wx.request({
      url: 'http://后端ip/getDoc/' + id,
      success: (res) => {
        let markdownString = res.data.data[0].docMain; // 获取从后端返回的data对象
        // 替换图片链接
        markdownString = markdownString.replace(/!\[.*?\]\((.*?)\)|<img src="(.*?)"(.*?)>/g, (match, p1, p2, p3) => {
          console.log("匹配成功")
          if (p1) {            
            return `<img src='https://gitee.com/gitee用户名/仓库/raw/main/${p1}'>`;
          } else {
            return `<img src='https://gitee.com/gitee用户名/仓库/raw/main/${p2}' ${p3}>`;
          }
        });
        console.log("打印markdownString")
        console.log(markdownString);
        let result = app.towxml(markdownString, 'markdown', {
          base: 'https://xxx.com',
          theme: 'light',
          events: {
            tap: (e) => {
              console.log('tap', e);
            }
          }
        });
        this.setData({
          article: result,
          isLoading: false
        });
      }
    }, );
  },
});
```

​		md文件以字符串的形式保存在mysql数据库，后端将mysql数据库的md文件也是以字符串的形式发送给前端，前端接收字符串在解析为md即可。

​	但是，保存在数据库中的md文件的图片的url是链接本地的，因此小程序中图片无法显示，代码中` markdownString = markdownString.replace()`就是就是为了解决这一问题，后面再仔细讲。

## 解决图片无法显示的问题

​		保存的md文档中的图片是以`![image-20240226143642237](WXMiniProgarmAboveSpringboot.assets/image-20240226143642237.png)`或`<img src="WXMiniProgarmAboveSpringboot.assets/image-20240226143642237.png" alt="image-20240226143642237" style="zoom:80%;" />`（如果图片有缩放）这样的形式保存的。

​		在微信小程序中，无法打开本地的`WXMiniProgarmAboveSpringboot.assets`，所以需要修改图片保存的路径。以保存到gitee为例（不保存到GitHub是因为众所周知的原因加载图片会比较慢），在gitee中新建一个仓库，用于保存小程序要展示的md文档的所有assets文件夹，用正则表达式遍历匹配保存Markdown文档的String字符串中的图片语法，然后替换为指定的图片链接：

```javascript
markdownString.replace(/!\[.*?\]\((.*?)\)|<img src="(.*?)"(.*?)>/g, (match, p1, p2, p3) => {
  console.log("匹配成功")
  if (p1) {            
    return `<img src='https://gitee.com/用户名/仓库名/raw/main/${p1}'>`;
  } else {
    return `<img src='https://gitee.com/用户名/仓库名/raw/main/${p2}' ${p3}>`;
  }
});
```

- `markdownString` 是一个包含 Markdown 文本的字符串。
- `replace` 方法接受一个正则表达式和一个回调函数。正则表达式 `/!\[.*?\]\((.*?)\)|<img src="(.*?)"(.*?)>/g` 匹配 Markdown 中的图片语法。
- 回调函数接受匹配到的结果和捕获组（即正则表达式中的括号内的内容）作为参数，然后根据匹配到的情况进行处理。
- 如果匹配到 `![...](...)` 形式的图片语法，使用 `p1` 中的内容构建新的图片链接。
- 如果匹配到 `<img src="..." ...>` 形式的图片语法，使用 `p2` 中的内容构建新的图片链接，并保留 `p3` 中的其他属性。
- 最终替换原始字符串中的图片语法为新的图片链接。



# 后端

​		见另一篇文档。



# 请求与响应

## 1. 进入页面时发起网络请求

1.1 基本信息

> 请求路径：/getPage
>
> 请求方式：GET
>
> 接口描述：该接口用于进入小程序页面时的文档预览



1.2 请求参数

参数格式：路径参数

参数说明：

| 参数名 | 类型   | 是否必须 | 备注     |
| ------ | ------ | -------- | -------- |
| style  | number | 必须     | 页面的ID |

请求参数样例：

```
/getPage/1
```



1.3响应数据

参数格式：application/json

参数说明：

| 参数名         | 类型      | 是否必须 | 备注                           |
| -------------- | --------- | -------- | ------------------------------ |
| code           | number    | 必须     | 响应码，1 代表成功，0 代表失败 |
| msg            | string    | 非必须   | 提示信息                       |
| data           | object[ ] | 必须     | 返回的数据                     |
| \|-id          | number    | 必须     | 文件对应的id                   |
| \|- tittle     | string    | 必须     | 文件名                         |
| \|- updateTime | string    | 必须     | 修改时间                       |



## 2. 跳转markdown页面时发起网络请求

2.1 基本信息

> 请求路径：/getDoc
>
> 请求方式：GET
>
> 接口描述：该接口用于获取markdown文件



2.2 请求参数

参数格式：路径参数

参数说明：

| 参数名 | 类型   | 是否必须 | 备注     |
| ------ | ------ | -------- | -------- |
| id     | number | 必须     | 文档的id |

请求参数样例：

```
/getDoc/1
```



2.3响应数据

参数格式：application/json

参数说明：

| 参数名     | 类型      | 是否必须 | 备注                           |
| ---------- | --------- | -------- | ------------------------------ |
| code       | number    | 必须     | 响应码，1 代表成功，0 代表失败 |
| msg        | string    | 非必须   | 提示信息                       |
| data       | object[ ] | 必须     | 返回的数据                     |
| \|-docMain | string    | 必须     | markdown文件                   |



