---
title: jquery
---

## Mescroll

[jquery i18n   教程](https://www.cnblogs.com/BlueKevin/p/5502735.html)!


[vue i18n      教程](http://blog.csdn.net/lyqhn2012/article/details/73480256)
[教程2](https://www.cnblogs.com/rogerwu/p/7744476.html)


一、<script>放在head内和body内有什么区别

  加载的顺序不一样，你可以把ＨＴＭＬ看成从上往下加载的。
  例如在网速慢的情况下把ｊｓ代码放在ｂｏｄｙ底部用户会先看到网页结构，等ｊｓ加载完成后才出现特效
  区别简述：
  在HTML body部分中的JavaScripts会在页面加载的时候被执行。
  在HTML head部分中的JavaScripts会在被调用的时候才执行，但是在主页和其余部分代码之前预先装载。
  
1.JavaScript应放在哪里

head 部分中的脚本: 需调用才执行的脚本或事件触发执行的脚本放在HTML的head部分中。当你把脚本放在head部分中时，可以保证脚本在任何调用之前被加载，

从而可使代码的功能更强大； 比如对*.js文件的提前调用。 也就是说把代码放在<head>区在页面载入的时候，就同时载入了代码，你在<body>区调用时就不需要再载入代码了，速度就提高了，这种区别在小程序上是看不出的，当运行很大很复杂的程序时，就可以看出了。

2.如果把javascript放在head里的话，则先被解析,但这时候body还没有解析。
（常规html结构都是head在前，body在后）如果head的js代码是需要传入一个参数（在body中调用该方法时，才会传入参数），并需调用该参数进行一系列的操作，那么这时候肯定就会报错，因为函数该参数未定义（undefined）。
$(document).ready(function(){  
//执行代码  
})  
3.从JavaScript对页面下载性能方向考虑：由于脚本会阻塞其他资源的下载（如图片等）和页面渲染，直到脚本全部下载并执行完成后，页面的渲染才会继续，因此推荐将所有的<script>标签尽可能放到<body>标签的底部，以尽量减少对整个页面下载的影响。

``` js

"dependencies": {
    "vue-i18n": "^7.4.0"
}


```

# main.js

```js

Vue.use(VueI18n) // 引入


// 在src中新建一个lang的文件夹，里面写一个cn.js  一个en.js
在en.js里

export default {
	branner: {
    title: 'POWER LINKS',
    subTitle: 'Microgrid based on blockchain',
    button: 'WHERE YOU GO ?'
  },
}
// 在main.js引入
import EN from '@/lang/en'
import CN from '@/lang/cn'


// locale 默认为EN 形式
const i18n = new VueI18n({
  locale: 'EN',
  messages: {
    EN,
    CN
  }
})

```

#### html文案

``` html

<template>
	<h2> {{ $t('branner.title') }} </h2>
  <p class="subTitle"> {{ $t('branner.subTitle') }} </p>
	[ <a href="javascript:;" :class="{ 'is-active': $i18n.locale === 'CN' }" @click="toggleLocale('CN')">中</a> / <a href="javascript:;" :class="{ 'is-active': $i18n.locale === 'EN' }" @click="toggleLocale('EN')">EN</a> ]
</template>

<script>
	toggleLocale (lang) {
      this.$nextTick(() => {
        this.$i18n.locale = lang
      })
    },
</script>

```


#### js 获取数据

``` js

// 加载更多数据
		infiniteHandler ($state) {
			setTimeout(() => {
				const obj = {
					data: JSON.stringify({
						pageNum: this.totalPages,
						pageSize: 10,
						status: this.status
					})
				}
				_fetch('order/getCouponList', obj, (res) => {
					if (res.code === 1) {
						// 没有数据 不加载
						if (res.data.coupons.length < 1) {
							$state.complete()
						} else {
							// 现有数据是否有值
							if (this.coupons.length > 0) {
								this.coupons = this.coupons.concat(res.data.coupons)
								$state.loaded()
							} else {
								this.coupons = res.data.coupons
								$state.loaded()
							}
							// 下一页
							this.totalPages++
						}
					}
				})
			}, 200)
		},

```
