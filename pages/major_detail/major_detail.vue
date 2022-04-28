<template>
	<view class="content">
		<view class="major_survey">
			<view class="survey_top">
				<view class="major_title">
					专业概况
				</view>
				<view class="add_like" @click="major_like_click">
					{{is_like==true?'取消添加':'添加意向专业'}}
				</view>
			</view>
		</view>
		
		<view class="major_detail">
			<view class="major_level major_box">
				<view class="major_box_top">
					<image class="detail_icon" src="../../static/level.png" mode=""></image>
					<view class="detail_level_title detail_level_text">
						学历学制
					</view>
				</view>
				<view class="level detail_text">
					{{major_detail.level}}
				</view>
			</view>
			<view class="major_boyGirlRate major_box">
				<view class="major_box_top">
					<image class="detail_icon" src="../../static/boyGirlRate.png" mode=""></image>
					<view class="detail_level_boygirl detail_level_text">
						男女比例
					</view>
				</view>
				<view class="boyGirlRate detail_text">
					{{major_detail.boyGirlRate}}
				</view>
			</view>
			<view class="major_lastYearEmployedRate major_box">
				<view class="major_box_top">
					<image class="detail_icon" src="../../static/Rate.png" mode=""></image>
					<view class="detail_level_jiuye detail_level_text">
						就业率
					</view>
				</view>
				<view class="lastYearEmployedRate detail_text">
					{{major_detail.lastYearEmployedRate}}
				</view>
			</view>
			<view class="major_avgSalary major_box">
				<view class="major_box_top">
					<image class="detail_icon" src="../../static/avgSalary.png" mode=""></image>
					<view class="detail_level_gongzi detail_level_text">
						平均薪资
					</view>
				</view>
				<view class="avgSalary detail_text">
					{{major_detail.avgSalary}}
				</view>
			</view>
		</view>
	
		<view class="major_introduce">
			<view class="introduce_tab">
				<view v-for="(item,index) in tab_list" :key="item.id" :class="tab_num==item.id?'active tab_text':'tab_text'" @click="tab_click(item.id)">
					{{item.title}}
				</view>
			</view>
			<view class="introduce_jianjie_body" v-if="tab_num==1?true:false">
				<view class="summary introduce_jianjie_list">
					<view class="major_title">
						专业介绍
					</view>
					<view class="major_content">
						{{major_detail.summary}}
					</view>
				</view>
				<view class="educationGoal introduce_jianjie_list">
					<view class="major_title">
						培养目标
					</view>
					<view class="major_content">
						{{major_detail.educationGoal}}
					</view>
				</view>
				<view class="educationRequire introduce_jianjie_list">
					<view class="major_title">
						培养要求
					</view>
					<view class="major_content">
						{{major_detail.educationRequire}}
					</view>
				</view>
				<view class="famousScholar introduce_jianjie_list">
					<view class="major_title">
						名人学者
					</view>
					<view class="major_content">
						{{major_detail.famousScholar}}
					</view>
				</view>
				<view class="mainCourse introduce_jianjie_list">
					<view class="major_title">
						主干课程
					</view>
					<view class="major_content">
						{{major_detail.mainCourse}}
					</view>
				</view>
				<view class="courseRequire introduce_jianjie_list">
					<view class="major_title">
						学科要求
					</view>
					<view class="major_content">
						{{major_detail.courseRequire}}
					</view>
				</view>
				<view class="knowledgeAbility introduce_jianjie_list">
					<view class="major_title">
						必备能力
					</view>
					<view class="major_content">
						{{major_detail.knowledgeAbility}}
					</view>
				</view>
			</view>
		<!-- 	<view class="introduce_jiuye_body" v-if="tab_num==2?true:false">
				b
			</view> -->
			<view class="introduce_leisizhuanye_body" v-if="tab_num==2?true:false"> 
				<view class="major_list" @click="to_major_detail(item)" v-for="(item,index) in similarMajorArr" :key=item.id>
					<view class="major_list_top">
						<view class="major_name">
							{{item.majorName}}-{{item.majorCode}}
						</view>
						<view class="major_type">
							专业类别：{{item.level}}-{{item.subject}}-{{item.majorType}}
						</view>
					</view>
					<view class="major_list_buttom">
						<view class="year">
							修业年限：{{item.schoolYear}}
						</view>
						<view class="boyGirlRate">
							男女比例：{{item.boyGirlRate}}
						</view>
						<view class="lastYearEmployedRate">
							就业率：{{item.lastYearEmployedRate}}
						</view>
						<view class="avgSalary">
							平均工资：￥{{item.avgSalary}}
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userName:'',//用户名
				is_like:false,//是否收藏
				isjianjie:true,
				isjiuye:false,
				isleisizhuanye:false,
				majorName:'',//专业名称
				major_detail:{},//专业详细信息
				similarMajorArr:[],//详细专业——数组
				tab_num:1,
				tab_list:[
					{
						id:1,
						title:"简介"
					},
					{
						id:2,
						title:"类似专业"
					}
				]
			}
		},
		methods:{
			tab_click(index){
				// console.log(index);
				this.tab_num = index;
			},
			to_major_detail(item) {
				
				uni.navigateTo({
					url:`../major_detail/major_detail?name=${item.majorName}`
				})
			},
			getMajor_detail() {
				uni.request({
					url:`http://localhost:3000/api/getMajorDetail`,
					data:{
						name:this.majorName
					},
					success: (res) => {
						this.major_detail = res.data.result.data[0]
						// console.log(this.major_detail)
						this.getsimilarMajorList();
					}
				})
			},
			getsimilarMajorList(){
				// console.log(this.major_detail.majorType)
				uni.request({
					url:`http://localhost:3000/api/getsimilarMajorList`,
					data:{
						name:this.major_detail.majorType
					},
					success: (res) => {
						// console.log( res.data.result.data)
						this.similarMajorArr = res.data.result.data;
						this.similarMajorArr.forEach((item,index)=>{
							// console.log(index);
							if(item.majorName == this.majorName) {
								this.similarMajorArr.splice(index,1)
							}
						})
						// console.log(this.similarMajorArr)
					}
				})
			},
			//收藏专业---请求
			add_like_major(userName,majorName){
				// console.log(userName,majorName);
				uni.request({
					url:`http://localhost:3000/api/addMajorLike`,
					data:{
						userName,
						majorName
					},
					success: (res) => {
						this.is_like=true
						uni.showToast({
							title: '添加成功',
							duration: 1000,
						});
					}
				})
			},
			//取消收藏院校---请求
			delete_like_major(userName,majorName){
				uni.request({
					url:`http://localhost:3000/api/delectMajorLike`,
					data:{
						userName,
						majorName
					},
					success: (res) => {
						// console.log(res)
						this.is_like=false;
						uni.showToast({
							title: '取消成功',
							duration: 1000,
						});
					}
				})
			},
			//查看是否收藏该专业
			search_like_major(userName,majorName){
				uni.request({
					url:`http://localhost:3000/api/searchMajorLike`,
					data:{
						userName,
						majorName
					},
					success: (res) => {
						if(res.data.result.is_like==true) {
							this.is_like = true
						}
					}
				})
			},
			//点击收藏按钮
			major_like_click(){
				// return;
				uni.getStorage({
					key:'userName',
					success: (res) => {
						this.userName = res.data
					}
				})
				uni.getStorage({
					key:'token',
					success: (res) => {
						// console.log(res.data)
						if(this.is_like==false){
							this.add_like_major(this.userName,this.majorName);
						}else {
							this.delete_like_major(this.userName,this.majorName);
						}
						
						
					},
					fail: (res) => {
						// console.log('err')
						uni.navigateTo({
							url:'../login/login'
						})
					}
				})
			},
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
			// console.log(option.name)
			this.majorName = option.name
			uni.setNavigationBarTitle({
				title: this.majorName
			})
			this.getMajor_detail();
			this.search_like_major(this.userName,this.majorName);
		}
	}
