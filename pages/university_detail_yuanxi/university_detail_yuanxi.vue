<template>
	<view class="content">
		<view class="yuanxi_list"  v-if="!nothing_show" v-for="(item,index) in list_arr" :key=index>
			<view class="yuanxi_list_title">
				{{item.college}}
			</view>
			<view class="yuanxi_list_border">
				
			</view>
			<view class="yuanxi_list_detail">
				<view class="detail_list" v-for="(item1,index1) in item.department" :key=index1>
					{{item1}}
				</view>
			</view>
		</view>
		<view class="nothing" v-if="nothing_show">
			<image class="nothing_icon" src="../../static/nothing.webp" mode=""></image>
		</view>
	</view>
</template>

<script>
	export default {
		data(){
			return {
				university_name:'',
				nothing_show:false,
				list_arr:[]//院系列表
			}
		},
		methods:{
			getYuanxiList() {
				uni.request({
					url:'http://47.107.23.59/gaokao-2.0/university/department',
					data:{
						universityName:this.university_name
					},
					success:(res)=> {
						console.log(res.data.data);
						this.list_arr = res.data.data
						if(this.list_arr.length==0){
							this.nothing_show = true
						}
						else{
							this.nothing_show = false
						}
					}
				})
			}
		},
		onLoad(option){
			this.university_name = option.name;
			console.log(this.university_name);
			this.getYuanxiList();
			
		}
	}
</script>

<style lang="scss">
	.content {
		width: 100%;
		background-color: #f0f0f0;
		.yuanxi_list {
			width: 100%;
			background-color: rgb(248,248,248);
			padding: 0 30rpx 10rpx;
			box-sizing: border-box;
			margin-bottom: 10rpx;
			.yuanxi_list_title {
				font-weight: bold;
				padding: 20rpx 0;
			}
			.yuanxi_list_border{
				height: 1rpx;
				width: 100%;
				transform: scaleY(0.3);
				background-color: #959595;
			}
			.yuanxi_list_detail {
				.detail_list {
					padding: 10rpx 0;
					color: #626262;
				}
			}
		}
		.nothing {
			width: 100%;
			height: 1246rpx;
			.nothing_icon {
				width: 100%;
				height: 100%;
			}
		}
	}
</style>
