<template>
	<view>
		<view class="centem-body" v-if="!is_show_login">
			<!-- 顶部图片 -->
			<view class="data-clerk">
				<view class="data-clerk-text">
					<view class="data-clerk-text-h">
						相濡以沫
					</view>
					<view class="data-clerk-text-w">
						山林不向四季发誓，荣枯随缘
					</view>
				</view>
				<view class="data-img">
					<image class="fengrui-img" src="../../static/down/upload.svg" mode="scaleToFill"></image>
				</view>
			</view>

			<!-- 标题 -->
			<view class="down-view-all list-li-left-h" style="margin-top: -30upx;">
				<text class="down-view-ruan">◉</text>{{detailData.fr_down_title}}
				<view class="down-view-title-dscr">
					{{ detailData.title.rendered }}
				</view>
			</view>
			<!-- 百度网盘下载 -->
			<view class="down-view-all" v-if=" detailData.fr_down_url != ''  ">
				<!-- v1.3.6隐藏登陆 -->
				<view class="down-user">
					<view class="down-user-img">
						<image class="fengrui-img" :src="xingsu.avatarUrl" mode="aspectFit"></image>
					</view>
					<view class="down-user-name">
						{{ xingsu.nickName }}
					</view>
				</view>
				<!-- <view class="titel-h">
					资源附件
				</view> -->
				<block v-if="is_show_down == false">
					<view class="down-network-view">
						<text class="down-network-describe" selectable="true">
							{{detailData.fr_down_url}}
						</text>
						<view class="down-network-icon">
							<image class="fengrui-img"
								style="width: 70%;height: 70%;margin: 19upx auto 0upx auto;display: block;"
								src="../../static/data/renking-1.svg" mode="aspectFit"></image>
						</view>
					</view>
					<view class="down-list-btn">
						<button class="down-btn" type="default" @tap="tapDownUrl(detailData.fr_down_url)">复制链</button>
						<button class="down-btn-key" type="default" v-if="detailData.fr_down_key != ''"
							@tap="tapDownKey(detailData.fr_down_key)">提取码:{{ detailData.fr_down_key }}</button>
					</view>
					<text class="que-user-red">*</text> <text class="down-list-btn-text">若您点击按钮无法复制，可以长按链接手动复制</text>
				</block>
				<button class="down-btn-video" @tap="getVideoAd()" v-if="is_show_down == true">
					<view class="down-btn-video-icon">
						<image class="fengrui-img" src="../../static/data/ad_video.svg" mode=""></image>
					</view>
					获取附件内容
				</button>
			</view>

			<!-- 其他网盘下载 -->
			<block v-if="is_show_down == false">
				<view class="down-view-all" v-if=" detailData.fr_down_url_alter != '' ">
					<view class="titel-h">
						备用资源
					</view>
					<block v-if="is_show_down == false">
						<view class="down-network-view">
							<text class="down-network-describe" selectable="true">
								{{detailData.fr_down_url_alter}}
							</text>
							<view class="down-network-icon">
								<image class="fengrui-img"
									style="width: 70%;height: 70%;margin: 19upx auto 0upx auto;display: block;"
									src="../../static/data/renking-10.svg" mode="aspectFit"></image>
							</view>
						</view>
						<view class="down-list-btn">
							<button class="down-btn" type="default"
								@tap="tapDownUrl(detailData.fr_down_url_alter)">复制链</button>
							<button class="down-btn-key" type="default" v-if="detailData.fr_down_key_alter != ''"
								@tap="tapDownKey(detailData.fr_down_key_alter)">提取码:{{ detailData.fr_down_key_alter }}</button>
						</view>
						<text class="que-user-red">*</text> <text
							class="down-list-btn-text">若您点击按钮无法复制，可以长按链接手动复制</text>
					</block>
				</view>
			</block>

			<!-- #ifdef MP-QQ -->
			<block v-if="about_center.length>0">
				<view style="width: 100%;">
					<ad v-if="(index +1) % 5 == 0" :unit-id="about_center[0].qq_tie_video" type="card"></ad>
				</view>
			</block>
			<!-- #endif -->

			<!-- 判断微信流量主 -->
			<!-- #ifdef MP-WEIXIN -->
			<block v-if="about_center.length>0">
				<view class="" style="margin: 30upx 30upx;">
					<ad-custom style="width: 100% !important;" :unit-id="about_center[0].wx_gezi"></ad-custom>
				</view>
			</block>

			<!-- 公众号下载 -->
			<view class="down-view-all" v-if=" detailData.account_key != '' && about_center[0].public_follow != '' ">
				<view class="titel-h">
					公众号下载
				</view>
				<view class="list-li">
					<view class="list-img">
						<image class="fengrui-img" src="../../static/down/down.svg" mode="aspectFill"></image>
					</view>
					<view class="list-li-left">
						<view class="list-li-left-describe">
							关注公众号回复“<text class="que-user-red">{{detailData.account_key}}</text>”下载
						</view>
						<view class="list-btn" @tap="tapWx(about_center[0].public_follow)">
							获取
						</view>
					</view>
				</view>
			</view>

			<!-- 其他平台 -->
			<view class="down-view-all">
				<view class="titel-h">
					其他平台
				</view>
				<view class="down-li-toew">
					<button class="down-icon-list-btn" @tap="tapShop(detailData.xsgoshop)"
						v-if="detailData.xsgoshop != ''">
						<view class="down-icon-list-img">
							<image class="fengrui-img" src="../../static/data/renking-8.svg" mode="aspectFit">
							</image>
						</view>
						<view class="down-icon-list-img-title">
							商店
						</view>
					</button>
					<button class="down-icon-list-btn" @tap="tapMoney()">
						<view class="down-icon-list-img">
							<image class="fengrui-img" src="../../static/data/renking-7.svg" mode="aspectFit">
							</image>
						</view>
						<view class="down-icon-list-img-title">
							奶茶
						</view>
					</button>
					<button class="down-icon-list-btn" open-type="contact" :send-message-title="posdCenterTitle"
						:send-message-path="posUrl" show-message-card="true">
						<view class="down-icon-list-img">
							<image class="fengrui-img" src="../../static/my/customer.png" mode="aspectFit">
							</image>
						</view>
						<view class="down-icon-list-img-title">
							客服
						</view>
					</button>
				</view>
			</view>
			<!-- #endif -->

			<view class="" style="margin: 38upx;">
				<view class="titel-h">
					申明：
				</view>
				<view class="data-clerk-text-w" style="margin: 30upx 0upx;">
					尊重各网络文件的版权问题，部分软件文件均出自网络，提供下载的软件和资源均为软件或程序作者提供和网友推荐收集整理而来，仅供学习和研究使用。如有侵犯你的版权，请及时通知我们，将立即改正
				</view>
			</view>
		</view>

		<!-- 加载动画 -->
		<view v-if="is_show_login" class="data-login-flex">
			<view class="data-login">
				<image class="fengrui-img" src="../../static/data/login.svg" mode=""></image>
			</view>
			<view class="progress-box">
				<progress :percent='percent' active="true" duration="15" border-radius="100" stroke-width="6"
					activeColor="#5891f6" @activeend="loginSucc()" />
			</view>
		</view>

	</view>
