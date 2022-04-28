<template>
	<view class="content">
		<view class="university_jieshao">
			<rich-text :nodes="gaikuang_data.text"></rich-text>
		</view>
		<view class="university_lianxi">
			<view class="university_lianxi_title">
				联系方式
				<view class="university_lianxi_border">
					
				</view>
			</view>
			<view class="university_lianxi_detail">
				<view class="lianxi_detail_phone lianxi_detail_xx">
					<view class="detail_xx_text">{{gaikuang_data.phone}}</view>
					<image class="detail_xx_icon" src="../../static/phone.png" mode=""></image>
				</view>
				<view class="lianxi_detail_homepage lianxi_detail_xx">
					<view class="detail_xx_text">{{gaikuang_data.address}}</view>
					<image class="detail_xx_icon" src="../../static/homepage.png" mode=""></image>
				</view>
				<view class="lianxi_detail_address lianxi_detail_xx">
					<view class="detail_xx_text">{{gaikuang_data.homePage}}</view>
					<!-- <uni-link class="detail_xx_text" :href="`${gaikuang_data.homePage}`" :text="`${gaikuang_data.homePage}`"></uni-link> -->
					<image class="detail_xx_icon" src="../../static/address.png" mode=""></image>
				</view>
			</view>
		</view>
		<view class="university_xiaoyou">
			<view class="university_xiaoyou_title">
				知名校友
				<view class="university_xiaoyou_border">
					
				</view>
			</view>
			<view class="university_xiaoyou_detail">
				<view class="xiaoyou_detail_phone xiaoyou_detail_xx" v-for="(item,index) in xiaoyou_arr" :key=index>
					<image class="detail_xx_icon" :src="`${item.image}`" mode=""></image>
					<view class="detail_xx_text">{{item.name}}</view>
					<view class="detail_xx_position">{{item.position}}</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data(){
			return {
				gaikuang_data:{},
				xiaoyou_arr:[]
			}
		},
		methods:{
			getmingren_xx(name) {
				uni.request({
					url:`http://47.107.23.59/gaokao-2.0/university/schoolMate`,
					data:{
						universityName:name
					},
					success: (res) => {
						console.log(res.data.data)
						this.xiaoyou_arr = res.data.data;
						console.log(this.xiaoyou_arr)
					}
				})
			}
		},
		onLoad(option){
			this.gaikuang_data = {
				text:option.text,
				phone:option.phone,
				address:option.address,
				homePage:option.homePage
			}
			// console.log(option)
			this.getmingren_xx(option.name);
		}
	}
</script>

<style lang="scss">
	.content {
		width: 100%;
		background-color: #f0f0f0;
		.university_jieshao {
			background-color: rgb(248,248,248);
			padding: 0 20rpx 25rpx;
			box-sizing: border-box;
			font-size: 26rpx;
			color: #7f7f7f;
			margin-bottom: 10rpx;
			p {
				text-indent:24px;
			}
		}
		.university_lianxi {
			background-color: rgb(248,248,248);
			padding: 10rpx 20rpx;
			box-sizing: border-box;
			.university_lianxi_title {
				padding: 10rpx 0 20rpx;
				font-size: 30rpx;
				font-weight: bold;
				// border-bottom: 1rpx solid black;
				.university_lianxi_border {
					height: 1px;
					transform: scaleY(0.3);
					margin-top: 20rpx;
					width: 100%;
					background-color: #b6b6b6;
				}
			}
			.university_lianxi_detail {
				
				
				.lianxi_detail_xx {
					font-size: 28rpx;
					color: #7f7f7f;
					margin-bottom: 10rpx;
					display: flex;
					justify-content: space-between;
					align-items: center;
					.detail_xx_text {
						width: 660rpx;
					}
					.detail_xx_icon {
						// margin-left: 50rpx;
						height: 30rpx;
						width: 30rpx;
					}
				}
				
			}
		}
		
		.university_xiaoyou {
			background-color: rgb(248,248,248);
			padding: 10rpx 20rpx;
			margin-top: 10rpx;
			box-sizing: border-box;
			.university_xiaoyou_title {
				padding: 10rpx 0 20rpx;
				font-size: 30rpx;
				font-weight: bold;
				// border-bottom: 1rpx solid black;
				.university_xiaoyou_border {
					height: 1px;
					transform: scaleY(0.3);
					margin-top: 20rpx;
					width: 100%;
					background-color: #b6b6b6;
				}
			}
			.university_xiaoyou_detail {
				padding: 0 0 30rpx;
				margin-top: 5rpx;
				display: flex;
				align-items: flex-start;
				.xiaoyou_detail_xx {
					margin-right: 15rpx;
					width: 120rpx;
					display: flex;
					flex-direction: column;
					justify-content: center;
					align-items: center;
					.detail_xx_icon {
						height: 100rpx;
						width: 100rpx;
						border-radius: 50%;
					}
					.detail_xx_text {
						font-size: 22rpx;
						margin-top: 10rpx;
					}
					.detail_xx_position {
						font-size: 20rpx;
						color: #afafaf;
						text-align: center;
						margin-top: 5rpx;
					}
				}
				
			}
		}
			
	}
</style>
