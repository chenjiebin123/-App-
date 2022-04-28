<template>
	<view class="content">
		<view class="major_introduce">
			<view class="introduce_tab">
				<view v-for="(item,index) in tab_list" :key="item.id" :class="tab_num==item.id?'active tab_text':'tab_text'" @click="tab_click(item.id)">
					{{item.title}}
				</view>
			</view>
			<view class="loading" v-show="is_loading">
				<image class="loading_icon"  src="../../static/like_loading.gif" mode=""></image>
				<view class="loading_text">
					努力加载中...
				</view>
			</view>
			<view class="introduce_jianjie_body" v-if="tab_num==1?true:false" v-show="!is_loading">
				<view class="university_box_list" v-for="(item,index) in universityListArr" :key=item.id>
					<view class="university_list" @click="to_university_detail(item)">
						<view class="list_left">
							<image class="badge" :src="`${item.badge}`" mode=""></image>
						</view>
						<view class="list_right">
							<view class="university_top">
								<view class="university_name">
									{{item.name}}
								</view>
								<view class="university_province">
									<image class="university_province_icon" src="../../static/input_delete.png" mode="" @click.stop="delete_like_university(item.name)"></image>
								</view>
							</view>
							<view class="university_buttom">
								<view class="university_xx">
									<view class="university_nature uni_xx">
										{{item.nature}}
									</view>
									<view class="university_type uni_xx">
										{{item.type}}
									</view>
									<view v-if="item.is985==1?true:false" class="university_is985 uni_xx">
										985
									</view>
									<view v-if="item.is211==1?true:false" class="university_is211 uni_xx">
										211
									</view>
									<view v-if="item.isDoubleTop==1?true:false" class="university_isDoubleTop uni_xx">
										双一流
									</view>
								</view>
							</view>
						</view>
					</view>
					<view class="border_buttom"></view>
				
				</view>
			</view>

			<view class="introduce_leisizhuanye_body" v-if="tab_num==2?true:false" v-show="!is_loading"> 
				<view class="major_box_list" @click="to_major_detail(item)" v-for="(item,index) in majorListArr" :key=item.id>
					<view class="major_list">
						<view class="major_list_left">
							{{item.majorName}}
						</view>
						<view class="major_list_right">
							<view :class="item.level=='本科'?'major_level':'major_level active'">
								{{item.level.slice(0,1)}}
							</view>
							<image class="list_right_icon" src="../../static/input_delete.png" mode="" @click.stop="delete_like_major(item.majorName)"></image>
						</view>
					</view>
					<view class="border_buttom"></view>
				</view>
				
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tab_num:1,
				universityListArr:[],
				majorListArr:[],
				is_loading:true,
				tab_list:[
					{
						id:1,
						title:"院校"
					},
					{
						id:2,
						title:"专业"
					}
				]
			}
		},
		methods:{
			to_major_detail(item) {
				// console.log(item)
				uni.navigateTo({
					url:`../major_detail/major_detail?name=${item.majorName}`
				})
			},
			tab_click(index){
				this.tab_num = index;
				if(index == 2) {
					this.getMajorLits(this.userName)
					if(this.majorListArr.length==0){
						this.is_loading=true
						
					}
				}
			},
			to_university_detail(item) {
				uni.navigateTo({
					url: `../university_detail/university_detail?name=${item.name}`,
				})
			},
			//取消收藏院校---请求
			delete_like_university(university_name){
				this.is_loading = true;
				uni.request({
					url:`http://localhost:3000/api/delectUniversityLike`,
					data:{
						userName:this.userName,
						university_name
					},
					success: (res) => {
						// console.log(res)
						this.getUniversityLits(this.userName);
					}
				})
			},
			//取消收藏专业---请求
			delete_like_major(majorName){
				this.is_loading = true;
				uni.request({
					url:`http://localhost:3000/api/delectMajorLike`,
					data:{
						userName:this.userName,
						majorName
					},
					success: (res) => {
						// console.log(res)
						this.getMajorLits(this.userName);
					}
				})
			},
			getUniversityLits(userName){
				uni.request({
					url:`http://localhost:3000/api/UniversityLikeList`,
					data:{
						userName
					},
					success: (res) => {
						this.universityListArr = res.data.result.data;
						setTimeout(()=>{
							this.is_loading = false;
						},700)
					}
				})
			},
			getMajorLits(userName){
				// return;
				uni.request({
					url:`http://localhost:3000/api/MajorLikeList`,
					data:{
						userName
					},
					success: (res) => {
						this.majorListArr = res.data.result.data;
						setTimeout(()=>{
							this.is_loading = false;
						},700)
					}
				})
			}
		},
		created() {
			
		},
		onLoad(option) {
			uni.getStorage({
				key:'userName',
				success: (res) => {
					this.userName = res.data
				}
			})
			this.getUniversityLits(this.userName);
			
		}
	}
