## 消息滚动展示

#### 注意在html 不要用v-if   这样会获取不到dom

#### 如果想控制用v-show


```js
/**
 * 引入jquery
 */
// npm install jquery
import $ from 'jquery'

const obj = {
			data: JSON.stringify({
				act: 'act20015',
				method: 'getData'
			})
		}
		_fetch('activity/call', obj, (res) => {
			this.$parent.toastHide()
			if (res.code === 1) {
				if (res.data.prizeList) {
					this.$nextTick(() => {
						this.prizeList = res.data.prizeList
						this.scroll()
					})
				}
			}
		})


methods: {
  scroll () {
			clearTimeout(this.scrollTimes)
			var obj = this.$('#scroll')
			var scrollA = this.$('#scroll_1')
			var scrollB = this.$('#scroll_2')
			this.scrollTimes = setInterval(() => {
				if (scrollB[0].offsetTop - obj[0].scrollTop <= 0) {
					obj[0].scrollTop -= scrollA[0].offsetHeight
				} else {
					obj[0].scrollTop++
				}
			}, 50)
		}
},
	beforeDestroy () {
		clearTimeout(this.scrollTimes)
	}
```

```html
<div class="award_situation">
					<div class="scroll"></div>
					<div id="scroll" class="scroll-wrap">
						<ul id="scroll_1" class="scroll-content">
							<li v-for="(item, index) in prizeList" :key="index">第{{item.term}}期  中奖码：{{item.prizeNo}}  奖品：京东电子购物卡</li >
						</ul>
						<ul id="scroll_2"  class="scroll-content">
							<li v-for="(item, index) in prizeList" :key="index">第{{item.term}}期  中奖码：{{item.prizeNo}}  奖品：京东电子购物卡</li >
						</ul>
					</div>
				</div>

```


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