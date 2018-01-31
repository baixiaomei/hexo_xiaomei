<template>
	<div id="colllectreg-page">
		<div class="pic2">
			<img src="/static/activity/collect/collectreg/bg1.png" alt="">
			<img src="/static/activity/collect/collectreg/bg2.png" alt="">
		</div>
		<div class="progress">
			<div class="bar">
				<div class="num">集赞数量</div>
				<div class="progress-bar">
					<div class="active-bar" :style="{width:gatherNum/48 * 100 + '%'}" ></div>
					<div class="p-top" :style="{left:gatherNum/48 * 100 + '%'}">
						<p class="geshu">{{gatherNum}}</p>
						<p class="indicator"></p>
					</div>
				</div>
				<div class="percent">
					<span>6</span>
					<span>12</span>
					<span>21</span>
					<span>48</span>
				</div>
			</div>
			<div class="dot">
				<div class="btn" @click="giveup">
					<img src="/static/activity/collect/collectreg/dot.png" alt="">
					<span>点赞</span>
				</div>
			</div>
		</div>
		<div class="lang-swiper">
			<div class="swiper-container" ref="swiper">
				<div class="swiper-wrapper">
					<div class="swiper-slide">
						<img src="/static/activity/collect/collectdot/6.png" alt="">
					</div>
					<div class="swiper-slide">
						<img src="/static/activity/collect/collectdot/12.png" alt="">
					</div>
					<div class="swiper-slide">
						<img src="/static/activity/collect/collectdot/21.png" alt="">
					</div>
					<div class="swiper-slide">
						<img src="/static/activity/collect/collectdot/48.png" alt="">
					</div>
				</div>
				<div class="swiper-button-prev"></div>
				<div class="swiper-button-next"></div>
			</div>
		</div>
		<div class="register">
			<div>
				<input type="tel" maxlength="11" v-model="userInfo.mobile" placeholder="请输入您的手机号码">
			</div>
			<div class="vscode">
				<input type="tel" placeholder="输入验证码" v-model="userInfo.validCode" maxlength="6">
				<input type="button" id="code" value="获取验证码" v-model="validCode" :class="{ 'is-disabled': is_disabled }" :disabled="is_disabled" @click="getSmsCode">
			</div>
			<div>
				<input type="password" placeholder="设置密码" maxlength="18" v-model="userInfo.password">
			</div>
			<div @click="register">
				<div class="registerbtn">注册</div>
			</div>
		</div>
		<div class="rules">
			<img src="/static/activity/collect/collectdot/bgtitle.png" alt="">
			<div class="info">
				1、活动时间：2018年1月12日-1月16日
				<br/>
				2、用户集赞到对应数量时自动解锁奖励。
				<br/>
				3、助力集赞的用户在集赞页面参与注册，可获得额外奖励。发起人解锁奖励时发放。
				<br/>
				4、解锁48个赞对应的奖励时，返回60天权益开始计算，被邀请人享受自己首笔购买金额的1%返现，邀请人享受被邀请人首笔购买金额的0.6%返现。
				<br/>
				5、如有疑问可咨询客服电话400-8671-777。
				<br/>
				6、本活动最终解释权归团油宝所有。
			</div>
			<img src="/static/activity/collect/collectdot/bg2.png" alt="">
		</div>

		<div id="shade" v-if="isShade"></div>
		<div id="Invitationtoregister-succeed" v-if="isShade">
			<div class="Invitationtoregister-defeatedmark">
				<img src="../../../static/img/correct.png">
			</div>
			<h1>{{ _shadeInfo }}</h1>
			<p>请关注微信公众号登录团油宝</p>
			<p>复制 <span>yfqguanwei</span> 添加微信公众号</p>
			<img class="qr" src="../../../static/img/yfqqr.jpg">
			<a class="jump" href="http://a.app.qq.com/o/simple.jsp?pkgname=com.czb.youfenqi">
				下载团油宝APP ( {{ validCodes }} )
			</a>
		</div>
	</div>
</template>

