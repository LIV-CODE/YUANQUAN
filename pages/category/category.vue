de <template>
	<view class="page-ov">
		<!-- 0.6 正方形图 -->
		<view class="" v-if="about_center[0].categ_style != 2">
			<view class="list-two-w">
				<view class="list-two-list">
					<view class="list-name">
						ResourseSpring 源泉
					</view>
					<view class="list-name-slug">
						兮其若朴，旷兮其若谷
					</view>
				</view>
				<view class="list-two-img">
					<image class="fengrui-img" src="../../static/index/333.svg" mode="aspectFill"></image>
				</view>
			</view>

			<!-- 专题滑动区域 -->
			<view class="swiper-list">
				<scroll-view class="scroll-view_H" scroll-x="true"  show-scrollbar="false">
					<block v-for="(cate ,index) in categoryList" :key="index" >
						<view  class="swiperzhuanti" hover-class="fengrui-li-hover" v-if="cate.xs_top == '1'" @tap="goArticleList(cate.id)">
							<view class="scroll-h-name">
								<view class="scroll-ruan">
									<image class="fengrui-img" src="../../static/down/ruan.svg" mode="aspectFit"></image>
								</view>
								{{ cate.name }}
							</view>
							<view class="list-name-slug">
								{{ cate.description }}
							</view>
							<view class="list-name-slug">
								数量：{{ cate.count }}
							</view>
							<view class="scroll-zhuan-img">
								<image class="fengrui-img" style="Opacity:0.9" :src="cate.cation_img" mode="aspectFit"></image>
							</view>
						</view>
					</block>
				</scroll-view>
			</view>

			<view class="subject-view">
				<view class="subject-h">
					专题分类
				</view>
				<view class="subject-list-v">
					<view class="subject-list" hover-class="fengrui-li-hover" v-for="(cate ,index) in categoryList" :key="index"
						@tap="goArticleList(cate.id)">
						<view class="subject-list-icon">
							<image class="fengrui-img" :src="cate.cation_img" mode="aspectFit"></image>
						</view>
						<view class="subject-list-name">
							{{ cate.name }}
						</view>
					</view>
				</view>
			</view>
			<!-- #ifdef MP-WEIXIN -->
			<!-- 关注微信公众号 -->
			<!-- <view class="account-view" v-if="about_center[0].public_follow != ''">
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
					<button hover-class="fengrui-blue-btn-lo-hover" class="account-btn" @tap="accountTap(about_center[0].public_follow)">关注</button>
				</view>
			</view> -->
			<!-- #endif -->
		</view>

		<!-- 星尘大图样式 -->
		<block v-if="about_center[0].categ_style == 2">
			<view hover-class="fengrui-li-hover" class="list-big-view" v-for="(cate ,index) in categoryList" :key="index"
				@tap="goArticleList(cate.id)">
				<view class="list-big-img">
					<image class="fengrui-img" :src="cate.cation_img" mode="aspectFill"></image>
				</view>
				<view class="list-big-h">
					{{ cate.name }}
				</view>
				<view class="list-big-slug">
					{{ cate.description}}
				</view>
				<view class="list-big-cout">
					数量：{{ cate.count }}
				</view>
			</view>
		</block>
	</view>
</template>

