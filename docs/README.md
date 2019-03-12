
``` bash
# install dependencies
$ cnpm i

# serve with hot reload 
$ npm run dev

# serve with hot reload production
npm run dev:prod

# fix eslint
$ npm run lint-fix
```


## Dependence 
> [flyio](https://github.com/wendux/fly/blob/master/README-CH.md) 同时支持浏览器、小程序、Node的基于Promise的跨平台http请求库。

> [minapp-api-promise](https://github.com/bigmeow/minapp-api-promise) 将所有异步微信小程序API promise化，支持then/catch、async/await的方式调用小程序API。

> [mpvue-entry](https://github.com/F-loat/mpvue-entry) 集中式页面配置。

> [mpvue-router-patch](https://github.com/F-loat/mpvue-router-patch) 在 mpvue 中使用 vue-router 兼容的路由写法。

> [mpvue-img-load](https://github.com/huangjinlin/mpvue-img-load) 小程序图片预加载组件 (不适合大量图片)
           

> 自动注册store

> 适配不同的编辑器IDE (.less 文件)
```
width: ~"40rpx"
```
即 width: 40rpx

1. inline-style 中只能用 rpx

2. `<style></style>` 标签内 既可 px 也可 rpx

> [命名](https://github.com/zhaotoday/bem)

### 图片3种方案

#### 1. OSS (较大图片)

> OSS 图片路径

```
  wbiao-static/mjmp/页面/图.png
```

#### 2. 小图片

> [webpack-spritesmith](https://github.com/qq20004604/webpack-study/tree/master/8%E3%80%81%E6%8F%92%E4%BB%B6/webpack-spritesmith) 雪碧图方案

#### 3. 自定义组件

## TIP

1.页面背景 需要 （ 非 scoped ）

```
<style>
  page {
    background-color: #f1f1f1;
  } 
</style>

// 其他
<style scoped>

</style>
```

2.`<open-data></open-data>` 样式设置

```
open-data[type="userAvatarUrl"] {
  display: block;
  overflow: hidden;
  .circle(68px);
}
```

3.定位

 [配置提示](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#permission)
 
4.sprites（雪碧图）重新生成，再次上传至 OSS 旧的 sprites 图被缓存

只是清除缓存，对于微信开发者工具，可以“清缓存”

通过在 sprites 图片 url 后加 ```时间戳``` 可以处理

[小程序图片缓存](https://developers.weixin.qq.com/community/develop/doc/0000ce3fc340b87cdbe776d4656400?highLine=%25E5%25B0%258F%25E7%25A8%258B%25E5%25BA%258F%25E5%259B%25BE%25E7%2589%2587%25E7%25BC%2593%25E5%25AD%2598)

eg.

```
https://static.wbiao.co/mjmp/sprite/generated-sprite.png?timepsan=1547707964163
```

5.[PhotoShop阴影效果转换成css中box-shadow](https://www.jianshu.com/p/f0b7dc56ab4a)

```
h-shadowt = Distance * cos(180 - Angle)
v-shadowt = Distance * sin(180 - Angle)

spread = Spread * Size
blur = Size - spread
```

```
box-shadow: h-shadow v-shadow blur spread color inset;
```


## 优化

1. 按需引用

2. 图片处理

## 坑

1.mpvue-cli  运行 ```npm run dev``` 有时候热更新了，但没把 vue-component 解析对

> 中止, 重新 npm run dev

2.增加页面或设置页面属性，不热更新

> 中止, 重新 npm run dev

3.阿里云 OSS 图片上传 "curl error"

> 重新导出图片

4.vue-component props 设置 required: true 无效

> 手动抛出错误

5.接口-列表

> 分页的第一页是 0 ``` pager: { now 0 } ```

> 总条数 total 是 第一页返回的 total，其他 total 是错的

6.watch props A

> 如果 A 本身只被 其他 watch 改变，则 watch 不到 A 的改变

7.node_modules mpvue-router-patch@0.2.1 push 函数（$router.push) query 处理 带 '?' 和 '&' 被截断

```js
function push(location, complete, fail, success) {
  let url = parseUrl(location);
  let params = { url: url, complete: complete, fail: fail, success: success };

  if (location.isTab) {
    wx.switchTab(params);
    return;
  }
  if (location.reLaunch) {
    wx.reLaunch(params);
    return;
  }
  // wx.navigateTo 对 params : { url } 的处理 对 '?' 和 '&' 做了处理
  // https://developers.weixin.qq.com/miniprogram/dev/api/wx.navigateTo.html
  
  wx.navigateTo(params);
}

```
eg.
```js
this.$router.push({
  path: '',
  query: {
    url:'something?status=8,6'
  }
});

// 预期表现
// query { url:'something?status=8,6' }

// 实际表现
// query { url: 'something' }

```

> 先 encodeURIComponent 再 decodeURIComponent
 
8.倒计时卡

[解决方法](https://developers.weixin.qq.com/community/develop/doc/0006a6c1bc4aa88ebcd73fb7156400)

9.mpvue 同一路由切换时，上一次的页面数据会保留

[问题及解决](https://github.com/Meituan-Dianping/mpvue/issues/140)

10.swiper 里的元素 在 onReady 用 wx.createSelectorQuery() 查询时为 null

```js
 setTimeout(() => {
  // ...
  wx.createSelectorQuery() 
  // ...
 }, 500)
```

11.swiper 里的元素 box-shadow 被遮挡一部分

> swiper 外层设置比 内层元素稍大 1px

> 内层元素 margin: 1px 

12.子组件不能执行onShow里面的内容

> [mounted里也写一次，或者 状态放 vuex](https://github.com/Meituan-Dianping/mpvue/issues/179)


## 目录结构
```
|____build              webpack打包的环境代码
 |__git-hook            git 预提交
 webpakc.base.config.js loader 及 sprite 图的配置
|____config             webpack打包的配置文件
|____node_modules       项目运行依赖的npm包
|____src                项目代码文件夹
 |__components          自定义组件
 |__constant            常量
  |__index.js           测试/正式域名 图片云服务器URI前缀
  |__localStorage.js    全部 localStorage       
 |__packageA            分包页面组件
 |__pages               页面组件
 |__plugins             vue插件
  |__ibox
  |__amap               高德地图插件
  |__poyfill            修补 mpvue 的缺陷
   |__index.js          vue插件的注册，包含接口请求及工具utils
   |__utils.js          工具类及共用方法注册js
  |__flyio          
   |__apiUrl            接口请求地址管理
   |__config            接口请求配置管理
   |__interceptors.js   接口请求拦截器
   |__request           接口请求封装（包括loading及toast，接口的定制化配置及默认配置)
  |__modules            store的管理文件
  |__index.js           实现store对modules文件下的自动注册
 |__service             api和路由拦截
   |__api               api
   |__router            路由拦截
 |__store               vuex状态管理
 |__style               样式
   |__constant          常量
   |__shortcuts         shortcuts
   |__utils             less 函数
   common.less          公共样式 
   reset.less           重置样式
 |__app.json            小程序app.json配置
 |__App.vue             小程序的App页面【整合了小程序页面快速布局的一些样式类】
 |__main.js             类似vue的main.js，可以插件进行配置
 |__pages.js            小程序的page.json的配置 集中式页面配置
|____static             静态资源文件夹
|____.babelrc           es6语法转换配置文件
|____.editorconfig      编辑器配置
|____.eslintignore      eslint的忽略配置
|____.eslintrc.js       eslint配置
|____.gitignore         git push忽略配置
|____.postcssrc.js      postcss插件的配置文件
|____index.html         SPA的index页面
|____package.json       npm包配置文件
|____README.md          readme文档

```