<script>
	import {
		_isMobile,
		_isPass,
		_isCode,
		_fetch,
		_getItem
//		_setItem
	} from '__lib__/utils'
	import Swiper from 'swiper'
	import 'swiper/dist/css/swiper.css'
	export default {
		name: 'collectdot',
		data () {
			return {
				inviteCode: this.$route.query.inviteCode ? this.$route.query.inviteCode : '', // 邀请码
				userInfo: {
					mobile: null,
					password: null,
					validCode: null,
					channelId: this.$route.query.id
				},
				validCode: '获取验证码',
				validTime: 60,
				is_disabled: false,
				validCodes: '4 s',
				validTimes: 4,
				isShade: false, // app下载引导页
				gatherNum: 0 // 集赞数目
			}
		},
		methods: {
			countdowns () {
				this.validTimes = 4
				this.interval = setInterval(() => {
					if (this.validTimes === 0) {
						window.location.href = 'http://a.app.qq.com/o/simple.jsp?pkgname=com.czb.youfenqi'
					} else {
						this.validTimes--
						this.validCodes = this.validTimes + ' s'
					}
				}, 1000)
			},
			register () {
				if (!this.userInfo.mobile) return this.$parent.prompt('请填写手机号')
				if (_isMobile(this.userInfo.mobile)) return this.$parent.prompt('手机号不正确')

				if (!this.userInfo.password) return this.$parent.prompt('请填写密码')
				if (_isPass(this.userInfo.password)) return this.$parent.prompt('请填写6-18位字母数字组合')

				if (!this.userInfo.validCode) return this.$parent.prompt('请填写验证码')
				if (_isCode(this.userInfo.validCode)) return this.$parent.prompt('验证码格式不正确')

				this.$parent.toastShow()

				const obj = {
					data: JSON.stringify(this.userInfo)
				}

				// 请求数据
				_fetch('user/regist', obj, (res) => {
					this.$parent.toastHide()
					if (res.code === 1) {
						this._shadeInfo = '注册成功'
						this.countdowns()
						this.isShade = true
					} else if (res.code === 1010) {
						this._shadeInfo = '您已注册过'
						this.countdowns()
						this.isShade = true
					} else {
						this.$parent.prompt(res.msg)
					}
				})
			},
			// 获取验证码
			getSmsCode () {
				if (!this.userInfo.mobile) return this.$parent.prompt('请填写手机号')
				if (_isMobile(this.userInfo.mobile)) return this.$parent.prompt('手机号不正确')
				this.$parent.toastShow()

				const obj = {
					data: JSON.stringify({
						mobile: this.userInfo.mobile,
						scene: 1
					})
				}
				_fetch('common/sendSmsCode', obj, (res) => {
					this.$parent.toastHide()
					if (res.code === 1) {
						this.$parent.prompt('验证码发送成功!')
						this.countdown()
					} else {
						this.$parent.prompt(res.msg)
					}
				})
			},
			countdown () {
				this.is_disabled = true
				this.validTime = 60
				this.interval = setInterval(() => {
					if (this.validTime === 0) {
						clearInterval(this.interval)
						this.is_disabled = false
						this.validCode = '重新获取验证码'
						this.validTime = 60
					} else {
						this.validTime--
						this.validCode = this.validTime + 's'
					}
				}, 1000)
			},
			getdotnum () {
				// 查询集赞数目
				const coldata = {
					data: JSON.stringify({
						gatherNum: 1
					})
				}
				_fetch('activity/getGatherNum', coldata, (res) => {
					console.log(res)
					this.$parent.toastHide()
					if (res.code === 1) {
						this.gatherNum = res.data.gatherNum
					} else {
						this.$parent.prompt(res.msg)
					}
				})
			},
			giveup () {
				// 点赞
				const givedata = {
					data: JSON.stringify({
						inviteCode: this.$route.query.inviteCode,
						openId: _getItem('id')
					})
				}
				_fetch('activity/gather', givedata, (res) => {
					if (res.code === 1) {
						this.$nextTick(function () {
							// 获取集赞的数目
							this.getdotnum()
						})
					} else {
						this.$parent.prompt(res.msg)
					}
				})
			},
			swiperInit () {
				// 获取银行卡
				let _that = this
				_that.swiper = new Swiper(this.$refs.swiper, {
					effect: 'coverflow',
					grabCursor: true,
					centeredSlides: true,
					slidesPerView: 'auto',
					coverflowEffect: {
						rotate: 50,
						stretch: 10,
						depth: 100,
						modifier: 1,
						slideShadows: true
					},
					// 如果需要前进后退按钮
					nextButton: '.swiper-button-next',
					prevButton: '.swiper-button-prev',
					onProgress: function (swiper) {
						var slide,
							progress,
							es,
							i
						for (i = 0; i < swiper.slides.length; i++) {
							slide = swiper.slides[i]
							progress = slide.progress
							es = slide.style
							es.opacity = 1 - Math.min(Math.abs(progress / 2), 1)
							es.webkitTransform = es.MsTransform = es.msTransform = es.MozTransform = es.OTransform = es.transform = 'translate3d(0px, 0,' + (-Math.abs(progress * 150)) + 'px)'
						}
					},
					onSetTransition: function (swiper, speed) {
						var es,
							i
						for (i = 0; i < swiper.slides.length; i++) {
							es = swiper.slides[i].style
							es.webkitTransitionDuration = es.MsTransitionDuration = es.msTransitionDuration = es.MozTransitionDuration = es.OTransitionDuration = es.transitionDuration = speed + 'ms'
						}
					},
					onInit: function (swiper) {
					},
					onSlideChangeStart: function (swiper) {
					}
				})
			}
		},
		components: {

		},
		computed: {

		},
		beforeCreate () {
			this.$parent.toggle._isBarBom = false
			window.document.title = '助力集赞'
			this.$parent.toastShow()
			// 查询集赞数目
			const coldata = {
				data: JSON.stringify({
					gatherNum: 1
				})
			}
			_fetch('activity/getGatherNum', coldata, (res) => {
				console.log(res)
				this.$parent.toastHide()
				if (res.code === 1) {
					this.gatherNum = res.data.gatherNum
				} else {
					this.$parent.prompt(res.msg)
				}
			})
		},
		created () {
//			this.swiperInit()
		},
		// 监控抽奖次数
		watch: {
		},
		mounted () {
			this.swiperInit()
		},
		beforeDestroy () {
		}
	}

