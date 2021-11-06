<template>
	<view>
		<view class="que-post-view">
			<image class="fengrui-img" src="../../static/data/faq.svg" mode="widthFix"></image>
		</view>
		
		<!-- 标题 -->
		<view class="titel-h-w">
			<view class="titel-h">
				其他问题
			</view>
		</view>
		
		<!-- 提问题列表 -->
		<block >
			<view class="que-lisr-v" v-for="(item ,index) in CommentsList" :key="index">
				<view class="que-user" v-if="item.parent == '0'">
					问
				</view>
				<view class="que-user que-user-d" v-else >
					答
				</view>
				<view class="que-description">
					<view class="que-name">
						{{ item.author_name }} 
						<text class="que-text" v-for="(userItem,i) in CommentsList" :key="i" v-if="userItem.id == item.parent">
							回答：  {{userItem.author_name}}
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
				posrId:"",
				CommentsList:[],
				CommentsListTown:[],
				page:1,
			}
		},
		// 监听触底----上啦刷新
		onReachBottom() {
			this.page = this.page + 1;
			this.GetPostComments();
		},
		onLoad(e) {
			console.log(e)
			this.posrId = e.id;
			// 请求评论
			this.GetPostComments()
		},
		methods: {
			GetPostComments:function(){
				var that = this;
				uni.request({
					url: API + '/wp-json/wp/v2/comments?page=' + that.page,
					success(res) {
						that.CommentsList = that.CommentsList.concat(res.data);
					},
					fail(err) {
						console.log(err);
					}
				});
			}
		}
	}
</script>

<style>
	/* 问题列表 */
	.que-time{
		color: #D5D5D5;
		font-size: 20rpx;
		margin-top: 14upx;
	}
	.que-wen{
		margin-top: 14upx;
		color: #7c7c7c;
		font-size: 28upx;
	}
	.que-name{
		font-size: 26upx;
	}
	.que-text{
		color: #cecccc;
		margin-left: 10upx;
	}
	.que-description{
		margin-left: 20upx;
	}
	.que-user{
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
	.que-user-d{
		background-color: #4c8cec !important;
	}
	.que-lisr-v{
		background-color: #FFFFFF;
		border-radius: 20upx;
		margin: 30upx;
		padding: 30upx;
		display: flex;
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
	
	/* 提交问题 */
	.que-post-view{
		height: 660upx;
	}
	/* 全局 */
	.fengrui-img {
		height: 100%;
		width: 100%;
	}
	
	page {
		background-color: #F7F7F7;
	}
	/* 暗黑模式下应用的样式 */
	@media (prefers-color-scheme: dark) {
		page {
			background: #161616;
		}
		.que-lisr-v {
			background-color: #202020;
		}
	}
</style>