</script>

<style lang="scss">
	.content {
		width: 100%;
		background-color: #f0f0f0;

		.major_survey {
			width: 100%;
			background-color: rgb(248, 248, 248);
			// background-color: red;
			padding: 20rpx 30rpx;
			box-sizing: border-box;

			.survey_top {
				display: flex;
				justify-content: space-between;
				align-items: center;

				.major_title {
					font-weight: bold;
				}

				.add_like {
					color: red;
					font-size: 20rpx;
					height: 45rpx;
					width: 170rpx;
					text-align: center;
					line-height: 45rpx;
					border: 1px solid red;
					border-radius: 30rpx;
				}
			}
		}

		.major_detail {
			display: flex;
			flex-wrap: wrap;
			justify-content: space-between;
			padding: 20rpx 30rpx;
			box-sizing: border-box;
			background-color: rgb(248, 248, 248);

			.major_box {
				width: 48%;
				height: 150rpx;
				background-color: #f0f0f0;
				border-radius: 20rpx;
				padding: 15rpx 20rpx;
				margin-bottom: 15rpx;
				box-sizing: border-box;
				.major_box_top {
					display: flex;
					align-items: center;
					margin-bottom: 20rpx;
					.detail_icon {
						height: 50rpx;
						width: 50rpx;
					}
					
					.detail_level_text {
						font-size: 24rpx;
						color: #7f7f7f;
						margin-left: 30rpx;
					}
				}
				.detail_text {
					font-weight: bold;
				}
			}
		}
	
		.major_introduce {
			margin-top: 15rpx;
			background-color: rgb(248, 248, 248);
			
			.introduce_tab {
				height: 70rpx;
				background-color: rgb(248, 248, 248);
				// background-color: blue;
				display: flex;
				justify-content: space-around;
				align-items: center;
				font-size: 28rpx;
				.tab_text {
					height: 50rpx;
					
					box-sizing: border-box;
					// background-color: red;
				}
				.active {
					color: red;
					border-bottom: 4rpx solid red;
				}
			}
			.introduce_jianjie_body {
				padding: 20rpx 30rpx;
				box-sizing: border-box;
				.introduce_jianjie_list {
					padding-top: 30rpx;
					// background-color: red;
					.major_title {
						font-weight: bold;
						font-size: 32rpx;
					}
					.major_content {
						margin-top: 10rpx;
						margin-bottom: 10rpx;
						font-size: 26rpx;
						color: #757575;
						// color: red;
					}
				}
			}
			.introduce_leisizhuanye_body {
				background-color: #f0f0f0;
				
				.major_list {
					width: 100%;
					// background-color: red;
					padding: 25rpx 25rpx 10rpx 25rpx;
					box-sizing: border-box;
					margin-bottom: 13rpx;
					background-color: rgb(248, 248, 248);
					margin-bottom: 15rpx;
					.major_list_top {
						// margin-bottom: 10rpx;
						.major_name {
							margin-bottom: 10rpx;
						}
						.major_type {
							font-size: 20rpx;
							color: #8b8b8b;
						}
					}
					.major_list_buttom {
						font-size: 20rpx;
						width: 100%;
						display: flex;
						flex-wrap: wrap;
						.year {
							width: 50%;
							margin-top: 15rpx;
						}
						.boyGirlRate {
							width: 50%;
							margin-top: 15rpx;
						}
						.lastYearEmployedRate {
							width: 50%;
							margin-top: 15rpx;
						}
						.avgSalary {
							width: 50%;
							margin-top: 15rpx;
						}
					}
				}
			}
		}
	
	}
</style>