</script>

<style lang="less">
	@colorA:#0f141d;
	@colorB:#090C17;
	@color2:#C19550;
	@fontRed: #ff4040;
	#colllectreg-page {
		width: 100%;
		.pic2 {
			width: 100%;
			img {
				width: 100%;
				display: block;
			}
		}
		.lang-swiper {
			width: 100%;
			background:linear-gradient(to bottom, @colorA 0%,@colorB 100%);
			.swiper-container {
				width: 100%;
			}
			.swiper-slide {
				background-position: center;
				background-size: cover;
				width:250px;
				img{
					width:100%;
				}
			}
			.swiper-button-next, .swiper-container-rtl{
				background-image: url("../../../static/activity/collect/collectdot/right.png");
				right: 10px;
				left: auto;
			}
			.swiper-button-prev{
				background-image: url("../../../static/activity/collect/collectdot/left.png");
				left: 10px;
				right: auto;
			}
		}
		// 集赞进度条
		.progress{
			padding-top:20px;
			width:90%;
			height:150px;
			padding-left:5%;
			padding-right:5%;
			background:linear-gradient(to bottom, @colorB 0%,@colorA 100%);
			.bar{
				position:relative;
				width:100%;
				height:45px;
				display:flex;
				// 弹性盒方法水平垂直居中
				align-items: center;
				justify-content: center;
				.num{
					width:25%;
					text-align: left;
					line-height: 40px;
					color:#fff;
					font-size:14px;
				}
				.progress-bar{
					width:90%;
					height:18px;
					position:relative;
					border-radius: 12px;
					background:#fff;
					.active-bar{
						height:16px;
						border-radius: 12px;
						width:100%;
						position:absolute;
						top:50%; /*垂直居中*/
						left:0;
						background:@color2;
						margin-top: -8px; /*height的一半*/
					}
					.p-top{ // 上面的三角形
						position:absolute;
						top:-20px;
						left:80%;
						.geshu{
							color:#fff;
						}
						.indicator{
							width:0;
							height:0;
							border-width:6px;
							border-style:solid dashed dashed dashed;
							border-color:@color2 transparent transparent transparent;
						}
					}
				}
				.percent{// 进度数
					position:absolute;
					display:flex;
					bottom:0px;
					right:0px;
					width:80%;
					text-align: right;
					span{
						color:#fff;
					}
					span:nth-child(1){
						width:15%;
					}
					span:nth-child(2){
						width:20%;
					}
					span:nth-child(3){
						width:25%;
					}
					span:nth-child(4){
						width:35%;
					}
				}
			}
			.dot{ // 解锁按钮
				width:80%;
				margin:25px auto;
				margin-bottom:0px;
				padding-bottom:10px;
				.btn{
					span{
						height:44px;
						line-height: 44px;
					}
					margin:0 auto;
					width:100%;
					height:44px;
					line-height: 44px;
					text-align: center;
					background:@color2;
					color:#fff;
					font-size:16px;
					border-radius:10px;
					img{
						width:23.5px;
						vertical-align:middle;
					}
				}
			}
		}

		.register{ // 注册
			width:90%;
			background:@colorB;
			padding:15px 5%;
			div{
				width:90%;
				height:44px;
				border-radius:10px;
				position:relative;
				margin:0 auto;
				margin-top:10px;
				display:block;
				input{
					width: 95%;
					line-height: 44px;
					font-size: 13px;
					padding-left:5%;
					display: inline-block;
					border-radius: 10px;
					color: #FFF;
					outline: none;
					background: @colorA;
					border:1px solid @color2;
				}
				.registerbtn{
					width: 95%;
					line-height: 44px;
					font-size: 16px;
					text-align: center;
					padding-left:5%;
					border-radius: 10px;
					color: #FFF;
					background: @color2;
				}
			}
			div:last-child{
				margin-top:30px;
			}
			.vscode{
				width:90%;
				height:44px;
				border-radius:10px;
				position:relative;
				margin-top:10px;
				display:flex;
				input:nth-child(1){
					width:55%;
					border:1px solid @color2;
				}
				input:nth-child(2){
					width:35%;
					margin-left:8%;
					border:1px solid @color2;
				}
				#code{
					padding-left:0;
					text-align: center;
					background:@color2;
					color:#fff;
				}
			}
		}

		.rules{ // 活动规则
			width:100%;
			img{
				display:block;
				width:100%;
			}
			.info{
				width:80%;
				padding-top:10px;
				padding-left:10%;
				padding-right:10%;
				background:@colorB;
				color:#fff;
				font-size:12px;
				line-height: 22px;
			}
		}
	}

	// 下载APP引导页
	#shade{
		background: rgba(0,0,0,.4);
		width: 100%;
		height: 100%;
		position: fixed;
		top: 0px;
		left: 0px;
	}
	#Invitationtoregister-succeed{
		position: fixed;
		width: 80%;
		height:auto;
		background: #ffffff;
		border-radius: 8px;
		margin-left: 10%;
		top: 100px;
		.Invitationtoregister-defeatedmark{
			width: 60px;
			height: 60px;
			background: #ee544a;
			border-radius: 100%;
			overflow: hidden;
			position: absolute;
			top: -25px;
			left: 50%;
			margin-left: -30px;
			img{
				display: block;
				width: 30px;
				margin: 20px auto;
			}
		}
		.qr {
			width: 80%;
			margin: 0 auto;
			display: block;
		}
		.jump {
			height: 46px;
			color: #FFF;
			display: block;
			text-align: center;
			line-height: 46px;
			font-size: 16px;
			background: #cd758d;
			background-image: -webkit-linear-gradient(0deg, #cd758d, #cc765b);
			background-image: linear-gradient(45deg, #cd758d, #cc765b);
		}
		h1{
			font-size: 18px;
			color: @fontRed;
			text-align: center;
			padding-top: 45px;
		}
		p{
			text-align: center;
			font-size: 14px;
			padding: 0 20px;
			color: #666;
			line-height: 20px;
			span {
				font-weight: bold;
				color: #333;
			}
		}
		button{
			display: block;
			width: 70%;
			height: 35px;
			text-align: center;
			margin: 20px auto;
			margin-bottom: 40px;
			font-size: 17px;
			color: #ffffff;
			line-height: 35px;
			border-radius: 100px;
			border: none;
			outline: none;
			background: #ee544a;
		}

	}
</style>
