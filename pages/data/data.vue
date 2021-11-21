<template>
	<view>
		<view class="index-ov" v-if="!is_show_login">
			<!-- <view class="back-img"></view> -->

			<!-- 标题 --><!-- 标题 -->
			<view class="titel-h-w">
				<view class="titel-h">
					{{ posdCenterTitle }}
				</view>
			</view>

			<!-- 时间和装饰品 --><!-- 时间和装饰品 -->
			<view class="time-view">
				<view class="time-img">
					<image class="fengrui-img" src="../../static/data/time.png" mode="aspectFit"></image>
				</view>
				<view class="time-font">
					{{ detailData.date }}
				</view>
			</view>

			<!-- 关注微信公众号 --><!-- 关注微信公众号 -->
			<!-- #ifdef MP-WEIXIN -->
			<view class="account-view" v-if="detailData.account_url != ''">
				<view class="account-flex">
					<view class="account-img">
						<image class="fengrui-img" :src="about_center[0].public_logo" mode=""></image>
					</view>
					<view class="account-aout">
						<view class="account-name">
							{{ about_center[0].public_name }}
						</view>
						<view class="account-describe">
							{{ about_center[0].public_describe }}
						</view>
					</view>
				</view>
				<view class="">
					<button class="account-btn" @tap="accountTap(detailData.account_url)">公众号阅读</button>
				</view>
			</view>
			<!-- #endif -->

			<!-- 正文内容 -->
			<view class="data-view">
				<mp-html :tag-style="tag_style" :content="contentDate" :container-style="container_style"
					selectable="true"> 内容疯狂加载ing...</mp-html>
			</view>

			<!-- #ifdef MP-WEIXIN -->
			<view v-if="about_center.length>0" style="width: 94%;padding-left: 3%;margin-top: 50upx;">
				<ad :unit-id="about_center[0].wx_video" ad-type="video" ad-theme="white"></ad>
			</view>
			<!-- #endif -->

			<!-- #ifdef MP-QQ -->
			<view v-if="about_center.length>0" style="width: 94%;padding-left: 3%;margin-top: 50upx;">
				<ad :unit-id="about_center[0].qq_video" ad-type="video" ad-theme="white"></ad>
			</view>
			<!-- #endif -->

			<!-- 猜你想搜 -->
			<view class="titel-felx">
				<view class="time-img">
					<image class="fengrui-img" src="../../static/my/star.png" mode=""></image>
				</view>
				<view class="titel-h-go">
					相关文章
				</view>
			</view>
			<!-- 列表 -->
			<block>
				<view class="list-li" v-for="(item ,index) in posTagsList" :key="index" @click="posTap(item.id)">
					<view class="list-img">
						<image class="fengrui-img" :src="item.thumbnailurl" mode="aspectFill"></image>
					</view>
					<view class="list-li-left">
						<view class="list-li-left-h">
							{{ item.title.rendered }}
						</view>
						<view class="list-li-left-describe">
							<view class="">推选阅读</view>
							<view>{{ item.date }}</view>
						</view>
					</view>
				</view>
			</block>

			<!-- 其他问题 -->
			<view class="titel-felx">
				<view class="time-img">
					<image class="fengrui-img" src="../../static/data/comment.svg" mode=""></image>
				</view>
				<view class="titel-h-go">
					其他问题
				</view>
			</view>
			<block>
				<view class="que-lisr-v" v-for="(item ,index) in XuMessageList" :key="index">
					<view class="que-user" v-if="item.parent == '0'">
						问
					</view>
					<view class="que-user que-user-d" v-else>
						答
					</view>
					<view class="que-description">
						<view class="que-name">
							{{ item.author_name }}
							<text class="que-text" v-for="(userItem,i) in XuMessageList" :key="i"
								v-if="userItem.id == item.parent">
								回答： {{userItem.author_name}}
							</text>
						</view>
						<view class="que-wen">
							<rich-text :nodes="item.content.rendered"></rich-text>
						</view>
						<view class="que-time">
							{{ item.date }}
						</view>
					</view>
				</view>
			</block>

			<!-- 支撑顶部高度 -->
			<view style="height: 140upx;"></view>
			<!-- 文章列表没有数据 -->
			<view class="no-list-data" v-if="no_list_data">
				--我的底线就到这里了--
			</view>

			<!-- 提问转发 -->
			<!-- #ifdef MP-WEIXIN -->
			<view class="cove-bom">
				<!-- #endif -->
				<!-- #ifdef MP-QQ -->
				<view class="cove-bom" v-if="about_center[0].xs_qq_off_down == '0'">
					<!-- #endif -->
					<view class="cover-view-h" @tap="openCoverTap()">
						<view class="cover-h-icon">
							<image class="fengrui-img" src="../../static/data/Appendixes.png" mode="aspectFit"></image>
						</view>
						<view>文章附件</view>
						<view class="cover-open-right">
							<image :class="openCover == true ? 'cover-open-right-bot' :'cover-open-right-up'"
								src="../../static/data/add.png" mode="aspectFit"></image>
						</view>
					</view>
					<view class="cover-list-view" :animation="animationData">
						<view
							v-if="detailData.fr_down_url != '' || detailData.account_key != '' || detailData.xsgoshop != ''"
							@tap="questionsTap()">
							<view class="cover-list-icon">
								<image class="fengrui-img" src="../../static/data/down.png" mode="aspectFit"></image>
							</view>
							<view class="cover-list-font">资源下载</view>
						</view>
						<view @tap="enclosure()">
							<view class="cover-list-icon">
								<image class="fengrui-img" src="../../static/data/comment.png" mode="aspectFit">
								</image>
							</view>
							<view class="cover-list-font">提交问题</view>
						</view>
						<!-- #ifdef MP-WEIXIN -->
						<button class="cover-list-btn" open-type="contact" :send-message-title="posdCenterTitle"
							:send-message-path="posUrl" show-message-card="true">
							<view class="cover-list-icon">
								<image class="fengrui-img" src="../../static/my/customer.png" mode="aspectFit"></image>
							</view>
							<view class="cover-list-font">咨询客服</view>
						</button>
						<view @tap="tapMoney()">
							<view class="cover-list-icon">
								<image class="fengrui-img" src="../../static/data/redbeans.png" mode="aspectFit">
								</image>
							</view>
							<view class="cover-list-font">赞赏作者</view>
						</view>
						<!-- #endif -->
					</view>
				</view>
			</view>
		
		<!-- 加载动画 -->
		<view v-if="is_show_login" class="data-login-flex">
			<view class="data-login" >
				<image class="fengrui-img" src="../../static/data/login.svg" mode=""></image>
			</view>
			<view class="progress-box">
				<progress :percent='percent' active="true" duration="15" border-radius="100" stroke-width="6" activeColor="#5891f6"  @activeend="loginSucc()"/>
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
	import mpHtml from '@/components/mp-html/mp-html';
	let rewardedVideoAd = null;
	export default {
		components: {
			mpHtml
		},
		data() {
			return {
				posdCenterTitle: '数据加载中....',
				detailData: [],
				posID: '',
				posTagsList: [],
				posTag: '',
				// 数据
				about_center: [],
				qq_down_shop: '0',
				// 文章没有数据
				no_list_data: false,
				// 检测文章是否需要开启激励视频
				preview: '',
				frDate: '',
				// 评论
				XuMessageList: [],
				page: 1,
				boolShow: false,
				posUrl: '',
				// #ifdef MP-WEIXIN
				account: [],
				// #endif
				openCover: true,
				animationData: {},
				postID: '',
				contentDate: [],
				percent:0,
				is_show_login:true,
				// 文本框解析
				tag_style: {
					h1: 'line-height: 50px;font-size:16px;',
					h2: 'line-height: 50px;font-size:16px',
					h3: 'line-height: 50px;font-size:16px',
					h4: 'line-height: 50px;font-size:16px',
				},
				container_style: 'font-size:15px;',
				
			}
		},
		// 建立动画
		onShow: function() {
			var animation = uni.createAnimation({
				duration: 400,
				timingFunction: 'linear',
			})
			this.animation = animation
		},
		onLoad(posID) {
			// 接受数据
			this.posdCenterID = posID.id;
			this.posUrl = 'pages/data/data?id=' + posID.id;

			// 自定义php接口请求
			this.Getres();


			// 微信分享盆友圈
			//#ifdef MP-WEIXIN
			wx.showShareMenu({
				withShareTicket: true,
				menus: ['shareAppMessage', 'shareTimeline']
			})
			//#endif

			//条件编译 QQ小程序分享
			//#ifdef MP-QQ
			qq.showShareMenu({
				showShareItems: ['qq', 'qzone', 'wechatFriends', 'wechatMoment']
			})
			//#endif

		},

		// 监听滑动
		onPageScroll: function(e) {
			// 判断型号 显示不同标题栏高度
			var that = this;

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
				path: '/pages/data/data?id=' + this.posdCenterID
			}
		},
		// 文章分享盆友圈 目前支持安卓
		onShareTimeline(res) {
			var that = this;
			return {
				title: that.detailData.title.rendered,
				imageUrl: that.detailData.thumbnailurl,
				path: '/pages/data/data?id=' + this.posdCenterID
			}
		},
		// 监听触底----上啦刷新
		onReachBottom() {
			this.page = this.page + 1;
			this.XuMessage();
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
						//#ifdef MP-QQ
						that.qq_down_shop = res.data[0].xs_qq_off_down;
						//#endif

						// 获取文章内容
						that.posdData();
					}
				});
			},



			// 文章数据
			posdData: function() {
				var that = this;
				uni.request({
							url: API + '/wp-json/wp/v2/posts/' + that.posdCenterID,
							success: (res) => {
								that.detailData = res.data;
								that.posdCenterTitle = res.data.title.rendered;
								that.posTag = res.data.tags[0];
								that.postID = res.data.id;
								
								// 赋值动态框
								that.percent = 100;
								
								// 正则添加class
								that.contentDate = res.data.content.rendered
									.replace(/<h1([\s\w"=\/\.:;]+)((?:(style="[^"]+")))/ig, '<h1')
									.replace(/<h1([\s\w"=\/\.:;]+)((?:(class="[^"]+")))/ig, '<h1')
									.replace(/<h1>/ig, '<h1 class="h1">')

									.replace(/<h2([\s\w"=\/\.:;]+)((?:(style="[^"]+")))/ig, '<h2')
									.replace(/<h2([\s\w"=\/\.:;]+)((?:(class="[^"]+")))/ig, '<h2')
									.replace(/<h2>/ig, '<h2 class="h2">')

									.replace(/<h3([\s\w"=\/\.:;]+)((?:(style="[^"]+")))/ig, '<h3')
									.replace(/<h3([\s\w"=\/\.:;]+)((?:(class="[^"]+")))/ig, '<h3')
									.replace(/<h3>/ig, '<h3 class="h3">')

									.replace(/<h4([\s\w"=\/\.:;]+)((?:(style="[^"]+")))/ig, '<h4')
									.replace(/<h4([\s\w"=\/\.:;]+)((?:(class="[^"]+")))/ig, '<h4')
									.replace(/<h4>/ig, '<h4 class="h4">')

									.replace(/<h5([\s\w"=\/\.:;]+)((?:(style="[^"]+")))/ig, '<h5')
									.replace(/<h5([\s\w"=\/\.:;]+)((?:(class="[^"]+")))/ig, '<h5')
									.replace(/<h5>/ig, '<h5 class="h4">')

									.replace(/<h6([\s\w"=\/\.:;]+)((?:(style="[^"]+")))/ig, '<h6')
									.replace(/<h6([\s\w"=\/\.:;]+)((?:(class="[^"]+")))/ig, '<h6')
									.replace(/<h6>/ig, '<h6 class="h6">')

									.replace(/<code([\s\w"=\/\.:;]+)((?:(style="[^"]+")))/ig, '<code')
									.replace(/<code([\s\w"=\/\.:;]+)((?:(class="[^"]+")))/ig, '<code')
									.replace(/<code>/ig, '<code class="language-css">')

								// 检测是否有标签和热门文章
								if (res.data.tags == '' || res.data.tags[0] == undefined) {
									console.log('该文章没有设置标签，请注意wordpress后天文章标签设置')
									that.no_list_data = true;
								} else {
									// 热门文章
									uni.request({
										url: API + '/wp-json/wp/v2/posts/?tags=' + that.posTag,
										success: (res) => {
											that.no_list_data = false;
											that.posTagsList = res.data.slice(0, 3);
										}
									});
								};

								// 先检测是有有流量主广告,如果有id执行预加载广告，如果没有阅读全文
								// 随后frDate获取当前时间，存为缓存storage_key + 文章id
								// 判断是否有缓存，如果没有则弹窗激励视频阅读，如果有缓存对比storage_key的值，大于需要阅读广告，其他不要
								//各个小程序端为其自带的storage api，数据存储生命周期跟小程序本身一致，即除用户主动删除或超过一定时间被自动清理，否则数据都一直可用
								//#ifdef MP-WEIXIN
								if (that.about_center[0].wx_jili_video != '' && that.detailData.fr_videp_if !=
									'0') {
									//#endif

									//#ifdef MP-QQ
									if (that.about_center[0].qq_jili_video != '' && that.detailData.fr_videp_if !=
										'0') {
										//#endif	

										if (that.about_center[0].fr_jili_video_record == '1') {
											var frDate = new Date();
											this.frDate = frDate.getUTCFullYear() + (frDate.getMonth() + 1) +
												frDate
												.getDate();
											var posIdInfo = uni.getStorageSync('storage_key' + this.postID);
											if (posIdInfo == '') {
												that.videoAD();
												console.log('缓存为空')
											} else {
												if (posIdInfo < this.frDate) {
													that.videoAD();
													var posIdInfo = uni.getStorageSync('storage_key' + this
														.postID);
													console.log(this.frDate)
												} else {
													console.log('直接阅读全文', this.frDate, posIdInfo)
												}
											}
										} else {
											that.videoAD();
										}

									} else {
										console.log('阅读该文章不需要激励视频广告')
									};

									// 广告加载
									that.CreateAd();
									// 加载留言
									that.XuMessage();

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
								}
							});

					},

					XuMessage: function() {
						var that = this;
						// 获取问题评论留言列表
						uni.request({
							url: API + '/wp-json/wp/v2/comments?post=' + that.posdCenterID + '&page=' + that
								.page,
							success: (res) => {
								that.XuMessageList = that.XuMessageList.concat(res.data);
							}
						});
					},
					
					// 加载过度
					loginSucc:function(){
						var that = this;
						that.is_show_login = false;
					},
					
					// 左上角返回按钮
					tarBlack: function() {
						// 建立页面栈 如果是1则是分享打开需要返回主页，大于1则返回上一级
						var selPage = getCurrentPages();
						// console.log(selPage.length)
						if (selPage.length == 1) {
							uni.switchTab({
								url: "../index/index"
							})
						} else {
							uni.navigateBack({
								delta: 1
							});
						}
					},

					// 赞赏图片弹出
					tapMoney: function() {
						var that = this;
						if (that.about_center[0].payman_img != '') {
							uni.previewImage({
								current: 1,
								urls: [that.about_center[0].payman_img],
								longPressActions: {
									itemList: ['发送给朋友', '保存图片', '收藏'],
									success: function(data) {
										console.log('选中了第' + (data.tapIndex + 1) + '个按钮,第' + (data.index +
												1) +
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

					// 点击跳转
					posTap: function(posd) {
						var posId = posd;
						console.log(posId)
						uni.navigateTo({
							url: '../data/data?id=' + posId,
						})
					},

					// 文章标签
					potgsTap: function(e) {
						var that = this;
						var jstag = e;
						uni.navigateTo({
							url: '../category/categ_lisst?jstag=' + jstag
						})
					},

					// 点击提交问题
					questionsTap: function() {
						var that = this;
						var posId = that.posdCenterID;
						uni.navigateTo({
							url: '../download/download-data?id=' + posId
						})
					},
					// 流量主视频弹窗
					videoAD: function() {
						var that = this;
						uni.showModal({
							title: '阅读说明',
							content: '感谢您阅读，但该文章需要您先阅读激励视频广告！！！',
							success: function(res) {
								if (res.confirm) {
									console.log('用户点击确定');
									that.getVideoAd();
								} else if (res.cancel) {
									console.log('用户点击取消');
									that.tarBlack();
								}
							}
						});
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
										that.adCache();
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
										that.adCache();
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

					// 点击回到主页那妞
					getIndex: function() {
						uni.switchTab({
							url: "../index/index"
						})
					},

					// 关注微信公众号
					accountTap: function(e) {
						var that = this;
						console.log(e)
						var sve = e;
						uni.navigateTo({
							url: '../weblist/weblist?aurl=' + sve
						})
					},

					// 附件点击
					enclosure: function() {
						var that = this;
						var posId = that.posdCenterID;
						uni.navigateTo({
							url: '../questions/questions?id=' + posId
						})
					},
					// 文章附件点击
					openCoverTap: function() {
						var that = this;
						if (that.openCover == true) {
							that.openCover = false
							that.animation.height(100).step(),
								that.animationData = that.animation.export()
						} else {
							that.openCover = true
							that.animation.height(0).step(),
								that.animationData = that.animation.export()
						}
					},

					// 点击获取链接地址
					navigate(href, e) {
						uni.setClipboardData({
							data: href,
							success: function() {
								console.log('设置系统剪贴板成功');
								uni.showToast({
									title: '复制成功啦',
									duration: 1500
								});
							},
							fail: function() {
								console.log('设置系统剪贴板失败');
								uni.showToast({
									title: '复制失败咯',
									duration: 1500
								});
							}
						});
					},

					// 数据缓存
					adCache() {
						uni.setStorage({
							key: 'storage_key' + this.postID,
							data: this.frDate,
							success: () => {
								console.log('成功缓存：storage_key' + this.postID);
							}
						});
					}
			}
		}
</script>

<style>
	/* 加载动画 */
	.data-login-flex{
		position: fixed;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -75%);
	}
	.progress-box{
		width: 80%;
		margin: auto;
	}
	.data-login {
		height: 660upx;
		width: 660upx;
		margin: auto;
	}
	
	/* 提问转发 */
	.cover-list-icon {
		height: 70upx;
		width: 70upx;
		display: block;
		margin: auto;
	}

	.cover-list-btn {
		margin: 0upx;
		padding: 0upx;
		background-color: unset;
		line-height: unset;
	}

	.cover-list-font {
		color: #807C7C;
		font-size: 24upx;
		text-align: center;
		margin-top: 16upx;
	}

	.cover-open-right-bot {
		width: 100%;
		height: 100%;
		transform: rotate(180deg);
	}

	.cover-open-right-up {
		width: 100%;
		height: 100%;
	}

	.cover-open-right {
		position: absolute;
		right: 44upx;
		height: 32upx;
		width: 32upx;
	}

	.cover-list-view {
		background-color: #FFFFFF;
		border-radius: 20upx;
		margin: 28upx;
		height: 0upx;
		display: flex;
		justify-content: space-around;
		align-items: center;
		overflow: hidden;
		/* box-shadow: #a5a5a5 4px 12px 17px; */
	}

	.cover-h-icon {
		height: 30upx;
		width: 30upx;
		overflow: hidden;
		margin-left: 28upx;
		margin-right: 14upx;
	}

	.cover-view-h {
		background-color: #1190ED;
		border-radius: 20upx;
		margin: 28upx;
		color: #FFFFFF;
		height: 80upx;
		font-size: 28upx;
		display: flex;
		align-items: center;

	}

	.cove-bom {
		position: fixed;
		bottom: 0upx;
		left: 0upx;
		width: 100%;
		z-index: 1020;
	}

	/* 问题列表 */
	.que-time {
		color: #D5D5D5;
		font-size: 20rpx;
		margin-top: 14upx;
	}

	.que-wen {
		margin-top: 14upx;
		color: #7c7c7c;
		font-size: 28upx;
	}

	.que-name {
		font-size: 26upx;
	}

	.que-text {
		color: #cecccc;
		margin-left: 10upx;
	}

	.que-description {
		margin-left: 20upx;
	}

	.que-user {
		height: 50upx;
		width: 50upx;
		border-radius: 100upx;
		background-color: #00BFFF;
		color: #FFFFFF;
		font-size: 24upx;
		line-height: 50upx;
		text-align: center;
		flex-shrink: 0;
	}

	.que-user-d {
		background-color: #4c8cec !important;
	}

	.que-lisr-v {
		border-radius: 20upx;
		margin: 30upx;
		display: flex;
	}

	/* 评论 */
	.leaving-user-time {
		color: #D5D5D5;
		font-size: 20upx;
	}

	.leaving-user-info {
		margin-left: 20upx;
	}

	.leaving-user-img {
		background-color: #007AFF;
		border-radius: 10upx;
		height: 80upx;
		width: 80upx;
		flex-shrink: 0;
	}

	.leaving-view {
		background-color: #f8f8f8;
		border-radius: 10upx;
		padding: 20upx;
		margin: 30upx;
		display: flex;
	}

	/* me-svg */
	.Vajra-list-font {
		color: #807C7C;
		font-size: 24upx;
		text-align: center;
		margin-top: 16upx;
	}

	.Vajra-list-img {
		height: 60upx;
		width: 60upx;
		overflow: hidden;
		margin: auto;
	}

	.me-sbg-btn {
		/* overflow-y: hidden; */
		justify-content: center;
		align-items: center;
	}

	.me-svg {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin: 30upx;
		background-color: #FFFFFF;
		padding: 30upx 0upx 20upx 0upx;
		border-radius: 16upx;
	}

	/* 资源 */
	.list-li-left-describe {
		margin: 30upx 0upx 10upx 0upx;
		font-size: 20upx;
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.list-li-tag {
		color: #D5D5D5;
		font-size: 20upx;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-line-clamp: 2;
		-webkit-box-orient: vertical;
		margin-top: 20upx;
	}

	.list-img {
		height: 100upx;
		width: 120upx;
		border-radius: 14upx;
		overflow: hidden;
		flex-shrink: 0;
	}

	.list-view {
		margin: 30upx;
		background-color: #fff;
		padding: 30upx;
		border-radius: 20upx;
		box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.10);
	}

	.list-li {
		display: flex;
		align-items: center;
		margin: 20upx 0upx;
	}

	/* 关注微信公众号 */
	.account-aout {
		margin-left: 30upx;
	}

	.account-btn {
		height: 60upx;
		background-color: #007AFF;
		border-radius: 200upx;
		font-size: 24upx;
		width: 184upx;
		color: #FFFFFF;
		flex-shrink: 0;
		margin-right: 0upx;
	}

	.account-describe {
		font-size: 20upx;
		color: #7c7c7c;
	}

	.account-img {
		height: 80upx;
		width: 80upx;
		border-radius: 200upx;
		overflow: hidden;
	}

	.account-flex {
		display: flex;
	}

	.account-view {
		height: 120upx;
		border-radius: 20upx;
		background-color: #f7f7f7;
		margin: 0upx 28upx;
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 0upx 18upx;
	}

	/* 没有数据时候 */
	.no-text {
		color: #ADADAD;
		margin-top: 40upx;
		font-size: 30upx;
	}

	.no-img {
		border-radius: 20upx;
		overflow: hidden;
		height: 700upx;
		width: 500upx;
	}

	.no-datalist {
		position: absolute;
		top: 40%;
		left: 50%;
		transform: translate(-50%, -50%);
	}

	/* 列表没有数据 */
	.no-list-data {
		text-align: center;
		margin: 30upx 0px;
		color: #ADADAD;
	}

	/* 列表 */
	.list-li-left-describe {
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-line-clamp: 1;
		-webkit-box-orient: vertical;
		color: #D5D5D5;
		font-size: 20upx;
		display: flex;
		justify-content: space-between;
	}

	.list-li-left-h {
		font-size: 26upx;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-line-clamp: 2;
		-webkit-box-orient: vertical;
	}

	.list-li-left {
		margin-left: 32upx;
		flex-grow: 1;
		height: 116upx;
		display: flex;
		flex-flow: column;
		justify-content: space-between;
	}

	.list-img {
		height: 100upx;
		width: 120upx;
		border-radius: 14upx;
		overflow: hidden;
		flex-shrink: 0;
	}

	.list-li {
		display: flex;
		margin: 20upx 48upx;
		align-items: center;
		border-radius: 20upx;
	}

	.list-li-toew {
		display: flex;
		margin: 20upx 0upx;
		align-items: center;
		border-radius: 20upx;
		justify-content: space-around;
	}

	.list-icon-list {
		text-align: center;
	}

	.list-icon-list-btn {
		text-align: center;
		margin: 0;
		background-color: unset;
		padding: 0;
	}

	.list-icon-list-img {
		height: 60upx;
		width: 60upx;
		overflow: hidden;

	}

	.list-icon-list-img-title {
		font-size: 22upx;
		color: #b1b1b1;
		margin-top: 12upx;
	}

	.list-li-toew-ma {
		margin: 40upx 0upx 20upx 0upx;
	}

	/* 标题 */
	.title-h-ad {
		height: 240upx;
		border-radius: 16upx;
		margin: 48upx;
		overflow: hidden;
	}

	.titel-felx {
		display: flex;
		align-items: center;
		padding: 28upx;
		border-radius: 20upx;
	}

	.titel-h-go {
		font-size: 32upx;
		margin-left: 20upx;
	}

	/* 金刚区域 */
	.district-poster {
		width: 220upx;
		height: 60upx;
		border-radius: 200upx;
		background: linear-gradient(90deg, rgba(67, 130, 235, 1) 0%, rgba(6, 189, 254, 1) 100%);
		text-align: center;
		line-height: 64upx;
		color: #FFFFFF;
		font-size: 28upx;
		margin: 0upx 20upx;
		box-shadow: #4284ec 0upx 4upx 12upx;
	}

	.district-fenrid {
		width: 220upx;
		height: 60upx;
		border-radius: 200upx;
		background: linear-gradient(90deg, #F86168, #FFC6D3 100%);
		text-align: center;
		line-height: 64upx;
		color: #FFFFFF;
		font-size: 28upx;
		margin: 0upx 20upx;
		box-shadow: #F86168 0upx 4upx 12upx;
	}

	.district-money {
		width: 220upx;
		height: 60upx;
		border-radius: 200upx;
		background: linear-gradient(90deg, rgba(12, 185, 162, 1) 0%, rgba(15, 236, 210, 1) 100%);
		text-align: center;
		line-height: 64upx;
		color: #FFFFFF;
		font-size: 28upx;
		margin: 0upx 20upx;
		box-shadow: #0bbda6 0upx 4upx 12upx;
	}

	.district-fabulous {
		width: 180upx;
		height: 60upx;
		border-radius: 200upx;
		background: linear-gradient(90deg, rgba(248, 97, 104, 1) 0%, rgba(255, 198, 211, 1) 100%);
		text-align: center;
		line-height: 64upx;
		color: #FFFFFF;
		font-size: 28upx;
		margin: 0upx 16upx;
		box-shadow: #f96a71 0upx 6upx 22upx;
	}

	.district {
		display: flex;
		align-items: center;
		justify-content: center;
	}

	/* 评语 */
	.span-color {
		color: red;
	}

	.review {
		background-color: #f9f9f9;
		padding: 32upx;
		margin: 20upx 30upx;
		border-radius: 10upx;
		color: #807C7C;
		font-size: 24upx;
	}

	/* 正文内容 */
	.data-font {
		margin-top: 40upx;
	}

	.data-h:after {
		content: '';
		position: absolute;
		bottom: -14upx;
		left: 0px;
		width: 96upx;
		height: 8upx;
		border-radius: 200upx;
		background: linear-gradient(90deg, rgba(55, 110, 234, 1) 0%, rgba(255, 255, 255, 1) 100%);
	}

	.data-h {
		color: #000000;
		font-size: 32upx;
		position: relative;
	}

	.data-img {
		border-radius: 10upx;
		width: 100%;
		height: auto;
		overflow: hidden;
		margin: 30upx 0upx;
	}

	.data-view {
		margin: 20upx 30upx;
		font-size: 34upx;
		color: #333232;
		line-height: 60upx; //文字行高

	}

	/* 时间和浏览量 */

	.time-le {
		margin-left: 20upx;
	}

	.ma-left-20 {
		margin-left: 20upx;
	}

	.time-font-2 {
		background-color: #796bf7;
		color: #FFFFFF;
		font-size: 24upx;
		border-radius: 10upx;
		padding: 4upx 20upx;
		margin-left: 10upx;
	}

	.time-font {
		font-weight: 400;
		font-size: 24upx;
		color: rgba(124, 124, 124, 1);
		margin-left: 10upx;
	}

	.time-img {
		height: 40upx;
		width: 40upx;
	}

	.time-view {
		margin: 30upx 28upx;
		display: flex;
		align-items: center;
		justify-content: flex-start;
	}

	/* 标题 */
	.tar-w-h {
		color: #000000;
	}

	.titel-h {
		font-size: 38upx;
		font-weight: bold;
	}

	.titel-view {
		width: 180upx;
		flex-shrink: 0;
	}

	.titel-h-w {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin: 50upx 30upx 0upx 30upx;
		flex-grow: 1;
	}

	/* 背景图片 */
	.fengrui-img-tar {
		height: 40upx;
		width: 40upx;
	}

	.square-black-img {
		height: 40upx;
		width: 40upx;
		margin-top: 10upx;
	}

	.square-black {
		position: fixed;
		left: 28upx;
		background: rgba(255, 255, 255, 0.5);
		height: 55upx;
		z-index: 2;
		width: 100upx;
		border-radius: 100upx;
		top: 0upx;
		text-align: center;
	}

	.back-radius {
		position: absolute;
		bottom: 0;
		left: 0;
		height: 60upx;
		width: 100%;
		border-radius: 40upx 40upx 0upx 0upx;
		background: #FFFFFF;
		z-index: 2;
	}

	.back-img {
		overflow: hidden;
		position: relative;
		background-color: #3482e2;
		border-radius: 0upx 0upx 40upx 40upx;
		padding-bottom: 20upx;
		height: 20upx;
	}

	.fengrui {
		height: 100%;
		width: 100%;
	}

	/* 自定义标题栏 */
	.tar-img {
		margin-left: 28upx;
		height: 48upx;
		width: 48upx;
		overflow: hidden;
	}

	.tar-t {
		background-color: #FFFFFF;
		position: fixed;
		left: 0;
		width: 100%;
		z-index: 9999;
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.tar-w {
		background-color: #FFFFFF;
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		z-index: 99999999999;
	}

	.fengrui-img {
		height: 100%;
		width: 100%;
	}

	.index-ov {
		/* overflow-x: hidden; */
	}

	button:after {
		border: 0px solid rgba(0, 0, 0, .2);

	}

	/* 富文本解析视频宽度 */
	.video {
		width: 100% !important;
	}


	/* 暗黑模式下应用的样式 */
	@media (prefers-color-scheme: dark) {
		page {
			background: #161616;
		}

		.back-radius {
			background: #161616;
		}

		.tar-w {
			background: #161616;
		}

		.tar-t {
			background: #161616;
		}

		.data-h {
			color: #d7d7d7;
		}

		.data-view {
			color: #d7d7d7 !important;
		}

		.cover-list-view,
		.account-view {
			background: #202020;
		}

		.me-svg {
			background: #202020;
		}

		.list-view {
			background: #202020;
		}

		.time-font {
			color: rgba(124, 124, 124, 1);
		}

		.fengrui-img-tar {
			color: rgba(201, 201, 201, 1);
		}

		.square-black-img {
			color: rgba(201, 201, 201, 1);
		}

		.tar-w-h {
			color: rgba(201, 201, 201, 1);
		}
	}
</style>
