<template>
	<view class="content">
		<view class="search_box">
			<input 
			ref="input"
			@click="input_click"
			@input="input_shuru"
			class="input_search" 
			type="text" 
			v-model.trim="input_cont" 
			value="" placeholder="输入搜索内容"
			@keydown.enter="search" />
			<image v-show="icon_show" class="input_icon" src="../../static/input_delete.png" mode=""
				@click="clear"></image>
			<view class="input_border"></view>
		</view>
		<view class="search_list" v-for="(item,index) in university_xx" :key=item.id v-if="list_show">
			<view class="university_list"  @click="to_university_detail(item)">
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
		<view class="search_histroy" v-if="histroy_show">
			<view class="zuijin">
				<view class="histroy_title">
					最近搜索
				</view>
				<view class="clearHistroy_button" @click="clearHistroy">
					清除记录
				</view>
			</view>
			<view class="histroy_list">
				<view class="histroy_text" v-for="(item,index) in histroy_arr" :key=index @click="search_university_list(item)">
					{{item}}
				</view>
			</view>
		</view>
		<view class="nothing" v-if="nothing_show">
			无搜索结果~
		</view>
	</view>

</template>

<script>
	export default {
		data() {
			return {
				input_cont: '',         //输入框的信息
				icon_show:false,       //判断输入框的取消按钮是否显示
				histroy_show: true,    //判断历史记录是否显示
				list_show: false,     //判断列表是否显示
				nothing_show:false,   //判断是否空白
				university_xx:[],   //高校列表
				page:10,            //搜索条数
				histroy_str:'',     //历史记录
				histroy_arr:[]        //历史记录列表
			}
		},
		methods: {
			clear() {
				// console.log(111)
				this.input_cont = "";
				if(this.input_cont.length == 0){
					this.icon_show = false
				}else {
					this.icon_show = true
				}
			},
			getUniversityList() {
				// console.log(1111)
				uni.request({
					url: `http://localhost:3000/api/getUniversityList`,
					data: {
						size: this.page
					},
					success: (res) => {
						console.log( res.data.result.data)
						this.university_xx = res.data.result.data
						// console.log(this.universityArr)
						
						if(this.university_xx.length==0){
							this.histroy_show = false;
							this.list_show = false;
							this.nothing_show = true;
						}else {
							this.histroy_show = false;
							this.list_show = true;
						}
					}
				})
			},
			search() {
				console.log(this.histroy_str,'1');
				let str = this.histroy_str;
				this.histroy_arr = str.split(',');
				console.log(this.histroy_arr,'2')
				this.histroy_arr.push(this.input_cont);
				this.histroy_arr = [... new Set(this.histroy_arr)]
				console.log(this.histroy_arr,'3')
				this.histroy_str = this.histroy_arr.toString();
				console.log(this.histroy_str,'4')
				uni.setStorage({
					key: 'storage_key',
					data: this.histroy_str,
					success: function () {
						// console.log('success');
					}
				});
				
				this.histroy_show = false;
				this.list_show = true;
				if(this.input_cont==''){
					// console.log(111);
					this.getUniversityList();
					return ;
				}
				// return;
				uni.request({
					url:`http://localhost:3000/api/searchUniversity`,
					data:{
						name:this.input_cont
					},
					method:"POST",
					success: (res) => {
						// console.log(res.data.result.data);
						this.university_xx = res.data.result.data;
						if(this.university_xx.length==0){
							this.histroy_show = false;
							this.list_show = false;
							this.nothing_show = true;
						}else {
							this.histroy_show = false;
							this.list_show = true;
						}
					}
				})
			},
			input_click() {
				if(this.input_cont.length == 0){
					this.icon_show = false
				}else {
					this.icon_show = true
				}
			},
			input_shuru() {
				if(this.input_cont.length == 0){
					this.icon_show = false
				}else {
					this.icon_show = true
				}
			},
			to_university_detail(item) {
				uni.navigateTo({
					url: `../university_detail/university_detail?name=${item.name}`,
				})
			},
			search_university_list(cont) {
				this.input_cont = cont
				this.histroy_show=false;
				this.list_show = true;
			},
			clearHistroy(){
				uni.showModal({
				    title: '提示',
				    content: '确定清除吗？',
				    success: function (res) {
				        if (res.confirm) {
				            uni.removeStorage({
				            	key: 'storage_key',
				            	success: function (res) {
				            		// console.log('success');
				            		this.histroy_arr = []
									uni.showToast({
													title: '清除成功',
													icon:'exception',
													duration:850
												});
				            	}
				            });
							
				        } else if (res.cancel) {
				            console.log('用户点击取消');
				        }
				    }
				});
				
			}
		},
		// 创建成功后来定义回车事件
		created() {
			document.onkeydown = (e) => {
				if (window.event === undefined) {
					var key = e.keyCode
				} else {
					// eslint-disable-next-line no-redeclare
					var key = window.event.keyCode
				}
				if (key === 13) {
					//  输入身份证按下回车后鼠标失去焦点
					this.icon_show = false
					this.$refs.input.$el.querySelector('input').blur()
				}
			}
		},
		destroyed() {
			document.onkeydown = null;
		},
		onLoad(){
			uni.getStorage({
				key: 'storage_key',
				success: (res)=> {
					console.log(111);
					// console.log(res.data);
					this.histroy_str = res.data;
					this.histroy_arr = this.histroy_str.split(',');
					this.histroy_arr.shift();
				},
				// fail:()=>{
				// 	uni.setStorage({
				// 		key: 'storage_key',
				// 		data: '',
				// 		success: function () {
				// 			// console.log('success');
				// 		}
				// 	});
				// }
			});
		},
		onReachBottom() {
			// this.is_loading = true;
			// return;
			this.page += 10;
			setTimeout(() => {
				this.getUniversityList();
				
				// this.is_loading = false;
			}, 300)
			// console.log("onReachBottom");
		}
	}
</script>

<style lang="scss">
	.content {
		padding: 20rpx;
		box-sizing: border-box;

		.search_box {
			height: 80rpx;
			position: relative;

			.input_search {
				// background-color: red;
				height: 60rpx;
				font-size: 28rpx;
			}

			.input_icon {
				position: absolute;
				right: 15rpx;
				top: 15rpx;
				height: 35rpx;
				width: 35rpx;
			}

			.input_border {
				background-color: #999999;
				width: 100%;
				height: 1rpx;
				transform: scaleY(0.2);
			}
		}

		.search_histroy {
			.zuijin {
				width: 100%;
				display: flex;
				justify-content: space-between;
				.histroy_title {
					font-size: 28rpx;
					color: #999999;
				}
				.clearHistroy_button {
					font-size: 28rpx;
					color: #999999;
				}
			}

			.histroy_list {
				.histroy_text {
					font-size: 26rpx;
					margin-top: 30rpx;
				}
			}
		}

		.search_list {
			.university_list {
				display: flex;
				width: 100%;
				height: 120rpx;
				margin: 3rpx 0;

				// background-color: blue;
				// border-bottom: 1px #767676 solid;
				.list_left {
					width: 15%;
					height: 120rpx;
					// background-color: red;
					display: flex;
					align-items: center;
					justify-content: center;

					// box-sizing: border-box;
					.badge {
						height: 90rpx;
						width: 90rpx;
					}
				}

				.list_right {
					// background-color: yellow;
					width: 85%;
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
						margin-top: 20rpx;

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
	
		.nothing {
			width: 100%;
			height: 400rpx;
			line-height: 400rpx;
			text-align: center;
		}
	}
</style>
