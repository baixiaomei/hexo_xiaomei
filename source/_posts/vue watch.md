---
title: vue 监测
---

## Mescroll

[jquery i18n   教程](https://www.cnblogs.com/BlueKevin/p/5502735.html)!


[vue i18n      教程](http://blog.csdn.net/lyqhn2012/article/details/73480256)
[教程2](https://www.cnblogs.com/rogerwu/p/7744476.html)


#### 引入

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
