<template>
	<view class="content">
		<!-- <button type="default" ">查大学</button> -->
		<!-- <button type="default" @click="to_major">查专业</button> -->
		<view class="tab">
			<view class="tab_list university" @click="to_university">
				<image class="tab_list_icon" src="../../static/university.png" mode=""></image>
				<view class="tab_list_text">
					院校库
				</view>
			</view>
			<view class="tab_list major" @click="to_major">
				<image class="tab_list_icon" src="../../static/major.png" mode=""></image>
				<view class="tab_list_text">
					专业库
				</view>
			</view>
			<view class="tab_list myself" @click="to_selfxx">
				<image class="tab_list_icon" src="../../static/myself.png" mode=""></image>
				<view class="tab_list_text">
					个人中心
				</view>
			</view>
		</view>
		<view class="time">
			<view class="time_wenzi">
				距离{{thisyear}}年高考倒计时
			</view>
			<view class="time_daojishi">
				{{day}}天{{hour}}小时{{minutes}}分{{seconds}}秒
			</view>
		</view>
		<view class="newprot">
			<view class="newprot_title">
				<image class="title_icon" src="../../static/newprot.png" mode=""></image>
				<view class="title_text">
					高考新闻
				</view>
			</view>
			<view class="newprot_border"></view>
			<view class="newprot_list_box">
				<view class="newprot_list" @click="to_newprot_detail(item)" v-for="(item,index) in newprotArr"
					:key=item.id>
					<view class="newprot_list_left">
						<view class="newprot_list_left_title">
							{{item.title}}
						</view>
						<view class="newprot_list_left_cont">
							<view class="cont_write">
								{{item.writer}}
							</view>
							<view class="cont_time">
								{{item.time}}
							</view>
						</view>
					</view>
					<view class="newprot_list_right">
						<image class="image" :src="`${item.image}`" mode=""></image>
					</view>
				</view>
				<view class="newprot_border">

				</view>
			</view>
		</view>
		<view class="loading" v-if="is_loading">
			正在加载中
			<image v-show="is_loading" class="loading_icon" src="../../static/loading.png" mode=""></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				is_loading: false, //是否加载
				title: 'Hello',
				page: 5,
				newprotArr: [],
				time:{},
				thisyear: '',
				day: '',
				hour: '',
				minutes: '',
				seconds: ''
			}
		},
		onLoad() {
			this.getNewprotList();
			this.daojishi();
		},
		methods: {
			to_selfxx() {
				uni.switchTab({
					url:"../myself/myself"
				})
			},
			to_university() {
				uni.navigateTo({
					url: "../university/university"
				})
			},
			to_major() {
				uni.navigateTo({
					url: "../major/major"
				})
			},
			to_newprot_detail(item) {
				let title = item.title
				console.log(title)
				uni.navigateTo({
					url: `../newprot_detail/newprot_detail?title=${title}`,

				})
			},
			daojishi() { //倒计时
			let that = this;
				 this.time=setInterval(function() {
					//现在的日期
					let nd = new Date();
					that.thisyear = nd.getFullYear();
					//目标的日期 0-11
					let td = new Date(that.thisyear, 5, 7);
					//td-nd  时间戳 毫秒差  把diff换算成天，小时，分钟，秒
					let diff = td - nd;
					that.day = parseInt(diff / (1000 * 60 * 60 * 24));
					that.hour = parseInt(diff % (1000 * 60 * 60 * 24) / (1000 * 60 * 60));
					that.minutes = parseInt(diff % (1000 * 60 * 60) / (1000 * 60));
					that.seconds = parseInt(diff % (1000 * 60 * 60) % (1000 * 60) / 1000);

				}, 1000)

			},
			getNewprotList() {
				// console.log(1111)
				uni.request({
					url: `http://localhost:3000/api/getNewprot`,
					data: {
						size: this.page
					},
					success: (res) => {
						// console.log( res.data.result.data)
						this.newprotArr = res.data.result.data
						// console.log(this.newprotArr)
					}
				})
			}

		},
		
		onReachBottom() {
			// return;
			this.is_loading = true;
			this.page += 10;
			setTimeout(() => {
				this.getNewprotList();
				this.is_loading = false;
			}, 1000)
		},
		onUnload() {
			clearInterval(this.time)
		}
	}
</script>

<style lang="scss">
	.content {
		// display: flex;
		// flex-direction: column;
		// align-items: center;
		// justify-content: center;
		background-color: rgb(248, 248, 248);

		.tab {
			width: 100%;
			height: 200rpx;
			padding: 50rpx 0;
			// background-color: red;
			display: flex;
			justify-content: space-around;
			align-items: center;

			.tab_list {
				display: flex;
				flex-direction: column;
				justify-content: center;
				align-items: center;
				width: 200rpx;

				.tab_list_icon {
					height: 90rpx;
					width: 90rpx;
				}

				.tab_list_text {
					margin-top: 20rpx;
					font-size: 28rpx;
				}
			}
		}

		.time {
			padding: 0 0 50rpx;

			.time_wenzi {
				width: 100%;
				text-align: center;
				margin-bottom: 20rpx;
				font-size: 38rpx;
			}

			.time_daojishi {
				width: 100%;
				text-align: center;
				color: #F0AD4E;
			}
		}

		.newprot {

			background-color: white;

			.newprot_title {
				display: flex;
				height: 80rpx;
				align-items: center;
				padding: 0 20rpx;
				box-sizing: border-box;

				.title_icon {
					height: 60rpx;
					width: 60rpx;
					margin-right: 20rpx;
				}

				.title_text {
					font-size: 30rpx;
					font-weight: bold;
				}
			}

			.newprot_border {
				height: 1rpx;
				width: 100%;
				background-color: #d5d5d5;
				transform: scaleY(0.5);
			}

			.newprot_list_box {
				.newprot_list {
					padding: 20rpx 20rpx;
					box-sizing: border-box;
					height: 260rpx;
					// background-color: red;
					display: flex;

					.newprot_list_left {
						// background-color: red;
						width: 65%;
						display: flex;
						flex-direction: column;
						justify-content: space-between;
						padding: 10rpx 0;
						box-sizing: border-box;

						.newprot_list_left_title {
							font-size: 28rpx;
							font-weight: bold;
							word-break: break-all;
							text-overflow: ellipsis;
							overflow: hidden;
							display: -webkit-box;
							-webkit-line-clamp: 2;
							-webkit-box-orient: vertical;
						}

						.newprot_list_left_cont {
							font-size: 20rpx;
							display: flex;
							width: 100%;
							justify-content: space-between;
							color: #ababab;

							.cont_write {
								margin-left: -10rpx;
								// text-transform: scale(0.8);
								transform: scale(0.85);
							}

							.cont_time {
								margin-left: -30rpx;
								transform: scale(0.85);
							}
						}

						// background-color: blue;
					}

					.newprot_list_right {
						// background-color: blue;
						width: 35%;
						display: flex;
						align-items: center;
						justify-content: center;

						.image {
							width: 220rpx;
							height: 180rpx;
							// background-color: red;
							border-radius: 10rpx;
						}
					}
				}

			}
		}
	
		.loading {
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
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