<script>
	// 引入域名和自定义php文件
	import {
		API
	} from '@/components/api.js';
	export default {
		data() {
			return {
				// 分类数据
				categoryList: [{"id":147,"count":0,"description":"","link":"https:\/\/www.frbkw.com\/category\/nativeapplet\/","name":"哈哈哈","slug":"nativeapplet","taxonomy":"category","parent":0,"meta":[],"cation_img":"","xs_top":"0","_links":{"self":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/categories\/147"}],"collection":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/categories"}],"about":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/taxonomies\/category"}],"wp:post_type":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/posts?categories=147"}],"curies":[{"name":"wp","href":"https:\/\/api.w.org\/{rel}","templated":true}]}},{"id":1,"count":20,"description":"\u5206\u4eab\u4e00\u4e9b\u767e\u5ea6\u7f51\u76d8\u8d44\u6e90","link":"https:\/\/www.frbkw.com\/category\/shiyonggongju\/uncategorized\/","name":"\u76d8\u5ea6","slug":"uncategorized","taxonomy":"category","parent":8,"meta":[],"cation_img":"https:\/\/cdn.frbkw.com\/xingshu\/muen\/renking-1.svg","xs_top":"0","_links":{"self":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/categories\/1"}],"collection":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/categories"}],"about":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/taxonomies\/category"}],"up":[{"embeddable":true,"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/categories\/8"}],"wp:post_type":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/posts?categories=1"}],"curies":[{"name":"wp","href":"https:\/\/api.w.org\/{rel}","templated":true}]}},{"id":5,"count":21,"description":"mui\u79fb\u52a8\u7aef\u6846\u67b6\u6587\u7ae0","link":"https:\/\/www.frbkw.com\/category\/mui\/","name":"MUI","slug":"mui","taxonomy":"category","parent":0,"meta":[],"cation_img":"https:\/\/cdn.frbkw.com\/xingshu\/muen\/renking-1.svg","xs_top":"0","_links":{"self":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/categories\/5"}],"collection":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/categories"}],"about":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/taxonomies\/category"}],"wp:post_type":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/posts?categories=5"}],"curies":[{"name":"wp","href":"https:\/\/api.w.org\/{rel}","templated":true}]}},{"id":7,"count":25,"description":"\u4e00\u4e9b\u4e0d\u8d77\u773c\u7684H5\u5e72\u8d27","link":"https:\/\/www.frbkw.com\/category\/qianduanyuanma\/","name":"web\u524d\u7aef","slug":"qianduanyuanma","taxonomy":"category","parent":0,"meta":[],"cation_img":"https:\/\/cdn.frbkw.com\/xingshu\/muen\/renking-5.svg","xs_top":"0","_links":{"self":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/categories\/7"}],"collection":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/categories"}],"about":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/taxonomies\/category"}],"wp:post_type":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/posts?categories=7"}],"curies":[{"name":"wp","href":"https:\/\/api.w.org\/{rel}","templated":true}]}},{"id":8,"count":48,"description":"\u6211\u5bf9\u5b83\u7684\u5b9a\u4f4d\u8ff7\u832b","link":"https:\/\/www.frbkw.com\/category\/shiyonggongju\/","name":"\u5b9e\u7528\u5de5\u5177","slug":"shiyonggongju","taxonomy":"category","parent":0,"meta":[],"cation_img":"https:\/\/cdn.frbkw.com\/xingshu\/muen\/renking-2.svg","xs_top":"1","_links":{"self":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/categories\/8"}],"collection":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/categories"}],"about":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/taxonomies\/category"}],"wp:post_type":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/posts?categories=8"}],"curies":[{"name":"wp","href":"https:\/\/api.w.org\/{rel}","templated":true}]}},{"id":92,"count":69,"description":"Uni-app\u591a\u7aef\u6846\u67b6\u6587\u7ae0","link":"https:\/\/www.frbkw.com\/category\/uniyuanma\/","name":"Uni-app","slug":"uniyuanma","taxonomy":"category","parent":0,"meta":[],"cation_img":"https:\/\/cdn.frbkw.com\/xingshu\/muen\/renking-6.svg","xs_top":"1","_links":{"self":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/categories\/92"}],"collection":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/categories"}],"about":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/taxonomies\/category"}],"wp:post_type":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/posts?categories=92"}],"curies":[{"name":"wp","href":"https:\/\/api.w.org\/{rel}","templated":true}]}},{"id":129,"count":8,"description":"\u81ea\u6211\u5b66\u4e60\u6210\u957f\u7684\u7ecf\u9a8c\u4ee5\u53ca\u5fc3\u5f97\u6587\u7ae0","link":"https:\/\/www.frbkw.com\/category\/ymyself\/","name":"\u81ea\u6211\u6210\u957f","slug":"ymyself","taxonomy":"category","parent":0,"meta":[],"cation_img":"https:\/\/cdn.frbkw.com\/xingshu\/muen\/renking-4.svg","xs_top":"0","_links":{"self":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/categories\/129"}],"collection":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/categories"}],"about":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/taxonomies\/category"}],"wp:post_type":[{"href":"https:\/\/www.frbkw.com\/wp-json\/wp\/v2\/posts?categories=129"}],"curies":[{"name":"wp","href":"https:\/\/api.w.org\/{rel}","templated":true}]}}],

				// 分类图片样式
				list_style: 2,

				// 数据缓存
				totalte_sources_key: [],

				// 过滤数组
				about_center: [],
			}
		},
		onLoad() {
			// 自定义php接口请求
			this.Getres();

			// 微信分享盆友圈
			//#ifdef MP-WEIXIN
			wx.showShareMenu({
				withShareTicket: true,
				menus: ['shareAppMessage', 'shareTimeline']
			})
			//#endif
		},

		// 下拉刷新
		onPullDownRefresh() {
			this.postList();
			setTimeout(function() {
				uni.stopPullDownRefresh();
			}, 1000);
		},

		// 分享好友配置
		onShareAppMessage(res) {
			var that = this;
			if (res.from === 'button') { // 来自页面内分享按钮
				console.log(res.target)
			}
			return {
				title: that.about_center[0].share_title,
				imageUrl: that.about_center[0].share_title_url,
				path: 'pages/category/category'
			}
		},

		methods: {
			// 自定义php接口请求
			Getres: function() {
				var that = this;
				uni.request({
					url: API + '/wp-json/wp/v2/fengrui',
					success: (res) => {
						// 关于数据
						that.about_center = res.data;
						// 获取列表
						this.postList();
					}
				});
			},

			// 点击跳转
			goArticleList: function(e) {
				var that = this;
				var catepos = e;
				console.log(catepos)
				uni.navigateTo({
					url: '../category/categ_lisst?catepos=' + catepos
				})
			},

			// 获取分类数据（接后端）
			// postList: function() {
			// 	var that = this;
			// 	uni.request({
			// 		url: API + '/wp-json/wp/v2/categories?per_page=100&exclude=' + that.about_center[0]
			// 			.categ_filter,
			// 		success(res) {
			// 			that.categoryList = res.data;
			// 		},
			// 		fail(err) {
			// 			console.log(err);
			// 		}
			// 	});
			// },

			// 关注微信公众号
			accountTap: function(e) {
				var that = this;
				var sve = e;
				uni.navigateTo({
					url: '../weblist/weblist?aurl=' + sve
				})
			},
		}

	}
