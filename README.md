# WXJS:

## 1.App()注册小程序：

App()模板：

```javascript
#####
   define global variable area
#####
App({
    onLaunch(options){
        //Do something initial when launch
    },
    onShow(options){
        //Do something when show
    },
    onHide(options){
        //Do something when hide.
    }
    onError(msg){
    console.log(msg)
    },
    globalData: 'I am global data'
})
```

所有页面共享唯一一个App实例。通过getApp方法获取全局唯一的App实例。

```javascript
const appInstance = getApp()
console.log(appInstance.globalData)
```

## 2.Page()构造器注册页面:  

注：不要在App.onLaunch的时候调用 getCurrentPages()，此时 page还没有生成

Data定义和初始化page页面内的变量, 使用this.setData({ ... })改变或添加data内的变量；

页面内组件绑定事件处理函数，使用<element bindTap="eventfunc"/>或

<element bindchange="eventfunc"/> 

当element为可tap，用bindTap; 当element为可change, 用bindChange

 Page内调用使用：

```javascript
Page({
  viewTap: function() {
    console.log('view tap')
  }
})
```

Page()模板：

```javascript
#### 
   define local variable area
####

Page({
    data: {
      text: "This is page data."   //页面第一次渲染使用的初始数据{{text}}
      num: 0,
      array: [{msg:'1'},{msg:'2'}]  // {{array[0].msg}}调用
      object: {
        text: 'init data'
      }
    },
    onLoad: function(options){
        //页面创建时执行
        console.log(this.route)
        //函数内this.route返回页面路径
    }，
    onShow: function(){
    //页面出现在前台时执行
    },
    onReady: function(){
      //页面首次渲染完毕时执行  
    },
    onHide: function(){
      //页面从前台变为后台时执行  
    },
    onUnload: function(){
        //页面销毁时执行
    },
    onPullDownRefresh: function(){
        //触发下拉刷新时执行
        //需要在app.json的window选项开启enablePullDownRefresh
    },
    onReachBottom: function(){
        //页面触底时执行
        //可以在app.json的window选项设置onReachBottomDistance
    },
    onShareAppMessage: function(res){
        //页面被用户分享时执行
        //需要return一个Object,用于自定义转发内容
        if(res.from === 'button'){
            console.log(res.target)
        }
        return{
            title: '自定义转发标题',
            path:'/page/user?id=123'
        }
    }，
    onPageScroll: function(){
        //页面滚动时执行
    }，
    onResize: function(){
        //页面尺寸变化是执行
    }，
    onTabItemTap(item){
        //tab点击时执行
        console.log(item.index)
        console.log(item.pagePath)
        console.log(item.text)
    },
        
    // 自定义事件处理函数
    eventfunc: function(){
        ...
    }
        
    // 事件响应函数,
    viewTap: function(){
        this.setData({
            text: 'Set some data for updating view.'
        }, function(){
            // this is setData callback
        })
    },
    // 自由数据
    customData: {
        hi: 'MINA'
    }
})
```

## 3.自定义组件:

Component(Object object):

```js
Component({

  behaviors: [],

  // 属性定义（详情参见下文）
  properties: {
    myProperty: { // 属性名
      type: String,
      value: ''
    },
    myProperty2: String // 简化的定义方式
  },

  data: {}, // 私有数据，可用于模板渲染

  lifetimes: {
    // 生命周期函数，可以为函数，或一个在methods段中定义的方法名
    attached: function () { },
    moved: function () { },
    detached: function () { },
  },

  // 生命周期函数，可以为函数，或一个在methods段中定义的方法名
  attached: function () { }, // 此处attached的声明会被lifetimes字段中的声明覆盖
  ready: function() { },

  pageLifetimes: {
    // 组件所在页面的生命周期函数
    show: function () { },
    hide: function () { },
    resize: function () { },
  },

  methods: {
    onMyButtonTap: function(){
      this.setData({
        // 更新属性和数据的方法与更新页面数据的方法类似
      })
    },
    // 内部方法建议以下划线开头
    _myPrivateMethod: function(){
      // 这里将 data.A[0].B 设为 'myPrivateData'
      this.setData({
        'A[0].B': 'myPrivateData'
      })
    },
    _propertyChange: function(newVal, oldVal) {

    }
  }

})
```