</template>

<script>
	// 引入域名和自定义php文件
	import {
		API
	} from '@/components/api.js';
	import {
		Getresources
	} from '@/components/api.js';
	let rewardedVideoAd = null;
	export default {
		data() {
			return {
				xingsu: [],
				// 数据
				about_center: [],
				detailData: [],
				posUrl: '',
				posdCenterID: '',
				is_show_down: true,
				percent: 0,
				is_show_login: true
			}
		},
		onLoad(posID) {
			// 接受数据
			this.posdCenterID = posID.id;
			this.posUrl = 'pages/download/download-data?id=' + posID.id;
			this.Getres();
		},
		// 分享好友配置
		onShareAppMessage(res) {
			var that = this;
			if (res.from === 'button') { // 来自页面内分享按钮
				console.log(res.target)
			}
			return {
				title: that.detailData.title.rendered,
				imageUrl: that.detailData.thumbnailurl,
				path: '/pages/download/download-data?id=' + this.posdCenterID
			}
		},
		// 文章分享盆友圈 目前支持安卓
		onShareTimeline(res) {
			var that = this;
			return {
				title: that.detailData.title.rendered,
				imageUrl: that.detailData.thumbnailurl,
				path: '/pages/download/download-data?id=' + this.posdCenterID
			}

		},
		methods: {
			// 自定义php接口请求
			Getres: function() {
				var that = this;
				// 请求基本数据
				uni.request({
					url: API + '/wp-json/wp/v2/fengrui',
					success: (res) => {
						// 数据
						that.about_center = res.data;
						that.posdData();
					}
				});
			},

			// 检测是否有登入  赋值给xsUserInfo判断是否为空
			xingSuUser: function() {
				var that = this;
				var xsUserInfo = uni.getStorageSync('xsUserInfo_key');
				console.log(xsUserInfo)
				if (xsUserInfo == '') {
					uni.showModal({
						title: '提示',
						content: '下载文件时请先登录',
						success: function(res) {
							if (res.confirm) {
								console.log('用户点击确定');
								that.getUserInfo();
							} else if (res.cancel) {
								uni.navigateBack({
									delta: 1
								});
							}
						}
					});
				} else {
					that.xingsu = xsUserInfo;
				}
			},
			
			// 用户登录
			getUserInfo: function() {
				var that = this;
				//#ifdef MP-QQ
				uni.getUserInfo({
					success: function(infoRes) {
						console.log(infoRes)
						that.xingsu = infoRes.userInfo;
						that.isLogin = true;
						uni.setStorageSync("xsUserInfo_key", that.xingsu);
					}
				});
				//#endif
			
				//#ifdef MP-WEIXIN
				uni.getUserProfile({
					desc: '用于完善会员资料', // 声明获取用户个人信息后的用途，后续会展示在弹窗中，请谨慎填写
					success: (res) => {
						console.log(res.userInfo)
						that.xingsu = res.userInfo;
						that.isLogin = true;
						uni.setStorageSync("xsUserInfo_key", that.xingsu);
					}
				})
				//#endif
			},
			
			// 文章数据
			posdData: function() {
				var that = this;
				uni.request({
					url: API + '/wp-json/wp/v2/posts/' + that.posdCenterID,
					success: (res) => {
						that.detailData = res.data;
						that.CreateAd();
						// 独立文章公告弹窗
						if (that.detailData.pos_notice != '') {
							uni.showModal({
								title: '公告',
								content: that.detailData.pos_notice,
								success: function(res) {
									if (res.confirm) {
										console.log('用户点击确定');
									} else if (res.cancel) {
										console.log('用户点击取消');
									}
								}
							});
						} else {
							console.log('该文章没有设置独立公告')
						};

						// 检查是否需要广告
						if (that.about_center[0].wx_jili_video == '' || that.detailData
							.if_rec_down_video == "0") {
							that.is_show_down = false;
						} else {
							that.is_show_down = true;
						}

						// 赋值动态框
						that.percent = 100;
					}
				});
			},

			// 加载过度
			loginSucc: function() {
				var that = this;
				that.is_show_login = false;
				that.xingSuUser();
			},
			//初始化激励视频广告组件
			CreateAd: function() {
				var that = this;
				//#ifdef MP-WEIXIN
				if (that.about_center[0].wx_jili_video != '') {
					if (wx.createRewardedVideoAd) {
						that.videoAd = wx.createRewardedVideoAd({
							adUnitId: that.about_center[0].wx_jili_video
						})
						that.videoAd.onLoad(() => {})
						that.videoAd.onError((err) => {
							uni.showToast({
								icon: 'none',
								title: "观看失败,请稍后重试"
							})
						})
						that.videoAd.onClose((res) => {
							if (res && res.isEnded) {
								uni.showToast({
									icon: 'none',
									title: "感谢您的支持"
								})
								that.is_show_down = false;
							} else {
								uni.showToast({
									icon: 'none',
									title: "中途关闭广告"
								})
								that.tarBlack();
							}
						})
					}

				} else {
					console.log('微信没有激励视频广告id')
				}
				//#endif

				//#ifdef MP-QQ
				if (that.about_center[0].qq_jili_video != '') {
					if (qq.createRewardedVideoAd) {
						that.videoAd = qq.createRewardedVideoAd({
							adUnitId: that.about_center[0].qq_jili_video
						})
						that.videoAd.onLoad(() => {})
						that.videoAd.onError((err) => {
							uni.showToast({
								icon: 'none',
								title: "观看失败,请稍后重试"
							})
						})
						that.videoAd.onClose((res) => {
							if (res && res.isEnded) {
								uni.showToast({
									icon: 'none',
									title: "感谢您的支持"
								})
								that.is_show_down = false;
							} else {
								uni.showToast({
									icon: 'none',
									title: "中途关闭广告"
								})
								that.tarBlack();
							}
						})
					}

				} else {
					console.log('QQ没有激励视频广告id')
				}
				//#endif
			},

			// 激励视频
			getVideoAd: function() {
				let that = this;
				// 用户触发广告后，显示激励视频广告
				if (that.videoAd) {
					that.videoAd.show().catch(() => {
						// 失败重试
						that.videoAd.load()
							.then(() => that.videoAd.show())
							.catch(err => {
								console.log('激励视频 广告显示失败')
								uni.showToast({
									icon: 'none',
									title: "观看失败,请稍后重试"
								})
							})
					})
				}
			},

			// 打开小商店详情
			tapShop: function() {
				var that = this;
				uni.navigateToMiniProgram({
					appId: that.about_center[0].xs_shop_appid,
					path: 'plugin-private://wx34345ae5855f892d/pages/productDetail/productDetail?productId=' +
						that.detailData.xsgoshop,
					success(res) {
						// 打开成功
					}
				})
			},

			// 赞赏图片弹出
			tapMoney: function() {
				var that = this;
				console.log(that.about_center[0].payman_img)
				if (that.about_center[0].payman_img != '') {
					uni.previewImage({
						current: 1,
						urls: [that.about_center[0].payman_img],
						longPressActions: {
							itemList: ['发送给朋友', '保存图片', '收藏'],
							success: function(data) {
								console.log('选中了第' + (data.tapIndex + 1) + '个按钮,第' + (data.index + 1) +
									'张图片');
							},
							fail: function(err) {
								console.log(err.errMsg);
							}
						}
					});

				} else {
					console.log('您没有有配置赞赏图')
				}
			},

			// 访问微信文章
			tapWx: function(wx) {
				var that = this;
				var sve = wx;
				uni.navigateTo({
					url: '../weblist/weblist?aurl=' + sve
				})
				uni.setClipboardData({
					data: that.detailData.account_key,
					success: function() {
						console.log('已自动复制关键字');
					}
				});

			},

			// 复制提取码
			tapDownKey: function(k) {
				var that = this;
				var key = k;
				console.log(k)
				uni.setClipboardData({
					data: key,
					success: function() {
						console.log('success');
					}
				});
			},

			// 没有激励视频直接下载
			tapDownUrl: function(dn) {
				var that = this;
				that.dnurl = dn;
				uni.setClipboardData({
					data: that.dnurl,
					success: function() {
						console.log('success');
					}
				});
			},
		}
	}
