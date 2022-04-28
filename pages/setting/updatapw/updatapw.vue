<template>
	<view class="content">
		<input type="password" class="old_pw input_pw" value="" v-model="old_pw" placeholder="请输入旧密码" />
		<input type="password" class="new_pw input_pw" value="" v-model="new_pw" placeholder="请输入新密码" />
		<view class="btn" @click="xiugai">
			修改密码
		</view>
	</view>
</template>

<script>
	export default {
		data(){
			return {
				old_pw:'',
				new_pw:'',
				userName:''
			}
		},
		methods:{
			xiugai() {
				// console.log(this.userName,this.old_pw,this.new_pw);
				if(this.old_pw.length==0) {
					uni.showToast({
						title: '旧密码不能为空',
						duration: 1000,
						icon:'error'
					});
					return;
				}
				if(this.new_pw.length==0) {
					uni.showToast({
						title: '新密码不能为空',
						duration: 1000,
						icon:'error'
					});
					return;
				}
				uni.request({
					url:'http://localhost:3000/api/updata_pw',
					data:{
						userName:this.userName,
						old_password:this.old_pw,
						new_password:this.new_pw
					},
					method:'POST',
					success: (res) => {
						console.log(1111)
						console.log(res)
						
						if(res.data.status==200){
							uni.showToast({
								title: res.data.reason+", 请重新登录",
								duration: 1000,
							});
							uni.removeStorage({
								key:'token'
							});
							uni.removeStorage({
								key:'userName'
							});
							setTimeout(()=>{
								uni.redirectTo({
									url:'../../login/login'
								})
							},1500)
						}else {
							uni.showToast({
								title: res.data.reason,
								duration: 1000,
								icon:'error'
							});
						}
					}
				})
			}
		}, 
		onLoad() {
			
		},
		created() {
			
		},
		onLoad() {
			uni.getStorage({
				key:'userName',
				success: (res) => {
					this.userName = res.data
				}
			})
		}
	}
</script>

<style lang="scss">
	.content {
		width: 100%;
		padding: 20rpx;
		box-sizing: border-box;
		.input_pw {
			width: 100%;
			border: 1px solid #aaaaaa;
			height: 80rpx;
			border-radius: 10rpx;
			margin-bottom: 30rpx;
			padding-left: 20rpx;
			box-sizing: border-box;
			&:hover{
				border: 2px solid #ffaa00;
				padding-left: 18rpx;
			}
		}
		.old_pw  {
			
		}
		.new_pw {
		}
		.btn {
			width: 200rpx;
			height: 80rpx;
			background-color: #007AFF;
			color: white;
			text-align: center;
			line-height: 80rpx;
			border-radius: 10rpx;
			margin-left: 220rpx;
		}
	}
</style>
