---
title: cordova
---

## Mescroll
link >

cordova.apache.org

cordova的前身是phonegap  开源的一个软件，做手机端的混合开发的东西,被adobe()收购了，把核心的东西给了apache这个开源组织。

javascript 的标准时ECMA script（欧洲标准计算机委员会）
基于webview的hybrid

cordova是一个做混合开发的一个平台  hybrid   基于webview(浏览器)的hybrid。

andriod 基于 Java 开发的

第一步：
搭建Andriod  和 Java 的环境
[mescroll](http://www.mescroll.com/api.html)! mescroll 是一个js的下拉刷新和上拉加载的框架。原生js,不依赖jquery,zepto 支持vue。 一套代码多端运行。完美运行于android,ios,手机各浏览器，兼容pc端主流浏览器

## vue-infinite-loading

[vue-infinite-loading](https://peachscript.github.io/vue-infinite-loading/#!/)


#### 引入

``` js

"dependencies": {
    "vue-infinite-loading": "^2.2.2"
}

$ import InfiniteLoading from 'vue-infinite-loading'

```


#### html文案

``` html

<infinite-loading @infinite="infiniteHandler" ref="infiniteLoading">
    <span slot="no-more">
        没有更多数据了
    </span>
    <span class="infinite-status-results" slot="no-results">
        <img src="~assets/img/icon_kongbai.png">
        <p class="font" v-if="status === 1">没有未使用优惠券</p>
    </span>
</infinite-loading>

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
