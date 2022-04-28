<template>
	<view class="content">
		<view class="logo">
			<image class="logo_icon" src="../../static/user_none_longin.png" mode=""></image>
		</view>
		<view class="input">
			<view class="userName input_box">
				<input class="userName_input input_cont" type="text" placeholder="用户名" v-model="userName_cont" value="" />
				<image class="userName_icon input_icon" @click="clear_input" src="../../static/input_delete.png" mode=""></image>
			</view>
			<view class="password input_box">
				<input class="password_input input_cont" :type="eye_text" placeholder="密码" v-model="passWord_cont" value="" />
				<image class="userName_icon input_icon" @click="see_input" src="../../static/password_eye.png" mode=""></image>
			</view>
		</view>
		<view class="button">
			<view class="button_box" @click="login">
				登录
			</view>
		</view>
		<view class="other">
			<view class="other_zuce other_btn" @click="to_register">
				新用户注册
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userName_cont:'',
				passWord_cont:'',
				is_eye:false,
				eye_text:'password',
			}
		},
		onLoad() {},
		methods:{
			clear_input() {
				this.userName_cont="";
			},
			see_input() {
				this.is_eye=!this.is_eye;
				this.eye_text = this.is_eye==false?'password':'text';
			},
			
			login() {
				uni.request({
					url:'http://localhost:3000/api/user_login',
					data:{
						userName:this.userName_cont,
						password:this.passWord_cont
					},
					method:'POST',
					success: (res) => {
						console.log(res.data)
						if(res.data.status==200) {
							uni.showToast({
								title: res.data.reason,
								duration: 1000,
							});
							uni.setStorage({
								key: 'token',
								data: res.data.result.token,
								success: function () {
									// console.log('success');
								}
							});
							uni.setStorage({
								key: 'userName',
								data: res.data.result.userName,
								success: function () {
									// console.log('success');
								}
							});
							setTimeout(function(){
								uni.navigateBack({
									
								})
							},1000)
						}
						else if(res.data.status==300) {
							uni.showToast({
								title: res.data.reason,
								duration: 1000,
								icon:'error'
							});
						}
					}
				})
			},
			to_register() {
				uni.navigateTo({
					url:'../register/register'
				})
			}
		}
		
	}
</script>

<style lang="scss">
	.content {
		
		.logo {
			width: 100%;
			height: 300rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			.logo_icon {
				height: 150rpx;
				width: 150rpx;
			}
		}
		
		.input {
			height: 260rpx;
			width: 100%;
			padding: 0 40rpx;
			box-sizing: border-box;
			.input_box {
				width: 100%;
				padding: 20rpx 0;
				box-sizing: border-box;
				border-bottom: 1px solid #d0d0d0;
				display: flex;
				justify-content: space-between;
				align-items: center;
				.input_cont {
					height: 60rpx;
					font-size: 30rpx;
					flex: 1;
				}
				.password_input {
					border: none;
					outline: none;
				}
				.input_icon {
					height: 35rpx;
					width: 35rpx;
				}
			}
		}
		
		.button {
			width: 100%;
			padding: 0 40rpx;
			box-sizing: border-box;
			.button_box {
				width: 100%;
				height: 80rpx;
				background-color: #2c6bff;
				color: white;
				border-radius: 10rpx;
				text-align: center;
				line-height: 80rpx;
			}
		}
		
		.other {
			width: 100%;
			padding: 0 40rpx;
			box-sizing: border-box;
			display: flex;
			justify-content: space-between;
			align-items: center;
			height: 100rpx;
			.other_btn {
				color: #4e9eee;
			}
		}
	}
</style>
