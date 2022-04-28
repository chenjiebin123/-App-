<template>
	<view class="content">
		<view class="newprot_title">
			{{newprotDetail.title}}
		</view>
		<view class="newprot_writerAndtime">
			<view class="newprot_writer">
				{{newprotDetail.writer}}
			</view>
			<view class="newprot_time">
				{{newprotDetail.time}}
			</view>
		</view>
		<image class="newprot_img" :src="`${newprotDetail.image}`" mode=""></image>
		<view class="newprot_content">
			
			<rich-text :nodes="newprotDetail.content"></rich-text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				newprottitle:'',
				newprotDetail:{}
			}
		},
		methods:{
			getnewProtList() {
				uni.request({
					url:'http://localhost:3000/api/getNewprotDetail',
					data:{
						title:this.newprottitle
					},
					success: (res) => {
						console.log(res.data.result.data[0]);
						this.newprotDetail = res.data.result.data[0]
					}
				})
			}
		},
		onLoad(option) {
			console.log(option.title)
			this.newprottitle = option.title
			uni.setNavigationBarTitle({
				title: this.newprottitle
			})
			this.getnewProtList();
		},
		created() {
			
		},
	}
</script>

<style lang="scss">
	.content {
		width: 100%;
		padding: 30rpx;
		box-sizing: border-box;
		.newprot_title {
			font-size: 34rpx;
			font-weight: bold;
			margin-bottom: 20rpx;
		}
		.newprot_writerAndtime {
			font-size: 20rpx;
			display: flex;
			color: #9c9c9c;
			margin-bottom: 30rpx;
			.newprot_writer {
				// transform: scale(0.8);
			}
			.newprot_time {
				// transform: scale(0.8);
				margin-left: 30rpx;
			}
		}
		.newprot_img {
			// height: 350rpx;
			width: 100%;
		}
		.newprot_content {
			font-size: 32rpx;
			text-indent: 40rpx;
			
		}
		
	}
</style>
