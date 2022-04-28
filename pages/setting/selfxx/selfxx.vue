<template>
	<view class="content">
		<view class="userName list_box">
			<view class="left">
				<view class="title">
					用户名
				</view>
			</view>
			<view class="right">
				<input class="box_input" type="text" value="" v-model="userName" />
				<image class="box_icon" src="../../../static/to.png" mode=""></image>
			</view>
		</view>
		<view class="phone list_box">
			<view class="left">
				<view class="title">
					联系方式
				</view>
			</view>
			<view class="right">
				<input class="box_input" type="text" value="" v-model="phone" />
				<image class="box_icon" src="../../../static/to.png" mode=""></image>
			</view>
		</view>
		<view class="sex list_box">
			<view class="left">
				<view class="title">
					性别
				</view>
			</view>
			<view class="right">
				<input class="box_input" type="text" value="" v-model="sex" />
				<image class="box_icon" src="../../../static/to.png" mode=""></image>
			</view>
		</view>
		<view class="btn" @click="updata_self_xx">
			保存
		</view>
	</view>
</template>

<script>
	export default {
		data(){
			return {
				login_name:'',
				userName:'',
				phone:'',
				sex:'',
				selft_xx:{}
			}
		},
		methods:{
			getself_xx(userName) {
				uni.request({
					url:'http://localhost:3000/api/self_xx',
					data:{
						userName
					},
					success: (res) => {
						this.selft_xx = res.data.result.data;
						this.userName= this.selft_xx.userName;
						this.phone = this.selft_xx.phone;
						this.sex = this.selft_xx.sex;
					}
				})
			},
			updata_self_xx() {
				console.log(this.userName,this.phone,this.sex,this.login_name)
				
				uni.request({
					url:'http://localhost:3000/api/updata_sele_xx',
					data:{
						userName:this.userName,
						phone:this.phone,
						sex:this.sex,
						login_name:this.login_name
					},
					method:'POST',
					success: (res) => {
						console.log(res.data);
						if(res.data.status==200) {
							uni.setStorage({
								key:'userName',
								data:this.userName
							})
						}
						if(res.data.status==300) {
							uni.showToast({
								title:res.data.reason,
								duration:1000,
								icon:'error'
							})
						}
					}
				})
			}
		},
		onLoad() {
			uni.getStorage({
				key:'userName',
				success: (res) => {
					this.login_name = res.data
				}
			});
			this.getself_xx(this.login_name)
		},
		created() {
			
		}
	}
</script>

<style lang="scss">
	.content {
		width: 100%;
		background-color: rgb(239, 239, 239);
		overflow: hidden;
		.list_box {
			display: flex;
			align-items: center;
			justify-content: space-between;
			padding: 20rpx 20rpx;
			margin-top: 10rpx;
			background-color: white;
			.left {
				.title {
					font-size: 34rpx;
				}
			}
			.right {
				display: flex;
				align-items: center;
				.box_input {
					width: 250rpx;
					// background-color: red;
					font-size: 28rpx;
					text-align: right;
				}
				.box_icon {
					margin-left: 20rpx;
					width: 30rpx;
					height: 30rpx;
				}
			}
		}
		
		.btn {
			position: fixed;
			bottom: 0;
			width: 100%;
			height: 80rpx;
			color: white;
			background-color: #019aff;
			text-align: center;
			line-height: 80rpx;
			font-size: 32rpx;
		}
	}
</style>
