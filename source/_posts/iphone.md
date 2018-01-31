## 监测是手机还是pc端

#### m站进行处理

运用userAgent.js监测系统设备

"useragent.js": "^0.5.6",

import useragent from useragent.js
```js


/**
 * 检测是否是手机端
 */
// 在PC端做判断
(function () {
  var ua = navigator.userAgent.toLowerCase()
  var contains = function (a, b) {
    if (a.indexOf(b) !== -1) return true
  }

  if ((contains(ua, 'android') && contains(ua, 'mobile')) || (contains(ua, 'android') && contains(ua, 'mozilla')) || (contains(ua, 'android') && contains(ua, 'opera')) || contains(ua, 'ucweb7') || contains(ua, 'iphone')) {

  } else window.location.href = 'http://test.www.powerlinks.net'
})()


```


```js


/**
 * 检测是否是PC端
 */
// 在m站做判断
(function () {
  var ua = navigator.userAgent.toLowerCase()
  var contains = function (a, b) {
    if (a.indexOf(b) !== -1) return true
  }

  if ((contains(ua, 'android') && contains(ua, 'mobile')) || (contains(ua, 'android') && contains(ua, 'mozilla')) || (contains(ua, 'android') && contains(ua, 'opera')) || contains(ua, 'ucweb7') || contains(ua, 'iphone')) window.location.href = 'http://test.m.powerlinks.net'
})()

```