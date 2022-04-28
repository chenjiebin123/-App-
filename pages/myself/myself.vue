<template>
	<view class="mm-my">
		<view class="mm-head">
			<view class="mm-bgimg">
				<!-- <image src="../../static/bg.png" mode='heightFix'></image> -->
			</view>
			<view class="mm-head-info" @click="to_LoginOrregister">
				<view class="info-userimg">

					<image v-if="userinfo" :src="user_image" mode=""></image>
					<image v-else src="../../static/user_none_longin.png" mode=""></image>
				</view>
				<view class="info-content">
					<view class="info-content-name" v-if="userinfo">
						{{username}}
					</view>
					<view class="info-content-name" v-else>
						注册/登录
					</view>
				</view>
			</view>
		</view>
		<view class="mm-contents">
			<view class="mm-my" @click="to_setting" v-if="userinfo">
				<view class="left" >
					设置中心
				</view>
				<image src="../../static/to.png" mode=""></image>
			</view>
			<view class="mm-content" @click="to_shoucang" v-if="userinfo">
				<view class="left">
					我的收藏
				</view>
				<image src="../../static/to.png" mode=""></image>
			</view>
			<view class="mm-my" @click="to_about">
				<view class="left" >
					关于我们
				</view>
				<image src="../../static/to.png" mode=""></image>
			</view>
			<view class="mm-content" @click="user_exit" v-if="userinfo">
				<view class="left">
					安全退出
				</view>
				<image src="../../static/to.png" mode=""></image>
			</view>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
				username: '',
				userinfo: false,
				user_image:'../../static/man_user.png'
			}
		},
		onShow() {
			// console.log(1)
			uni.getStorage({
					key: 'token',
					success: (res) => {
						// if(res.token)
						// console.log(res.data)
						if (res.data) {
							this.userinfo = true
						}
					}
				});
				uni.getStorage({
					key: 'userName',
					success: (res) => {
						// if(res.token)
						// console.log(res.data)
						if (res.data) {
							this.username = res.data
						}
					}
				})
		},
		methods: {
			to_about(){
				uni.navigateTo({
					url:"../about/about"
				})
			},
			to_setting() {
				uni.navigateTo({
					url:'../setting/setting'
				})
			},
			to_LoginOrregister() {
				// console.log(111)
				if(this.userinfo==false){
					uni.navigateTo({
						url: '../login/login'
					})
				}
			},
			to_shoucang() {
				uni.navigateTo({
					url: '../shoucang/shoucang'
				})
			},
			user_exit() {
				this.userinfo = false;
				uni.showToast({
					title: '退出成功',
					duration: 1000,
				});
				uni.removeStorage({
					key: 'token',
					success: function(res) {
						// console.log('success');
						
					}
				});
				uni.removeStorage({
					key: 'userName',
					success: function(res) {
						// console.log('success');
					}
				})
			}
		}
	}
</script>
<style>
	.mm-head {
		position: relative;
	}

	.mm-head .mm-bgimg {
		position: absolute;
		top: 0;
		left: 0;
	}

	.info-userimg {
		width: 150rpx;
		height: 150rpx;
	}

	.info-userimg image {
		border-radius: 50%;
		width: 150rpx;
		height: 150rpx;
	}

	.mm-head-info {
		width: 100%;
		display: flex;
		justify-content: flex-start;
		align-items: center;
		padding: 50rpx;
		box-sizing: border-box;
	}

	.info-content {
		margin-left: 30rpx;
		position: relative;
		font-size: 40rpx;
		z-index: 2;
	}

	.mm-money,
	.mm-contents {
		padding: 10rpx 20rpx;
		position: relative;
		z-index: 2;
	}

	.mm-contents>view {
		background-color: #FFFFFF;
		padding: 20rpx;
		display: flex;
		justify-content: space-between;
		align-items: center;
		border-bottom: 1px solid #CCCCCC;
	}

	.mm-contents .mm-my {
		border-top-right-radius: 20rpx;
		border-top-left-radius: 20rpx;
	}

	.mm-contents image {
		width: 40rpx;
		height: 40rpx;
	}
</style>
