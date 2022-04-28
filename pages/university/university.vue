<template>
	<view class="content">
		<view class="top">
			<view class="search" @click="to_university_search">
				<view class="search_input">
					搜索院校
				</view>
				<image class="search_img" src="../../static/search.png" mode=""></image>
			</view>
			<view class="select" @click="select_detail_show">
				<view class="nature">
					<view class="name">
						{{cenci_click_name=='所有'?'层次':cenci_click_name}}
					</view>
					<image src="../../static/select_img.png" mode=""></image>
				</view>
				<view class="type">
					<view class="name">
						{{leixin_click_name=='所有'?'类型':leixin_click_name}}
					</view>
					<image src="../../static/select_img.png" mode=""></image>
				</view>
				<view class="province">
					<view class="name">
						{{diqu_click_name=='所有'?'地区':diqu_click_name}}
					</view>
					<image src="../../static/select_img.png" mode=""></image>
				</view>
			</view>
		</view>
		<view class="buttom">
			<view class="university_box_list" v-for="(item,index) in universityArr" :key=item.id>
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
								{{item.province}}
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
		<view class="loading" v-if="is_loading">
			正在加载中
			<image v-show="is_loading" class="loading_icon" src="../../static/loading.png" mode=""></image>
		</view>
		<view class="is_null" v-show="is_null">
			<image class="is_null_icon" src="../../static/nothing.webp" mode=""></image>
		</view>
		<view class="mask" @click.self="select_detail_hidden" v-show=mask_show>
			<view class="mask_body">
				<view class="mask_title">
					条件筛选
				</view>
				<view class="btn  nature">
					<view class="title">
						层次
					</view>
					<view class="select">
						<view :class="cenci_id==item.id?'list active':'list'" @click="cenci(item)"
							v-for="(item,index) in cenciList" :key=item.id>
							{{item.name}}
						</view>
					</view>
				</view>
				<view class="btn type">
					<view class="title">
						类型
					</view>
					<view class="select">
						<view :class="leixin_id==item.id?'list active':'list'" @click="leixin(item)"
							v-for="(item,index) in leixinList" :key=item.id>
							{{item.name}}
						</view>
					</view>
				</view>
				<view class="btn province">
					<view class="title">
						地区
					</view>
					<view class="select">
						<view :class="diqu_id==item.id?'list active':'list'" @click="diqu(item)"
							v-for="(item,index) in diquList" :key=item.id>
							{{item.name}}
						</view>
					</view>
				</view>
				<view class="button_box">
					<view class="close">
						<image @click="select_detail_hidden" class="close_icon" src="../../static/close.png" mode="">
						</image>
					</view>
					<view class="button" @click="reset_select">重置</view>
					<view @click="click_sure" class="button">确定</view>
				</view>
			</view>

		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				is_null:false,//列表是否为空
				is_loading: false, //是否加载
				mask_show: false,
				select_scroll:false,
				page: 10, //大学数据条数
				cenci_id: 0,
				cenci_name: '',
				leixin_id: 0,
				leixin_name: '',
				diqu_id: 0,
				cenci_click_id: 0,//点击时的id
				cenci_click_name: '层次',//点击时的名称
				leixin_click_id: 0,//点击时的id
				leixin_click_name: '类型',//点击时的名称
				diqu_click_id: 0,//点击时的id
				diqu_click_name: '地区',//点击时的名称
				universityArr: [],
				cenciList: [{
						id: 0,
						name: '所有'
					},
					{
						id: 1,
						name: '985'
					}, {
						id: 2,
						name: '211'
					},
					{
						id: 3,
						name: '双一流'
					},
					{
						id: 4,
						name: '公立'
					},
					{
						id: 5,
						name: '民办'
					},
				],
				leixinList: [{
						id: 0,
						name: '所有'
					},
					{
						id: 1,
						name: '财经'
					}, {
						id: 2,
						name: '语言'
					},
					{
						id: 3,
						name: '艺术'
					},
					{
						id: 4,
						name: '综合'
					},
					{
						id: 5,
						name: '政法'
					},
					{
						id: 6,
						name: '师范'
					},
					{
						id: 7,
						name: '医药'
					}, {
						id: 8,
						name: '理工'
					},
					{
						id: 9,
						name: '农林'
					},
					{
						id: 10,
						name: '军事'
					},
					{
						id: 11,
						name: '体育'
					},
					{
						id: 12,
						name: '民族'
					},
					{
						id: 13,
						name: '其他'
					},
				],
				diquList: [{
						id: 0,
						name: '所有'
					},
					{
						id: 1,
						name: '北京'
					},
					{
						id: 2,
						name: '天津'
					},
					{
						id: 3,
						name: '上海'
					},
					{
						id: 4,
						name: '重庆'
					},
					{
						id: 5,
						name: '台湾'
					},
					{
						id: 6,
						name: '香港'
					},
					{
						id: 7,
						name: '澳门'
					},
					{
						id: 8,
						name: '安徽'
					},
					{
						id: 9,
						name: '福建'
					},
					{
						id: 10,
						name: '广东'
					},
					{
						id: 11,
						name: '黑龙江'
					},
					{
						id: 12,
						name: '广西'
					},
					{
						id: 13,
						name: '甘肃'
					},
					{
						id: 14,
						name: '贵州'
					},
					{
						id: 15,
						name: '河北'
					},
					{
						id: 16,
						name: '湖北'
					},
					{
						id: 17,
						name: '河南'
					},
					{
						id: 18,
						name: '湖南'
					},
					{
						id: 19,
						name: '海南'
					},
					{
						id: 20,
						name: '吉林'
					},
					{
						id: 21,
						name: '江苏'
					},
					{
						id: 22,
						name: '江西'
					},
					{
						id: 23,
						name: '辽宁'
					},
					{
						id: 24,
						name: '宁夏'
					},
					{
						id: 25,
						name: '内蒙古'
					},
					{
						id: 26,
						name: '青海'
					},
					{
						id: 27,
						name: '四川'
					},
					{
						id: 28,
						name: '山东'
					},
					{
						id: 29,
						name: '山西'
					},
					{
						id: 30,
						name: '陕西'
					},
					{
						id: 31,
						name: '新疆'
					},
					{
						id: 32,
						name: '西藏'
					},
					{
						id: 33,
						name: '云南'
					},
					{
						id: 34,
						name: '浙江'
					}
				],
			}
		},
		onLoad() {

		},
		methods: {
			reset_select(){
				this.leixin_id = 0;
				this.cenci_id = 0;
				this.diqu_id = 0;
			},
			select_detail_show() {
				// console.log(1111)
				this.mask_show = true;
				this.cenci_name =   this.cenci_click_name;
				this.leixin_name =  this.leixin_click_name;
				this.diqu_name =    this.diqu_click_name;
			},
			select_detail_hidden() {
				this.mask_show = false;
				this.cenci_id = this.cenci_click_id;
				this.leixin_id = this.leixin_click_id;
				this.diqu_id = this.diqu_click_id;
			},
			cenci(index) {
				this.cenci_id = index.id;
				this.cenci_name = index.name;
			},
			leixin(index) {
				this.leixin_id = index.id;
				this.leixin_name = index.name;
			},
			diqu(index) {
				this.diqu_id = index.id;
				this.diqu_name = index.name;
			},
			click_sure(){
				this.mask_show = false;
				this.cenci_click_name = this.cenci_name;
				this.leixin_click_name = this.leixin_name;
				this.diqu_click_name = this.diqu_name;
				this.cenci_click_id = this.cenci_id;
				this.leixin_click_id = this.leixin_id;
				this.diqu_click_id = this.diqu_id;
				
				this.page = 10
				this.select_scroll = true;
				// console.log({cenci:this.cenci_name,leixin:this.leixin_name,diqu:this.diqu_name})
				this.getSelectUniversity({cenci:this.cenci_name,leixin:this.leixin_name,diqu:this.diqu_name})
				
			},
			to_university_search() {
				uni.navigateTo({
					url: '../university_search/university_search'
				})
			},
			to_university_detail(item) {
				uni.navigateTo({
					url: `../university_detail/university_detail?name=${item.name}`,
				})
			},
			getSelectUniversity(item) {
				// console.log(item)
				let {cenci,leixin,diqu} = item
				// console.log({cenci,leixin,diqu})
				// return;
				uni.request({
					url: `http://localhost:3000/api/selectUniversity`,
					data:{
						cenci:cenci,
						leixin:leixin,
						diqu:diqu,
						size:this.page
					},
					method:'POST',
					success: (res) => {
						// console.log( res.data.result.data)
						this.universityArr = res.data.result.data
						// console.log(this.universityArr)
						if(this.universityArr.length==0) {
							this.is_null = true;
						}else{
							this.is_null = false
						}
					}
				})
			},
			getUniversityList() {
				// console.log(1111)
				uni.request({
					url: `http://localhost:3000/api/getUniversityList`,
					data: {
						size: this.page
					},
					success: (res) => {
						// console.log( res.data.result.data)
						this.universityArr = res.data.result.data
						// console.log(this.universityArr)
					}
				})
			}
		},
		onLoad() {
			this.getUniversityList();
		},
		onReachBottom() {
			this.is_loading = true;
			// return;
			this.page += 10;
			if(this.select_scroll) {
				setTimeout(() => {
					this.getSelectUniversity({cenci:this.cenci_name,leixin:this.leixin_name,diqu:this.diqu_name})
					this.is_loading = false;
				}, 1000)
			}else {
				setTimeout(() => {
					this.getUniversityList();
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
		background-color: f0f0f0;
		padding: 0 25rpx 0 25rpx;
		box-sizing: border-box;
		position: relative;
		/* background-color: #007AFF; */
	}

	.content .search {
		width: 100%;
		height: 70rpx;
		position: relative;
	}

	.content .search .search_input {
		height: 70rpx;
		background-color: #f0f0f0;
		text-align: center;
		line-height: 70rpx;
		color: #bfbfbf;
		border-radius: 10rpx;
	}

	.content .search .search_img {
		position: absolute;
		right: 40rpx;
		top: 10rpx;
		height: 50rpx;
		width: 50rpx;
	}

	.content .select {
		width: 100%;
		height: 100rpx;
		margin-top: 2rpx;
		background-color: white;
		display: flex;
		justify-content: space-around;
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

	.content .buttom {
		.university_list {
			display: flex;
			width: 100%;
			height: 140rpx;
			margin: 7rpx 0;

			// background-color: blue;
			// border-bottom: 1px #767676 solid;
			.list_left {
				width: 20%;
				height: 140rpx;
				// background-color: red;
				display: flex;
				align-items: center;
				justify-content: center;

				// box-sizing: border-box;
				.badge {
					height: 120rpx;
					width: 120rpx;
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
						font-size: 26rpx;
						color: #767676;
					}
				}

				.university_buttom {
					// background-color: red;
					height: 50rpx;
					margin-top: 40rpx;

					.university_xx {
						display: flex;

						.university_nature {}

						.university_type {}

						.university_isDoubleTop {}

						.university_is985 {}

						.university_is211 {}

						.uni_xx {
							font-size: 26rpx;
							color: #767676;
							margin-right: 20rpx;
						}
					}
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
	
	.content .is_null {
		width: 100%;
		height: 1050rpx;
		.is_null_icon {
			width: 100%;
			height: 100%;
		}
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

	.content .mask {
		width: 100%;
		height: 1245rpx;
		position: absolute;
		top: 0;
		left: 0;
		z-index: 99;
		background-color: rgba(0, 0, 0, .4);

		.mask_body {
			width: 70%;
			height: 1150rpx;
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

			.btn {
				margin-top: 10rpx;
				padding-left: 22rpx;

				.title {
					font-size: 30rpx;
				}

				.select {
					margin-top: 15rpx;
					// padding: 15rpx 0;
					width: 100%;
					height: auto;
					display: flex;
					justify-content: flex-start;
					flex-wrap: wrap;

					.list {
						height: 45rpx;
						width: 85rpx;
						margin: 5rpx 15rpx 15rpx 0;
						font-size: 20rpx;
						border-radius: 10rpx;
						text-align: center;
						line-height: 45rpx;
						background-color: #ebebeb;

					}

					.list.active {
						color: red;
						background-color: #ffd9e4;
						border: 1rpx red solid;
						box-sizing: border-box;
					}
				}
			}

			.button_box {
				position: absolute;
				bottom: 0;
				left: 0;
				display: flex;
				width: 100%;

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
