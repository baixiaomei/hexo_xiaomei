---
title: 调取微信支付宝支付
---


1、 后端调取微信或支付宝提供的接口

接口返回支付链接

2、前端调取后台的接口，


后台把那个链接返回给我前端

3、设置setHeader

```js

created: {

	this.setHeader('Referer', window.location.origin)

}

utils/下的方法
调取app的方法
export const connectWebViewJavascriptBridge = callback => {
  if (window.WebViewJavascriptBridge) {
    callback(window.WebViewJavascriptBridge)
  } else {
    document.addEventListener('WebViewJavascriptBridgeReady', () => {
      callback(window.WebViewJavascriptBridge)
    }, false)
  }
}

methods: {
      setHeader (key, value) {
        var jsonStr = '{key: "' + key + '", value: "' + value + '"}'
        try {
          if (isCarelandAndroid()) {
            window.cmwebbrowsetojs.doWebMapAction(jsonStr, 301)
          } else {
            connectWebViewJavascriptBridge(bridge => {
              this.bridge = bridge
              this.bridge.callHandler('doWebMapAction', {'actionType': 301, 'key': key, 'value': value}, (res) => {})
            })
          }
        } catch (e) {

        }
      }
    }
```


```js

跳转到支付链接页面
window.location.href = JSON.parse(sessionStorage.getItem('json')).mweb_url

```

