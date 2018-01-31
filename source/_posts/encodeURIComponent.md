## encodeURIComponent()函数

### 作用：
  可把字符串 作为 URI 组件 进行 编码。其返回值URIstring的副本，其中的某些字符将被十六进制的转义序列进行替换。


```js

微信授权的时候需要用

router.beforeEach((to, from, next) => {
	// 如果是微信客户端并且tid没有的情况下 跳转微信授权
	if (__isWeChat() && !_getItem('tid')) {
		// 如果没有code时
		if (!to.query.code) {
			// 去授权
			const URI = encodeURIComponent(window.location.href.split('?')[0])
			var url = 'https://open.weixin.qq.com/connect/oauth2/authorize?appid=' + _APPID_ + '&redirect_uri=' + URI + '&response_type=code&scope=snsapi_base&state=STATE#wechat_redirect'
			window.location.href = url
		} else {
			setTimeout(() => {
				const obj = {
					data: JSON.stringify({
						code: to.query.code
					})
				}
				_fetch('wx/getOpenId', obj, (res) => {
					if (res.code === 1) {
						_setItem('id', res.data.openId)
						_setItem('tid', res.data.openId)
						if (res.data.token) {
							_setItem('token', res.data.token)
						}
						next()
					} else {
						next()
					}
				})
			}, 100)
		}
	} else {
		next()
	}
})
```