</script>

<style lang="scss">
	.content {
		height: 100%;
		.major_introduce {
			background-color: rgb(248, 248, 248);
			height: 100%;
			.introduce_tab {
				height: 70rpx;
				background-color: rgb(248, 248, 248);
				// background-color: blue;
				display: flex;
				justify-content: space-around;
				align-items: center;
				font-size: 28rpx;
				color: #838383;
				.tab_text {
					height: 50rpx;
					
					box-sizing: border-box;
					// background-color: red;
				}
				.active {
					border-bottom: 4rpx solid #007AFF;
				}
			}
			.loading {
				height: 100%;
				width: 100%;
				background-color: white;
				display: flex;
				flex-direction: column;
				justify-content: center;
				align-items: center;
				padding-top: 200rpx;
				.loading_icon {
					height: 300rpx;
					width: 300rpx;
				}
				.loading_text {
					
					width: 100%;
					height: 40rpx;
					text-align: center;
					line-height: 40rpx;
					font-size: 38rpx;
					font-weight: bold;
				}
			}
			.introduce_jianjie_body {
				height: 100%;
				padding: 20rpx 30rpx;
				box-sizing: border-box;
				.university_list {
					display: flex;
					width: 100%;
					height: 120rpx;
					margin: 7rpx 0;
				
					// background-color: blue;
					// border-bottom: 1px #767676 solid;
					.list_left {
						width: 20%;
						height: 120rpx;
						// background-color: red;
						display: flex;
						align-items: center;
						justify-content: center;
				
						// box-sizing: border-box;
						.badge {
							height: 100rpx;
							width: 100rpx;
						}
					}
				
					.list_right {
						// background-color: yellow;
						width: 80%;
						padding: 10rpx 20rpx;
						box-sizing: border-box;
				
						.university_top {
							display: flex;
							justify-content: space-between;
				
							.university_name {
								font-size: 30rpx;
							}
				
							.university_province {
								// font-size: 26rpx;
								// color: #767676;
								// margin-top: 50rpx;
								position: relative;
								.university_province_icon {
									position: absolute;
									top: 35rpx;
									right: -10rpx;
									height: 40rpx;
									width: 40rpx;
								}
							}
						}
				
						.university_buttom {
							// background-color: red;
							height: 50rpx;
							margin-top: 20rpx;
				
							.university_xx {
								display: flex;
				
								.university_nature {}
				
								.university_type {}
				
								.university_isDoubleTop {}
				
								.university_is985 {}
				
								.university_is211 {}
				
								.uni_xx {
									font-size: 22rpx;
									color: #767676;
									margin-right: 20rpx;
								}
							}
						}
					}
				}
				// background-color: #f0f0f0;
				.border_buttom {
					height: 1rpx;
					width: 100%;
					background-color: #999999;
					transform: scaleY(0.2);
				}
			}
			.introduce_leisizhuanye_body {
				height: 100%;
				padding: 20rpx 30rpx;
				box-sizing: border-box;
				// background-color: #f0f0f0;
				.major_box_list {
					.major_list{
						display: flex;
						width: 100%;
						padding: 20rpx 10rpx;
						margin: 7rpx 0;
						display: flex;
						justify-content: space-between;
						.major_list_left{
							font-size: 32rpx;
						}
						.major_list_right{
							width: 90rpx;
							justify-content: space-between;
							display: flex;
							align-items: center;
							.major_level{
								height: 32rpx;
								width: 32rpx;
								text-align: center;
								line-height: 32rpx;
								border-radius: 50%;
								font-size: 22rpx;
								color: white;
								background-color: #00aeff;
							}
							.active {
								background-color: #ffaa00;
							}
							.list_right_icon{
								height: 40rpx;
								width: 40rpx;
							}
						}
					}
					.border_buttom {
						height: 1rpx;
						width: 100%;
						background-color: #999999;
						transform: scaleY(0.2);
					}
				}
				
			}
		}
			
	}
</style>
