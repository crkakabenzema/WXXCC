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
.className {...}
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











