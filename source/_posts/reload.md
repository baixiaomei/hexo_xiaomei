---
title: 点击按钮，页面重新加载
---

#### 方法一
    window.location.reload()
    
    
#### 方法二 
    this.$router.go(0)
    
    
#### 方法三
    点击的时候跳转到一个空的页面，在空的页面里写回调回到原来页面。
```js
	export default {
		data () {
			this.$router.replace('/user/index')
			return {
			}
		}
	}

```
#### 引入