Behavior(Object object):

```javascript
// my-behavior.js
module.exports = Behavior({
  behaviors: [],
  properties: {
    myBehaviorProperty: {
      type: String
    }
  },
  data: {
    myBehaviorData: {}
  },
  attached: function(){},
  methods: {
    myBehaviorMethod: function(){}
  }
})
```

## 4.模块化（require和export):

示例：

```javascript
// common.js
function sayHello(name){
    console.log('Hello ${name} !')
}
function sayGoodbye(name){
    console.log('Goodbye ${name} !')
}

module.exports.sayHello = sayHello
//或者
exports.sayGoodbye = sayGoodbye
```

```javascript
var common = require('commmon.js')
Page({
    helloMINA：function(){
        common.sayHello('M')
    },
    goodbyeMINA: function(){
        common.sayGoodbye('M')
    }
})
```

## 5.定时器：

### 1. number setTimeout(function callback, number delay, any rest)

在定时到期以后执行注册的回调函数

参数:

function callback 回调函数

number delay 延迟的时间，函数的调用会在该延迟之后发生，单位 ms。

any rest param1, param2, ..., paramN 等附加参数，它们会作为参数传递给回调函数。

函数返回值：

number 定时器的编号。这个值可以传递给 [clearTimeout](https://developers.weixin.qq.com/miniprogram/dev/api/base/timer/clearTimeout.html) 来取消该定时。

### 2. clearTimeout(number timeoutID)

取消由 setTimeout 设置的定时器。

参数：

number timeoutID 要取消的定时器的 ID

### 3. number setInterval(function callback, number delay, any rest)

设定一个定时器。按照指定的周期（以毫秒计）来执行注册的回调函数

参数：

function callback 回调函数

number delay 执行回调函数之间的时间间隔，单位 ms。

any rest param1, param2, ..., paramN 等附加参数，它们会作为参数传递给回调函数。

函数返回值：

number 定时器的编号。这个值可以传递给 [clearInterval](https://developers.weixin.qq.com/miniprogram/dev/api/base/timer/clearInterval.html) 来取消该定时。

### 4. clearInterval(number intervalID)

取消由 setInterval 设置的定时器。

参数: number intervalID 要取消的定时器的 ID



# WXML:

wxss选定组件: 

```css
.className {...}   #class selector
```

```css
#the-id   #ID selector
```

```css
.the-parent > .the-child   #子元素选择器
```

```
.the-ancestor .the-descendant   #后代选择器
```

```css
#a-node, .some-other-nodes   多选择器的并集
```

## 1.视图容器：

1.1 <view>:

子<view>是横向还是纵向布局: style: flex-direction:column;||flex-direction:row;

<view style="...">



1.2 <scroll-view>:

该元素将无法使用onPullDownRefresh

scroll-view属性：scroll-x; scroll-y; upper-threshold; lower-threshold; scroll-top;

scrol-left; scroll-into-view; scroll-with-animation; enable-back-to-top; enable-flex; scroll-anchoring; bindscrolltoupper; bindscrolltolower; bindscroll; 





1.3 <swiper>: # need to improve

内含<swiper-item>

swiper属性：

indicator-dots; autoplay; circular; vertical; interval; duration; previous-margin; next-margin

<swiper>示例, <block>内的内容详见**动态列表渲染**：

```html
<swiper>
    <block wx:for="{{background}}" wx:key="*this">
        <swiper-item>
            <view class="swiper-item {{item}}"></view>
        </swiper-item>
    </block>
</swiper>
```

```html
Page({
  data: {
    background: ['demo-text-1', 'demo-text-2', 'demo-text-3'],
    indicatorDots: true,
    vertical: false,
    autoplay: false,
    circular: false,
    interval: 2000,
    duration: 500,
    previousMargin: 0,
    nextMargin: 0
  },
})
```



### 2.基础内容：

2.1 <text>:



2.2 <icon>:

icon 属性：type (category: success, success_no_circle, info, warn, waiting, cancel, download, search, clear); size; color



2.3 <progress>:

progress 属性：percent, show-info, font-size, activeColor, backgroundColor, active, active-mode, bindactiveend(type: eventhandle)

### 3.表单组件：

3.1<button>







3.2 <form>

将组件内的用户输入的[switch](https://developers.weixin.qq.com/miniprogram/dev/component/switch.html) [input](https://developers.weixin.qq.com/miniprogram/dev/component/input.html) [checkbox](https://developers.weixin.qq.com/miniprogram/dev/component/checkbox.html) [slider](https://developers.weixin.qq.com/miniprogram/dev/component/slider.html) [radio](https://developers.weixin.qq.com/miniprogram/dev/component/radio.html) [picker](https://developers.weixin.qq.com/miniprogram/dev/component/picker.html) 提交。







3.3 <input>



3.4 <label>



3.5 <slider>



3.6 <switch>



3.7 <radio>



3.8 <textarea>





### 4.媒体组件：

4.1 <audio>



4.2 <camera>



4.3 <image>



4.4 <video>



4.5 <live-player>



4.6 <live-pusher>



### 5.画布：

5.1 <canvas>



### 6.地图：

6.1 <map>



### 7.导航：

7.1 <navigator>



### 8.导航栏：

8.1 <navigation-bar>







### 动态变量渲染：

格式：

```
# fileName.wxml
<element>{{variableName}}</element>
```

```javascript
# fileName.js
Page({
    data:{
       variableName: variable 
    },
    ...
})
```

### 动态列表渲染：

格式：

```html
<element wx:for="{{array}}">
    {{index}}:{{item.message}}
</element>  
```

默认数组的当前项的下标变量名默认为 index，数组当前项的变量名默认为 item



使用 wx:for-item 指定数组当前元素的变量名

使用 wx:for-index 指定数组当前下标的变量名：

```html
<view wx:for="{{array}}" wx:for-index="idx" wx:for-item="itemName">
  {{idx}}: {{itemName.message}}
</view>
```



绑定数组：使用wx:for控制属性，wx:for="{{arrayName}}"

数组下标变量：index

数组当前项的变量名：item

指定数组当前元素的变量名：wx:for-item

指定数组当前下标的变量名:  wx:for-index



列表项目为保持自己的特征和状态，确保重新排序，提高渲染效率：使用wx:key 指定列表中项目的唯一标识符

wx:key的值：

1. 当array的item为dict时：

   字符串, 代表循环的array中item的某key, 该key需要是唯一的字符串或数字，且不能动态改变；

2. 当array的item为list时：

   *this, 代表在for循环中的item本身, 需要item是唯一的字符串或数字，即item不能是dict。



模板：定义后在不同的地方调用，使用name属性，作为模板的名字。

模板定义:

```html
<!--
item: {
  index: 0,
  msg: 'this is a template',
  time: '2016-06-18'
}
-->

<template name="msgItem">
    <view>
        <text> {{index}}: {{msg}} </text>
        <text> Time: {{time}} </text>
    </view>
</template>
```

模板声明：

```html
<template is="msgItem" data="{{...item}}"/>
```

is可以动态决定具体需要渲染哪个模板:

```html
<template name="odd">
  <view> odd </view>
</template>

<template name="even">
  <view> even </view>
</template>

<block wx:for="{{[1, 2, 3, 4, 5]}}">
  <template is="{{item % 2 == 0 ? 'even' : 'odd'}}"/>
</block>
```



### 路由：

下划线为必填属性：

#### 1. wx.switchTab(Object object)

跳转到tabBar页面，关闭其他所有非tabBar页面

Object属性：<u>url</u> ; success(type: function); fail(type: function); complete(type: function)

#### 2. wx.redirectTo(Object object)

关闭当前页面，跳转到应用内的某个页面。但是不允许跳转到 tabbar 页面。

Object属性：<u>url(must)</u>, success, fail, complete

#### 3. wx.reLaunch(Object object)

关闭所有页面，打开到应用内的某个页面

Object属性：<u>url(must)</u>, success, fail,  complete

#### 4. wx.navigateTo(Object object) 

保留当前页面，跳转到应用内的某个页面。但是不能跳到 tabbar 页面

Object属性：<u>url(must)</u>, events(type: Object) 页面间通信接口，监听被打开页面发送到当前页面的数据, success(type: function), fail(type: function), complete(type: function)

Object.success回调函数属性：eventChannel 和被打开页面进行通信

#### 5. wx. navigateBack(Object object)

关闭当前页面，返回上一页面或多级页面。可通过 [getCurrentPages](https://developers.weixin.qq.com/miniprogram/dev/reference/api/getCurrentPages.html) 获取当前的页面栈，决定需要返回几层

Object属性：delta 返回的页面数, success, fail, complete

#### 6. EventChannel 页面间事件通信通道

EventChannel.emit(string eventName, any args) 触发一个事件

EventChannel.off(string eventName, function fn) 取消监听一个事件

EventChannel.on(string eventName, function fn) 持续监听一个事件

EventChannel.once(string eventName, function fn) 监听一个事件一次，触发后失效



### 界面：

#### 1. wx.showModal(Object object)

显示模态对话框

Object属性：title, content, showCancel, cancelText, cancelColor, confirmText, confirmColor, success, fail, complete

Object.success回调函数属性：confirm, cancel



### 背景：

#### 1. wx.setBackgroundTextStyle(Object object)

动态设置下拉背景字体、loading 图的样式

Object属性：textStyle(must), success, fail, complete

Object.textStyle值: dark, light



### 下拉刷新：

#### 1. wx.stopPullDownRefresh(Object object)

停止当前页面下拉刷新

#### 2. wx.startPullDownRefresh(Object object)

开始当前页面下拉刷新



### 滚动：

#### 1. wx.pageScrollTo(Object object)

将页面滚动到目标位置，支持选择器和滚动距离两种方式定位

Object属性：scrollTop (滚动到页面的目标位置，单位 px), duration, selector, success, fail, complete



### 动画：

### 1. wx.createAnimation(Object object)

创建一个动画实例animation。调用实例的方法来描述动画。

Object属性：duration, timingFunction, delay, transformOrigin

timingFunction的合法值：'linear' ,'ease','ease-in','ease-in-out','ease-out','step-start','step-end'

返回值：Animation动画对象



### WXML:

#### 1. wx.createSelectorQuery()

返回值：SelectorQuery，查询节点信息的对象

返回一个SelectorQuery对象实例

```html
const query = wx.createSelectorQuery()
query.select('#the-id').boundingClientRect()
query.selectViewport().scrollOffset()
query.exec(function(res){
  res[0].top       // #the-id节点的上边界坐标
  res[1].scrollTop // 显示区域的竖直滚动位置
})
```

##### 1.1 SelectorQuery.exec(function callback):

执行所有的请求。请求结果按请求次序构成数组，在callback的第一个参数中返回

返回值：NodesRef

##### 1.2 SelectorQuery.in(Component component):

将选择器的选取范围更改为自定义组件 `component` 内

返回值：SelectorQuery

```html
Component({
  queryMultipleNodes (){
    const query = wx.createSelectorQuery().in(this)
    query.select('#the-id').boundingClientRect(function(res){
      res.top // 这个组件内 #the-id 节点的上边界坐标
    }).exec()
  }
})
```

##### 1.3 SelectorQuery.select(string selector):

返回值：NodesRef

##### 1.4 SelectorQuery.selectAll(string selector):

在当前页面下选择匹配选择器 selector 的所有节点

返回值：NodesRef

##### 1.5 SelectorQuery.selectViewport():

选择显示区域。可用于获取显示区域的尺寸、滚动位置等信息

返回值：NodesRef



#### 2. IntersectionObserver:

用于推断某些节点是否可以被用户看见、有多大比例可以被用户看见。

IntersectionObserver.disconnect()：停止监听

IntersectionObserver.observe(string targetSelector, function callback)：指定目标节点并开始监听相交状态变化情况

IntersectionObserver.relativeTo(string selector, Object margins): 使用选择器指定一个节点

IntersectionObserver.relativeToViewport(Object margins): 指定页面显示区域作为参照区域之一























### 网络：

#### 1. 发起请求 wx.request(Object object)：

发起HTTPS网络请求

Object属性：<u>url</u>, data, header(content-type: application/json (default)), method, dataType, responseType, success, fail, complete

object.method值：OPTIONS, GET, HEAD, POST, PUT, DELETE, TRACE, CONNECT

object.dataType值：json

object.responseType值：text, arraybuffer

object.success回调函数：data(开发者服务器返回的数据), statusCode(开发者服务器返回的HTTP状态码）, header(开发者服务器返回的HTTP Response Header)

#### 2. 下载 wx.downloadFile(Object object)：

下载文件资源到本地。客户端直接发起一个HTTPS GET请求，返回文件的本地临时路径。

Object属性: <u>url</u>, header, filePath, success, fail, complete

Object.success属性：tempFilePath, filePath, statusCode

#### 3. 上传 wx.uploadFile(Object object):

将本地资源上传到服务器。客户端发起一个 HTTPS POST 请求，其中 `content-type` 为 `multipart/form-data`

Object属性：<u>url, filePath, name</u>, header, formData, success(type: function), fail(type: function), complete(type: function)



### 地图：

#### 1. 创建map上下文MapContext对象：

wx.createMapContext(string mapId, Object this)

mapId为map组件的id, Object this为当前组件实例的this, 以操作组件内map组件

返回 MapContext

#### 2. MapContext:

MapContext.getCenterLocation(): 获取当前地图中心的经纬度

MapContext.moveToLocation(Object object): 将地图中心移置当前定位点

MapContext.includePoints(Object object): 缩放视野展示所有经纬度

MapContext.getRegion(): 获取当前地图的视野范围

应用实例：

```javascript
// map.js
Page({
  onReady: function (e) {
    // 使用 wx.createMapContext 获取 map 上下文
    this.mapCtx = wx.createMapContext('myMap')
  },
  getCenterLocation: function () {
    this.mapCtx.getCenterLocation({
      success: function(res){
        console.log(res.longitude)
        console.log(res.latitude)
      }
    })
  },
  moveToLocation: function () {
    this.mapCtx.moveToLocation()
  },
  translateMarker: function() {
    this.mapCtx.translateMarker({
      markerId: 0,
      autoRotate: true,
      duration: 1000,
      destination: {
        latitude:23.10229,
        longitude:113.3345211,
      },
      animationEnd() {
        console.log('animation end')
      }
    })
  },
  includePoints: function() {
    this.mapCtx.includePoints({
      padding: [10],
      points: [{
        latitude:23.10229,
        longitude:113.3345211,
      }, {
        latitude:23.00229,
        longitude:113.3345211,
      }]
    })
  }
})
```



### 图片：

#### 1. wx.chooseImage(Object object)

从本地相册选择图片或使用相机拍照。

Object参数：count, sizeType, sourceType, success, fail, complete

Object.sizeType合法值：original, compressed

Object.sourceType合法值：album, camera

Object.success回调函数：tempFilePaths, tempFiles

res.tempFiles结构：path, size

#### 2. wx.getImageInfo(Object object)

获取图片信息

Object参数：<u>src</u>, success, fail, complete

Object.success回调函数：res属性（width, height, path, orientation, type）

res.orientation合法值（up,up-mirrored, down, down-mirrored, left-mirrored, right, right-mirrored, left）

#### 3. wx.saveImageToPhotosAlbum(Object object)

保存图片到系统相册

Object属性：<u>filePath</u>, success, fail, complete



### 转发：

#### 1. wx.updateShareMenu

Object属性：withShareTicket, isUpdatableMessage, activityId, templateInfo, success, fail, complete

Object.templateInfo的结构：parameterList

parameterList的结构： name, value

#### 2. wx.showShareMenu

Object属性：withShareTicket, success, fail, complete

#### 3. wx.hideShareMenu

Object属性：succes, fail, complete

#### 4. wx.getShareInfo

Object属性：shareTicket, timeout, success, fail, complete,

Object.success回调函数：errMsg, encryptedData, iv, cloudID







### 登录：





### 小程序跳转：



### 用户信息：



### 支付：

wx.requestPayment(Object object)



### 授权：

wx.authorize(Object object)

提前向用户发起授权请求。调用后会立刻弹窗询问用户是否同意授权小程序使用某项功能或获取用户的某些数据，但不会实际调用对应接口



