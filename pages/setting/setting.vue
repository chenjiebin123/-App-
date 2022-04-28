<template>
	<view class="content">
		<view class="mm-contents">
			<view class="mm-my" @click="to_selfxx">
				<view class="left" >
					个人信息
				</view>
				<image src="../../static/to.png" mode=""></image>
			</view>
			<view class="mm-mima" @click="to_updatapw">
				<view class="left">
					修改密码
				</view>
				<image src="../../static/to.png" mode=""></image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data(){
			return {}
		},
		methods:{
			to_updatapw() {
				uni.navigateTo({
					url:'./updatapw/updatapw'
				})
			},
			to_selfxx() {
				uni.navigateTo({
					url:'./selfxx/selfxx'
				})
			}
		},
		onLoad() {
			
		},
		created() {
			
		}
	}
</script>

<style lang="scss">
	
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
		margin: 20rpx 0;
	}
	
	.mm-contents image {
		width: 40rpx;
		height: 40rpx;
	}
</style>