</script>

<style>
	/* 加载动画 */
	.data-login-flex {
		position: fixed;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -75%);
	}

	.progress-box {
		width: 80%;
		margin: auto;
	}

	.data-login {
		height: 600upx;
		width: 600upx;
		margin: auto;
	}

	.down-view-ruan {
		color: #23c1aa;
		margin-right: 14upx;
	}

	.down-view-title-dscr {
		padding: 14upx 36upx 30upx 46upx;
		color: #B2B2B2;
		font-size: 24upx;
	}

	.down-icon-list-btn {
		text-align: center;
		margin: 0;
		background-color: unset;
		padding: 0;
	}

	.down-li-toew {
		display: flex;
		margin-top: 60upx;
		align-items: center;
		border-radius: 20upx;
		justify-content: space-around;
	}

	.down-icon-list {
		text-align: center;
	}

	.down-icon-list-img {
		height: 60upx;
		width: 60upx;
		overflow: hidden;

	}

	.down-icon-list-img-title {
		font-size: 22upx;
		color: #b1b1b1;
		margin-top: 12upx;
	}

	.down-li-toew-ma {
		margin: 40upx 0upx 20upx 0upx;
	}

	/* 说明 */
	.que-user-red {
		color: red;
	}

	.list-btn {
		height: 50upx;
		width: 160upx;
		background-color: #007AFF;
		border-radius: 100upx;
		text-align: center;
		color: #FFFFFF;
		line-height: 50upx;
		bottom: 0upx;
		position: absolute;
		right: 0;
	}

	/* 列表 */
	.list-li-left-describe {
		color: #D5D5D5;
		font-size: 26upx;
		position: absolute;
		top: 0upx;
		left: 0upx;
	}

	.list-li-tag {
		color: #0BBDA6;
		font-size: 20upx;
	}

	.list-li-left-h {
		font-size: 36upx;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-line-clamp: 2;
		-webkit-box-orient: vertical;
	}

	.list-li-left {
		margin-left: 32upx;
		flex-grow: 1;
		height: 120upx;
		position: relative;
	}

	.list-img {
		height: 120upx;
		width: 140upx;
		border-radius: 14upx;
		overflow: hidden;
		flex-shrink: 0;
	}

	.list-li {
		display: flex;
		margin-top: 60upx;
		align-items: center;
		background-color: #fff;
		/* padding: 24upx; */
		border-radius: 20upx;
	}

	/* 百度网盘下载 */
	.down-btn {
		background-color: #007AFF !important;
		color: #FFFFFF !important;
		border-radius: 100upx !important;
		width: 100%;
		margin: 0upx 10upx;
		font-size: 28upx;
		line-height: 80upx;
		height: 80upx;
	}

	.down-btn-video {
		background-color: #007AFF !important;
		color: #FFFFFF !important;
		border-radius: 100upx !important;
		width: 80%;
		margin: auto;
		font-size: 28upx;
		line-height: 80upx;
		height: 80upx;
		display: flex;
		justify-content: center;
		align-items: center;
		margin-top: 60upx;
	}

	.down-btn-video-icon {
		height: 46upx;
		width: 56upx;
		margin-top: -6upx;
		margin-right: 8upx;
	}

	.down-btn-key {
		background-color: #23c1aa !important;
		color: #FFFFFF !important;
		border-radius: 100upx !important;
		width: 100%;
		margin: 0upx 10upx;
		font-size: 28upx;
		line-height: 80upx;
		height: 80upx;
	}

	.down-list-btn-text {
		font-size: 22upx;
		color: #B2B2B2;
	}

	.down-list-btn {
		display: flex;
		align-items: center;
		margin-bottom: 10upx;
	}

	.down-network-icon {
		width: 150upx;
		background-color: #EFEFEF;
		border-radius: 10upx;
		height: 130upx;
		margin-left: 20upx;
		overflow: hidden;
		flex-shrink: 0;
		line-height: 130upx;
		margin: auto;
	}

	.down-network-describe {
		background-color: #EFEFEF;
		border-radius: 10upx;
		padding: 30upx;
		color: #B2B2B2;
		font-size: 24upx;
		width: 380upx;
		flex-shrink: 0;
		overflow: hidden;
	}

	.down-network-view {
		display: flex;
		height: 130upx;
		margin: 30rpx 0upx;
		overflow: hidden;
	}

	.down-user-name {
		margin-left: 20upx;
		font-size: 32upx;
	}

	.down-user-img {
		height: 60upx;
		width: 60upx;
		overflow: hidden;
		border-radius: 200upx;
	}

	.down-user {
		display: flex;
		align-items: center;
		margin: 0rpx;
		justify-content: flex-start;
	}

	/* 顶部图片 */
	.data-clerk-text-w {
		font-size: 24upx;
		color: #b1b1b1;
	}

	.data-clerk-text-h {
		font-size: 40upx;
		margin-bottom: 20upx;
	}

	.data-clerk-text {
		width: 360upx;
		position: absolute;
		top: 80upx;
		left: 40upx;
	}

	.data-clerk {
		width: 100%;
		height: 300upx;
		position: relative;
	}

	.data-img {
		height: 300upx;
		width: 360upx;
		overflow: hidden;
		position: absolute;
		bottom: 0upx;
		right: 0upx;
	}

	/* 全局 */
	.titel-h {
		font-size: 34upx;
		position: relative;
	}

	.titel-h:after {
		content: '';
		position: absolute;
		bottom: -14upx;
		left: 0px;
		width: 96upx;
		height: 8upx;
		border-radius: 200upx;
		background: linear-gradient(90deg, rgba(55, 110, 234, 1) 0%, rgba(255, 255, 255, 1) 100%);
	}

	.down-view-all {
		background-color: #FFFFFF;
		border-radius: 20upx;
		padding: 38upx;
		margin: 38upx;
	}

	.fengrui-img {
		height: 100%;
		width: 100%;
	}

	page {
		background-color: #F7F7F7;
	}

	.centem-body {
		/* background-image: linear-gradient(#3482e2, #F7F7F7); */
	}

	button:after {
		border: 0px solid rgba(0, 0, 0, .2);

	}

	/* 暗黑模式下应用的样式 */
	@media (prefers-color-scheme: dark) {
		page {
			background: #161616;
		}

		.down-view-all,
		.list-li {
			background-color: #202020;
		}

		.down-network-describe,
		.down-network-icon {
			background-color: #797979;
		}
	}
</style>
