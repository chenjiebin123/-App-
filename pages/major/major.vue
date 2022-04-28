<template>
	<view class="content">
		<view class="top">
			<view class="search">
				<input class="search_input" v-model="cont" @keydown.enter="search" placeholder="搜索专业"/>

				<image class="search_img" src="../../static/search.png" mode=""></image>
			</view>
			<view class="select" @click="select_detail_show">
				<view class="nature">
					<view class="name">
						{{cenci_click_name}}
					</view>
					<image src="../../static/select_img.png" mode=""></image>
				</view>
				<view class="type">
					<view class="name">
						{{leibie_click_name}}
					</view>
					<image src="../../static/select_img.png" mode=""></image>
				</view>
				<view class="province">
					<view class="name">
						{{fenlei_click_name}}
					</view>
					<image src="../../static/select_img.png" mode=""></image>
				</view>
			</view>
		</view>
		<view class="buttom">
			<view class="major_list" @click="to_major_detail(item)" v-for="(item,index) in MajorArr" :key=item.id>
				<view class="major_list_top">
					<view class="major_name">
						{{item.majorName}}-{{item.majorCode}}
					</view>
					<view class="major_type">
						专业类别：{{item.level}}-{{item.majorName}}
					</view>
				</view>
				<view class="major_list_buttom">
					<view class="year">
						修业年限：{{item.schoolYear}}年
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
		<view class="loading" v-if="is_loading">
			正在加载中
			<image v-show="is_loading" class="loading_icon" src="../../static/loading.png" mode=""></image>
		</view>
		
		<view class="mask" @click.self="select_detail_hidden" v-show=mask_show>
			<view class="mask_body">
				<view class="mask_title">
					条件筛选
				</view>
				<view class="quanbu_box">
					
					<view class="btn  nature">
						<view class="title">
							专业层次
						</view>
						<view class="select">
							<view :class="cenci_id==item.id?'list active':'list'" @click="cenci_click(item)" v-for="(item,index) in cenciArr" :key=item.id>
								{{item.level}}
							</view>
						</view>
					</view>
					<view class="btn type">
						<view class="title">
							专业类别
						</view>
						<view class="select leibie">
							<view :class="leibie_id==item.id?'list active':'list'" @click="leibie_click(item)" v-for="(item,index) in leibieArr" :key=item.id>
								{{item.subject}}
							</view>
						</view>
					</view>
					<view class="btn province">
						<view class="title">
							细致分类
						</view>
						<view class="select fenlei">
							<view :class="fenlei_id==item.id?'list active':'list'" @click="fenlei_click(item)" v-for="(item,index) in fenleiArr" :key=item.id>
								{{item.majorType}}
							</view>
						</view>
					</view>
					
				</view>
				<view class="button_box">
					<view class="close">
						<image @click="select_detail_hidden" class="close_icon" src="../../static/close.png" mode="">
						</image>
					</view>
					<view class="button" @click="reset_select">重置</view>
					<view @click="suer_click" class="button">确定</view>
				</view>
			</view>

		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				cont:'',//输入框的数据
				mask_show: false,
				is_loading: false, //是否加载
				select_scroll:false,
				page: 10, //专业数据条数
				MajorArr:[],//获取的专业数据数组
				cenciArr:[],//层次数组
				cenci_id:0,//选择的id(cenci_id)
				cenci_click_id:0,//选择的id(cenci_id)
				cenci_name:'',//层次名称
				cenci_click_name:'专业层次',//点击后层次名称
				leibieArr:[],//类别数组
				leibie_id:0,//选择的id(leibie_id)
				leibie_click_id:0,//选择的id(leibie_id)
				leibie_name:'',//类别名称
				leibie_click_name:'专业类别',//点击后的类别名称
				fenleiArr:[],//数组
				fenlei_id:0,//选择的id(fenlei_id)
				fenlei_click_id:0,//选择后的id(fenlei_id)
				fenlei_name:'',//分类名称
				fenlei_click_name:'细致分类',//点击后的分类名称
			}
		},
		onLoad() {

		},
		methods: {
			reset_select(){
				this.cenci_id=0
				this.leibie_id=0
				this.fenlei_id=0
			},
			select_detail_show() {
				// console.log(1111)
				this.getcenci();
				this.mask_show = true;
				this.cenci_name =   this.cenci_click_name;
				this.leibie_name =  this.leibie_click_name;
				this.fenlei_name =    this.fenlei_click_name;
				
			},
			select_detail_hidden() {
				this.mask_show = false;
				this.cenci_id = this.cenci_click_id;
				this.leibie_id = this.leibie_click_id;
				this.fenlei_id = this.fenlei_click_id;
			},
			suer_click() {
				this.mask_show = false;
				this.select_scroll=true;
				this.page = 10
				this.cenci_click_name = this.cenci_name;
				this.leibie_click_name = this.leibie_name;
				this.fenlei_click_name = this.fenlei_name;
				this.cenci_click_id = this.cenci_id;
				this.leibie_click_id = this.leibie_id;
				this.fenlei_click_id = this.fenlei_id;
				// console.log({cenci:this.cenci_click_name,leibie:this.leibie_click_name,fenlei:this.fenlei_click_name});
				this.getSelectMajor({cenci:this.cenci_click_name,leibie:this.leibie_click_name,fenlei:this.fenlei_click_name});
					
			},
			cenci_click(item) {
				this.cenci_id=item.id
				this.cenci_name=item.level
				this.fenlei_id=0;
				this.leibie_id=0;
				// this.fenlei_click_name=''
				let level = item.level
				this.getleibie(level);
				
				// console.log(level)
			},
			leibie_click(item) {
				// console.log(item)
				this.leibie_id=item.id
				this.leibie_name = item.subject
				this.fenlei_id=0;
				
				let type = item.subject
				this.getfenlei(type);
				
			},
			fenlei_click(item) {
				this.fenlei_id=item.id
				this.fenlei_name = item.majorType
				// let level = item.level
				// this.getleibie(level);
			},
			to_major_detail(item) {
				// console.log(item)
				uni.navigateTo({
					url:`../major_detail/major_detail?name=${item.majorName}`
				})
			},
			search() {
				uni.request({
					url:`http://localhost:3000/api/searchMajor`,
					data:{
						name:this.cont
					},
					method:'POST',
					success: (res) => {
						this.MajorArr = res.data.result.data
					}
				})
				// console.log(this.cont)
			},
			getcenci(){
				this.cenci_click_name=this.cenci_click_name=="专业层次"?'本科':this.cenci_click_name;
				this.leibie_click_name=this.leibie_click_name=="专业类别"?'哲学':this.leibie_click_name;
				this.fenlei_click_name=this.fenlei_click_name=="细致分类"?'哲学类':this.fenlei_click_name;
				// console.log(this.cenci_click_name,this.leibie_click_name,this.fenlei_click_name)
				uni.request({
					url:`http://localhost:3000/api/major_cenci`,
					success: (res) => {
						this.cenciArr =res.data.result.data;
						this.cenciArr.forEach((item,index)=>{
							item.id = index
							
						})
						this.getleibie(this.cenci_name);
						this.getfenlei(this.leibie_name);
					}
				})
			},
			getleibie(level){
				uni.request({
					url:`http://localhost:3000/api/major_leibie`,
					data:{
						name:level
					},
					success: (res) => {
						// console.log(res.data.result.data)
						this.leibieArr =res.data.result.data;
						this.leibieArr.forEach((item,index)=>{
							item.id = index
						})
					}
				})
			},
			getfenlei(type){
				
				// console.log(type)
				// return;
				uni.request({
					url:`http://localhost:3000/api/major_fenlei`,
					data:{
						name:type
					},
					success: (res) => {
						// console.log(res.data.result.data)
						this.fenleiArr =res.data.result.data;
						this.fenleiArr.forEach((item,index)=>{
							item.id = index
						})
					}
				})
			},
			getSelectMajor(item){
				let {cenci,leibie,fenlei} = item;
				uni.request({
					url:`http://localhost:3000/api/selectMajor`,
					data:{
						cenci:cenci,
						leibie:leibie,
						fenlei:fenlei,
						size:this.page
					},
					method:'POST',
					success: (res) => {
						this.MajorArr =res.data.result.data
						// console.log(this.MajorArr)
					}
				})
			},
			getMajorList() {
				// console.log(this.page)
				uni.request({
					url: `http://localhost:3000/api/getMajorList`,
					data: {
						size: this.page
					},
					success: (res) => {
						// console.log( res.data.result.data)
						this.MajorArr = res.data.result.data
						// console.log(this.MajorArr)
					}
				})
			}
		},
		onLoad() {
			this.getMajorList();
			
			
		},
		onReachBottom() {
			this.is_loading = true;
			// return;
			this.page += 10;
			if(this.select_scroll) {
				setTimeout(() => {
					this.getSelectMajor({cenci:this.cenci_click_name,leibie:this.leibie_click_name,fenlei:this.fenlei_click_name})
					this.is_loading = false;
				}, 1000)
			}else {
				setTimeout(() => {
					this.getMajorList();
					this.is_loading = false;
				}, 1000)
			}
			
			// console.log("onReachBottom");
		}
	}