</script>

<style>
	.scroll-h-name{
		display: flex;
		justify-content: flex-start;
		align-items: center;
	}
	.scroll-zhuan-img{
		width: 80upx;
		height: 80upx;
		position: absolute;
		right: 0upx;
		bottom: 0upx;
	}
	.swiperzhuanti:last-child {
		margin-right: 30upx;
	}
	.scroll-ruan{
		height: 30upx;
		width: 30upx;
		border-radius: 200upx;
		margin-right: 10upx;
		margin-bottom: 6upx;
	}
	.swiperzhuanti {
		width: 320upx;
		padding: 30upx;
		height: 140upx;
		background-color: #FFFFFF;
		border-radius: 20upx;
		display: inline-block;
		margin-left: 30upx;
		position: relative;
	}

	.swiper-list {
		white-space: nowrap;
	}

	/* 关注微信公众号 */
	.account-aout {
		margin-left: 30rpx;
	}

	.account-btn {
		height: 60upx;
		background-color: #007AFF;
		border-radius: 200upx;
		font-size: 24upx;
		width: 150upx;
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
		background-color: #FFFFFF;
		border-radius: 20upx;
		height: 120upx;
		margin: 30rpx 30upx;
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 18upx;
	}

	/* 星尘大图模式 */
	.list-big-cout {
		position: absolute;
		right: 30upx;
		bottom: 30upx;
		color: #807C7C;
		font-size: 20upx;
	}

	.list-big-slug {
		font-size: 20upx;
		color: #807C7C;
		margin: -20upx 30upx 30upx 30upx;
		
	}

	.list-big-h {
		font-size: 34upx;
		margin: 30upx;
	}

	.list-big-img {
		height: 260upx;
		overflow: hidden;
		background-color: #000000;
		border-radius: 20upx;
	}

	.list-big-view {
		position: relative;
		background-color: #FFFFFF;
		border-radius: 20upx;
		overflow: hidden;
		margin: 50upx 30upx;
	}

	/* 文章列表双排小图 */
	.subject-h {
		margin: 30upx;
	}

	.subject-list-name {
		text-align: center;
		margin-top: 18upx;
		font-size: 28upx;
	}

	.subject-list-v {
		display: flex;
		justify-content: start;
		align-items: center;
		flex-flow: wrap;

	}

	.subject-list {
		margin: 30upx 0upx;
		width: 228upx;
		flex-shrink: 0;
	}

	.subject-list-icon {
		height: 80upx;
		width: 80upx;
		margin: auto;
		border-radius: 10upx;

	}

	.subject-view {
		background-color: #FFFFFF;
		border-radius: 20upx;
		margin: 30upx 30upx;
		overflow: hidden;
	}

	.list-two-abstract {
		color: #ADADAD;
		font-size: 24upx;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-line-clamp: 2;
		-webkit-box-orient: vertical;
		margin-top: 14upx;
	}

	.list-two-title {
		font-size: 30upx;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-line-clamp: 2;
		-webkit-box-orient: vertical;
		margin-top: 26upx;
	}

	.list-two-img {
		width: 250upx;
		height: 250upx;
		border-radius: 20upx;
		/* overflow: hidden; */
		margin: -50upx 50rpx 0rpx 50rpx;
		flex-shrink: 0;
	}

	.list-two-img-all {
		width: 260upx;
		height: 130upx;
		border-radius: 20upx;
		overflow: hidden;
		flex-shrink: 0;
		margin-right: 50rpx;
	}

	.list-two-list {
		margin: 0upx 50upx;

	}

	.list-name {
		font-size: 38upx;
		margin-bottom: 6upx;
		
	}

	.list-name-slug {
		font-size: 20upx;
		color: #807C7C;
		margin-bottom: 6upx;
		
		margin-top: 16upx;
	}

	.list-two-w {
		margin: 30upx 30upx;
		display: flex;
		justify-content: space-between;
		align-items: center;
		background-color: #FFFFFF;
		border-radius: 20upx;
		/* overflow: hidden; */
		height: 220upx;
	}

	/* 热评 */
	.review {
		background-color: #f9f9f9;
		padding: 32upx;
		margin: 34upx 30upx;
		border-radius: 10upx;
		color: #807C7C;
		font-size: 24upx;
	}

	.status-ma40 {
		margin: 30upx;
	}

	.fengrui-img {
		height: 100%;
		width: 100%;
	}

	.page-ov {
		overflow: hidden;
	}

	page {
		background-color: #F7F7F7;
	}
	::-webkit-scrollbar {
	  width: 0;
	  height: 0;
	  color: transparent;
	}
	/* 暗黑模式下应用的样式 */
	@media (prefers-color-scheme: dark) {
		page {
			background: #161616;
		}

		.list-big-view {
			background: #202020;
		}

		.account-view,.subject-view,.swiperzhuanti,.list-two-w {
			background: #202020;
		}
	}
</style>
