<template>
	<view class="centem-body">
		<!-- 说明 -->
		<view class="que-user-view">
			<span class="que-user-red">*</span> 特别说明：应管理需求您在文章提交问题需要输入邮箱；同时您的留言我们将进行审核，感谢您的理解！
		</view>

		<!-- 留言内容 -->
		<form @submit="formSubmit" @reset="formReset">
			<view class="que-center-view">
				<view class="que-center-user-display">
					<view class="que-center-user-img">
						<image class="fengrui-img" :src="xingsu.avatarUrl" mode="aspectFit"></image>
					</view>
					<view class="que-center-user-name">
						{{ xingsu.nickName }}
					</view>
				</view>
				<view class="que-center-user-rea">
					<textarea  name="textarea_center" @blur="bindTextAreaBlur" :placeholder="placeholder" maxlength="280" placeholder-style="font-size:20upx;color:#4A4949;"/>
				</view>
				<view class="que-center-email-view">
					<view class="que-center-email-span">
						<span class="color-red">*</span>请输入邮箱:
					</view>
					<input class="que-center-iput" type="text" value="" placeholder="emple@qq.com" name="input" />
				</view>
			</view>
			<button class="que-center-butn" form-type="submit">提交</button>
		</form>
		
		
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
				placeholder:"请输入您的问题描述...",
				xingsu:[]
			}
		},
		onLoad(just) {
			// 判断用户是否登录
			this.checkLogin();
			this.posdCenterID = just.id;
			// console.log(this.posdCenterID)
		},
		methods: {
			// 检测是否有登入  赋值给xsUserInfo判断是否为空
			checkLogin: function() {
				var that = this;
				var xsUserInfo = uni.getStorageSync('xsUserInfo_key');
				console.log(xsUserInfo)
				if (xsUserInfo == '') {
					uni.showModal({
					    title: '提示',
					    content: '提交问题前请先登录',
					    success: function (res) {
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
				}else{
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
			
			// 点击提交获取输入数据
			formSubmit: function(e) {
				var that = this;
				var formdata = e.detail.value
				if(formdata.textarea_center == ""){
					uni.showToast({
						icon:"none",
					    title: '请输入问题描述内容哦！',
					    duration: 2000
					});
				}else if(formdata.input == ""){
					uni.showToast({
						icon:"none",
					    title: '为了更好审核内容请您输入邮箱地址！',
					    duration: 2000
					});
				}else{
					uni.request({
						method:'POST',
						data:{
							post:this.posdCenterID,
							author_name:that.xingsu.nickName,
							author_email:formdata.input,
							content:formdata.textarea_center,
							parent:0
						},
						header: {
						'content-type': 'application/json'// 默认值
						},
						url: API + '/wp-json/wp/v2/comments',
						success(res) {
							console.log(res)
							if(res.statusCode == 400){
								uni.showToast({
									icon:"none",
								    title: res.data.data.params.author_email,
								    duration: 3000
								});
							}else if(res.statusCode != 201 ){
								uni.showToast({
									icon:"none",
								    title: res.data.message,
								    duration: 3000
								});
							}else if(res.statusCode == 201){
								uni.showToast({
									icon:"none",
								    title: "我们即将审核您提交的内容！",
								    duration: 3000
								});
							}
						},
						fail(err) {
							console.log(err);
						}
					});
				}	
			},
			// 多行文本输入框
			bindTextAreaBlur: function (e) {
			    // console.log(e.detail.value)
			},
			
			
		}
	}
</script>

<style>
	/* 问题列表 */
	.que-name{
		
	}
	.que-description{
		margin-left: 20upx;
	}
	.que-user{
		height: 60upx;
		width: 60upx;
		border-radius: 100upx;
		background-color: #00BFFF;
		color: #FFFFFF;
		font-size: 24upx;
		line-height: 60upx;
		text-align: center;
	}
	.que-lisr-v{
		background-color: #FFFFFF;
		border-radius: 20upx;
		margin: 30upx;
		padding: 30upx;
		display: flex;
	}
	
	/* 其他问题 */
	.titel-h {
		font-size: 38upx;
		margin: 60upx 30upx;
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
	/* 留言内容 */
	.que-center-butn{
		color: #FFFFFF;
		background-color: #007AFF;
		border-radius: 200upx;
		width: 500upx;
		margin: 80upx auto;
	}
	.que-center-iput{
		border: 1px #ececec solid;
		border-radius: 8upx;
		padding: 10upx 20upx;
		margin-left: 10upx;
		width: 350upx;
	}
	.color-red{
		color: red;
		margin-right: 10upx;
	}
	.que-center-email-span{
		display: flex;
		font-size: 28upx;
		color: #B2B2B2;
	}
	.que-center-email-view{
		display: flex;
		justify-content: space-around;
		align-items: center;
	}
	.que-center-user-rea{
		background-color: #F8F8F8;
		margin: 50upx 0upx;
		padding:30upx;
		border-radius: 20upx;
		display: block;
		
	}
	.que-center-user-name {
		margin-left: 20upx;

	}

	.que-center-user-img {
		height: 60upx;
		width: 60upx;
		border-radius: 200upx;
		overflow: hidden;
	}

	.que-center-user-display {
		display: flex;
		justify-content: flex-start;
		align-items: center;
	}

	.que-center-view {
		background-color: #FFFFFF;
		margin: 30upx;
		padding: 50upx 30upx;
		border-radius: 20upx;
	}

	/* 说明 */
	.que-user-red {
		color: red;
		margin-right: 6upx;
	}

	.que-user-view {
		background-color: #EFEFEF;
		margin: 30upx;
		border-radius: 20upx;
		padding: 30upx;
		color: #B2B2B2;
		font-size: 24upx;
	}

	/* 全局 */
	.fengrui-img {
		height: 100%;
		width: 100%;
	}

	page {
		background-color: #F7F7F7;
	}
</style>