</script>

<style lang="scss">
	.content {
		width: 100%;
		// background-color: f0f0f0;
		// padding: 0 25rpx 0 25rpx;
		// box-sizing: border-box;
		position: relative;
		background-color: white;
	}

	.content .search {
		width: 100%;
		padding: 10rpx 25rpx;
		box-sizing: border-box;
		background-color: white;
		height: 90rpx;
		position: relative;
	}

	.content .search .search_input {
		height: 70rpx;
		background-color: #f0f0f0;
		padding-left: 90rpx;
		line-height: 70rpx;
		color: #999999;
		border-radius: 10rpx;
	}

	.content .search .search_img {
		position: absolute;
		left: 40rpx;
		top: 20rpx;
		height: 50rpx;
		width: 50rpx;
	}

	.content .select {
		width: 100%;
		height: 90rpx;
		padding: 0 25rpx 0 25rpx;
		box-sizing: border-box;
		margin-top: 1rpx;
		margin-bottom: 2rpx;
		display: flex;
		justify-content: space-around;
		background-color: white;
	}

	.content .select .nature,
	.content .select .type,
	.content .select .province {
		display: flex;
		align-items: center;
		justify-content: center;
		color: #515151;
		font-size: 30rpx;
		/* background-color: #007AFF; */
	}

	.content .select .nature image,
	.content .select .type image,
	.content .select .province image {
		width: 20rpx;
		height: 20rpx;
		margin-left: 20rpx;
	}
	.content .loading {
		width: 100%;
		text-align: center;
		display: flex;
		justify-content: center;
		align-items: center;
		height: 80rpx;
	
		.loading_icon {
			margin-left: 20rpx;
			height: 50rpx;
			width: 50rpx;
			animation-name: loading;
			animation-duration: 1s;
			animation-iteration-count: infinite;
			animation-timing-function: linear;
	
			@keyframes loading {
				0% {
					transform: rotate(0deg);
				}
	
				100% {
					transform: rotate(360deg);
				}
			}
		}
	}
	.content .buttom {
		width: 100%;
		.major_list {
			width: 100%;
			// background-color: red;
			padding: 25rpx 25rpx 10rpx 25rpx;
			box-sizing: border-box;
			margin-bottom: 13rpx;
			background-color: white;
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

		.content .mask {
			width: 100%;
			height: 1245rpx;
			
			position: absolute;
			top: 0;
			left: 0;
			z-index: 99;
			background-color: rgba(0, 0, 0, .4);

			.mask_body {
				width: 80%;
				height: 1150rpx;
				// overflow: scroll;
				background-color: white;
				top: 0;
				right: 0rpx;
				z-index: 100;
				position: absolute;

				.mask_title {
					height: 60rpx;
					width: 100%;
					padding-left: 30rpx;
					box-sizing: border-box;
					line-height: 60rpx;
					background-color: #f0f0f0;
				}

				.quanbu_box {
					overflow: scroll;
					height: 1150-60rpx;
					padding-bottom: 100rpx;
					box-sizing: border-box;
					.btn {
						// margin-top: 20rpx;
						padding-left: 30rpx;
					
						.title {
							font-size: 30rpx;
							margin-bottom: 10rpx;
						}
					
						.select {
							// margin-top: 5rpx;
							padding:0 0 15rpx;
							width: 100%;
							height: auto;
							display: flex;
							justify-content: flex-start;
							flex-wrap: wrap;
					
							.list {
								height: 50rpx;
								width: 120rpx;
								margin: 0rpx 20rpx 15rpx 0;
								font-size: 20rpx;
								border-radius: 10rpx;
								// text-align: center;
								padding-left: 10rpx;
								line-height: 50rpx;
								background-color: #ebebeb;
								box-sizing: border-box;
							}
					
							.list.active {
								color: red;
								background-color: #ffd9e4;
								border: 1rpx red solid;
								
							}
						}
						.leibie,.fenlei {
							.list {
								width: 45%;
							}
						}
					}
					
				}
				
				.button_box {
					position: absolute;
					bottom: 0;
					left: 0;
					display: flex;
					width: 100%;
					background-color: white;
					// justify-content: space-around;
					.button {
						width: 45%;
						height: 92rpx;
						text-align: center;
						line-height: 90rpx;

						&:nth-child(3) {
							background-color: red;
							color: white;
						}
					}

					.close {
						width: 10%;
						height: 92rpx;
						line-height: 92rpx;
						text-align: center;
						padding-left: 10rpx;
						box-sizing: border-box;

						.close_icon {
							width: 30rpx;
							height: 30rpx;
						}
					}
				}
			}
		}
</style>
