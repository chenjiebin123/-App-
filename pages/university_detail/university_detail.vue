<template>
	<view class="content">
		<view class="university_jbxx">
			<view class="university_sc" @click="university_like_click">
				{{is_like==true?'取消添加':'添加意向院校'}}
			</view>
			<image class="university_zp" :src="`${university_detail.schoolScenery}`" mode=""></image>
			<view class="university_xx">
				<view class="xx_box">
					<view class="box_left">
						<image class="badge" :src="`${university_detail.badge}`" mode=""></image>
					</view>
					<view class="box_right">
						<view class="box_right_top">
							{{university_detail.name}}
						</view>
						<view class="box_right_buttom">
							<view class="code">
								院校代码：{{university_detail.code}}
							</view>
							<view class="address">
								地址：{{university_detail.province}}
							</view>
							<view class="ranking">
								排名：{{university_detail.ranking}}
							</view>
						</view>
					</view>
				</view>
				<view class="arrangement">
					<view class="arrangement_title">
						院校层次:
					</view>
					<view class="arrangement_box">
						<view class="arrangement_list">
							{{university_detail.nature}}
						</view>
						<view class="arrangement_list">
							{{university_detail.type}}
						</view>
						<view v-if="university_detail.is985==1?true:false" class="arrangement_list">
							985
						</view>
						<view v-if="university_detail.is211==1?true:false" class="arrangement_list">
							211
						</view>
						<view v-if="university_detail.isDoubleTop==1?true:false" class="arrangement_list">
							双一流
						</view>
					</view>
				</view>
			</view>

		</view>
		<view class="university_introduce">
			<view class="introduce_tab">
				<view v-for="(item,index) in tab_list" :key="item.id"
					:class="tab_num==item.id?'active tab_text':'tab_text'" @click="tab_click(item.id)">
					{{item.title}}
				</view>
			</view>
		</view>
		<view class="university_detail" v-if="tab_num==1?true:false">
			<view class="university_gaikuang">
				<view class="university_gaikuang_top">
					<view class="university_gaikuang_title">
						学校概况
					</view>
					<view class="university_gaikuang_more" @click="to_more_gaikuang(university_detail)">
						更多<image class="university_gaikuang_more_icon" src="../../static/to.png" mode=""></image>
					</view>
				</view>
				<view class="university_gaikuang_content">
					<rich-text :nodes="university_detail.briefIntroduction"></rich-text>
				</view>
			</view>
			<view class="university_guimo">
				<view class="major_survey">
					<view class="survey_top">
						<view class="major_title">
							院校规模
						</view>
					</view>
				</view>
				<view class="major_detail">
					<view class="major_level major_box">
						<view class="major_box_top">
							<image class="detail_icon" src="../../static/mianji.png" mode=""></image>
							<view class="detail_level_title detail_level_text">
								占地面积
							</view>
						</view>
						<view class="level detail_text">
							{{university_detail.area}}亩
						</view>
					</view>
					<view class="major_boyGirlRate major_box">
						<view class="major_box_top">
							<image class="detail_icon" src="../../static/teacher.png" mode=""></image>
							<view class="detail_level_boygirl detail_level_text">
								专职教师
							</view>
						</view>
						<view class="boyGirlRate detail_text">
							{{university_detail.teachersNum}}人
						</view>
					</view>
					<view class="major_lastYearEmployedRate major_box">
						<view class="major_box_top">
							<image class="detail_icon" src="../../static/student.png" mode=""></image>
							<view class="detail_level_jiuye detail_level_text">
								全日制学生
							</view>
						</view>
						<view class="lastYearEmployedRate detail_text">
							{{university_detail.studentsNum}}人
						</view>
					</view>
					<view class="major_avgSalary major_box">
						<view class="major_box_top">
							<image class="detail_icon" src="../../static/shishengbi.png" mode=""></image>
							<view class="detail_level_gongzi detail_level_text">
								师生比
							</view>
						</view>
						<view class="avgSalary detail_text">
							{{university_detail.studentTeacherRate}}
						</view>
					</view>
				</view>
			</view>
			<view class="university_yuanxi">
				<view class="university_yuanxi_top">
					<view class="university_yuanxi_title">
						院系设置
					</view>
					<view class="university_yuanxi_more" @click="to_more_yuanxi(university_detail.name)">
						更多<image class="university_yuanxi_more_icon" src="../../static/to.png" mode=""></image>
					</view>
				</view>
				<view class="university_yuanxi_content">
					<view class="yuanxi_list" v-for="(item,index) in university_yuanxi_list" :key=index>
						{{item.department}}
					</view>
				</view>
			</view>
			<view class="university_zhibiao">
				<view class="major_survey">
					<view class="survey_top">
						<view class="major_title">
							办学指标
						</view>
					</view>
				</view>
				<view class="major_detail">
					<view class="major_level major_box">
						<view class="major_box_top">
							<image class="detail_icon" src="../../static/chuanban.png" mode=""></image>
							<view class="detail_level_title detail_level_text">
								创办时间
							</view>
						</view>
						<view class="level detail_text">
							{{university_detail.createTime}}年
						</view>
					</view>
					<view class="major_boyGirlRate major_box">
						<view class="major_box_top">
							<image class="detail_icon" src="../../static/jiuye.png" mode=""></image>
							<view class="detail_level_boygirl detail_level_text">
								就业率
							</view>
						</view>
						<view class="boyGirlRate detail_text">
							{{university_detail.employedRate}}
						</view>
					</view>
					<view class="major_lastYearEmployedRate major_box">
						<view class="major_box_top">
							<image class="detail_icon" src="../../static/shenzao.png" mode=""></image>
							<view class="detail_level_jiuye detail_level_text">
								深造率
							</view>
						</view>
						<view class="lastYearEmployedRate detail_text">
							{{university_detail.furtherStudyRate}}
						</view>
					</view>
					<view class="major_avgSalary major_box">
						<view class="major_box_top">
							<image class="detail_icon" src="../../static/zhuanye.png" mode=""></image>
							<view class="detail_level_gongzi detail_level_text">
								优势专业
							</view>
						</view>
						<view class="avgSalary detail_text">
							{{university_detail.advancedMajorNum}}个
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="university_zhaoshen" v-if="tab_num==2?true:false">
			<view class="select" @click="select_detail_show">
				<view class="nature">
					<view class="name">
						{{nianfen_name}}
					</view>
					<image src="../../static/select_img.png" mode=""></image>
				</view>
				<view class="type">
					<view class="name">
						{{kemu_name}}
					</view>
					<image src="../../static/select_img.png" mode=""></image>
				</view>
				<view class="province">
					<view class="name">
						{{pici_name}}
					</view>
					<image src="../../static/select_img.png" mode=""></image>
				</view>
			</view>
			<view class="fenshuxian">
				<view class="fenshuxian_xx fenshuxian_xiangtong">
					<view class="nf xiangtong">
						年份
					</view>
					<view class="pcx xiangtong">
						批次线
					</view>
					<view class="tdx xiangtong">
						投档线
					</view>
					<view class="pw xiangtong">
						最低排位
					</view>
					<view class="rs xiangtong">
						录取人数
					</view>
				</view>
				<view class="fenshuxian_fs fenshuxian_xiangtong" v-for="(item,index) in fenshu_xx_arr" :key=index>
					<view class="nf xiangtong">
						{{item.year}}
					</view>
					<view class="pcx xiangtong">
						{{item.enrollLine}}
						
					</view>
					<view class="tdx xiangtong">
						{{item.admissionMinScore}}
						
					</view>
					<view class="pw xiangtong">
						{{item.admissionMinRanking}}
						
					</view>
					<view class="rs xiangtong">
						{{item.enrollRealCount}}
					</view>
				</view>
			</view>
			<view class="luquxian">
				<view class="luquxian_title">
					院校录取情况
				</view>
				<view class="luquxian_xx luquxian_xiangtong">
					<view class="zyz xiangtong">
						专业组
					</view>
					<view class="zdf xiangtong">
						最低分
					</view>
					<view class="pw xiangtong">
						最低排位
					</view>
					<view class="jhrs xiangtong">
						计划人数
					</view>
					<view class="lqrs xiangtong">
						录取人数
					</view>
				</view>
				<view class="luquxian_fs luquxian_xiangtong" v-for="(item,index) in luqu_xx_arr" :key=index>
					<view class="nf xiangtong">
						{{item.universityCode}}
					</view>
					<view class="nf xiangtong">
						{{item.admissionMinScore}}
					</view>
					<view class="pcx xiangtong">
						{{item.admissionMinRanking}}
					</view>
					<view class="tdx xiangtong">
						{{item.enrollPlanCount}}
					</view>
					<view class="pw xiangtong">
						{{item.enrollRealCount}}
					</view>
				</view>
			</view>
		</view>
		<view class="mask" @click.self="select_detail_hidden" v-show=mask_show>
			<view class="mask_body">
				<view class="mask_title">
					条件筛选
				</view>
				<view class="btn  nature">
					<view class="title">
						年份
					</view>
					<view class="select">
						<view :class="nianfen_id==item.id?'list active':'list'" @click="nianfen(item)"
							v-for="(item,index) in xuanze_arr" :key=item.id>
							{{item.year}}
						</view>
					</view>
				</view>
				<view class="btn type">
					<view class="title">
						科目
					</view>
					<view class="select">
						<view :class="kemu_id==item.id?'list active':'list'" @click="kemu(item)"
							v-for="(item,index) in kemuList" :key=item.id>
							{{item.category}}
						</view>
					</view>
				</view>
				<view class="btn province">
					<view class="title">
						批次
					</view>
					<view class="select">
						<view :class="pici_id==item.id?'list active':'list'" @click="pici(item)"
							v-for="(item,index) in piciList" :key=item.id>
							{{item.enrollType}}
						</view>
					</view>
				</view>
				<view class="button_box">
					<view class="close">
						<image @click="select_detail_hidden" class="close_icon" src="../../static/close.png" mode="">
						</image>
					</view>
					<view class="button" @click="click_reset">重置</view>
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
				userName:'',//用户名
				is_like:false,//是否收藏
				piciList: [], //批次列表
				kemuList: [], //科目列表
				nianfenList: [], //年份列表
				fenshu_xx_arr: [], //每年分数信息列表
				luqu_xx_arr: [], //院校录取详情
				cateArr:[],//每年的选课信息
				like_dataArr:[],//理科每年分数线数据
				wenke_dataArr:[],//文科每年分数线数据
				qita_dataArr:[],//其他每年分数线数据
				nianfen_click_id: 0,
				kemu_click_id: 0,
				pici_click_id: 0,
				nianfen_id: 0,
				kemu_id: 0,
				pici_id: 0,
				nianfen_click_name: '', //选项点击
				kemu_click_name: '', //选项点击
				pici_click_name: '', //选项点击
				nianfen_name: '',
				kemu_name: '',
				pici_name: '',
				tab_num: 1,
				university_name: '', //学校名字
				mask_show: false, //年份，批次，科目的选择层是否显示
				university_detail: {},
				zhaoshen_xuanze: {}, //招生选择
				xuanze_arr: [], //选择内容列表
				university_yuanxi_list: [], //院系列表
				tab_list: [{ //tab选项卡
						id: 1,
						title: "院校详情"
					},
					{
						id: 2,
						title: "招生历史"
					}
				]
			}
		},
		methods: {
			tab_click(index) {
				// console.log(index);
				this.tab_num = index;
			},
			to_more_gaikuang(data) {
				// console.log(data)
				uni.navigateTo({
					url: `../university_detail_gaikuang/university_detail_gaikuang?text=${data.briefIntroduction}&phone=${data.phone}&address=${data.address}&homePage=${data.homePage}&name=${data.name}`

				})
			},
			to_more_yuanxi(name) {
				uni.navigateTo({
					url: `../university_detail_yuanxi/university_detail_yuanxi?name=${name}`,

				})
			},
			select_detail_show() {
				// console.log(1111)
				this.mask_show = true;
				this.nianfen_click_name = this.nianfen_name;
				this.kemu_click_name = this.kemu_name;
				this.pici_click_name = this.pici_name;

			},
			select_detail_hidden() {
				this.mask_show = false;
				this.nianfen_click_name = this.nianfen_name;
				this.kemu_click_name = this.kemu_name;
				this.pici_click_name = this.pici_name;
				this.nianfen_id = this.nianfen_click_id;
				this.kemu_id = this.kemu_click_id;
				this.pici_id = this.pici_click_id;
				this.kemuList = this.xuanze_arr[this.nianfen_id].categoryList;
				this.piciList = this.kemuList[this.kemu_id].enrollTypeList;
			},
			click_reset() {
				this.nianfen_id = 0;
				this.kemu_id = 0;
				this.pici_id = 0;
				uni.request({
					url: `http://47.107.23.59/gaokao-2.0/university/planConfig?students=%E5%B9%BF%E4%B8%9C`,
					data: {
						universityName: this.university_name
					},
					success: (res) => {
						this.nianfen_click_name = this.zhaoshen_xuanze.select.year - 0;
						this.kemu_click_name = this.zhaoshen_xuanze.select.category;
						this.pici_click_name = this.zhaoshen_xuanze.select.enrollType;
					},
				})
			},
			click_sure() {
				this.mask_show = false;
				this.nianfen_name = this.nianfen_click_name;
				this.kemu_name = this.kemu_click_name;
				this.pici_name = this.pici_click_name;
				this.nianfen_click_id = this.nianfen_id;
				this.kemu_click_id = this.kemu_id;
				this.pici_click_id = this.pici_id;
				// console.log(1213123)
				// console.log(this.university_name, this.kemu_name, this.nianfen_name, this.pici_name)
				this.getoneyear_xx(this.university_name, this.nianfen_name, this.kemu_name, this.pici_name);
				// this.geteveryyear_xx(this.university_name, this.nianfen_name, this.kemu_name, this.pici_name);
				console.log(this.kemu_name)
				if(this.kemu_name=="物理"||this.kemu_name=="理科") {
					this.fenshu_xx_arr = this.like_dataArr
				}
				else if(this.kemu_name=="历史"||this.kemu_name=="文科") {
					this.fenshu_xx_arr = this.wenke_dataArr
				}
				else {
					this.fenshu_xx_arr = this.qita_dataArr
				}
			},
			nianfen(item) {
				this.nianfen_id = item.id;
				this.kemu_id = 0;
				this.pici_id = 0;
				this.nianfen_click_name = item.year;
				this.kemu_click_name = item.categoryList[0].category;
				this.pici_click_name = item.categoryList[0].enrollTypeList[0].enrollType;
				let that = this;
				this.xuanze_arr.forEach((item, index) => {
					if (item.id == that.nianfen_id) {
						that.kemuList = item.categoryList;
						that.piciList = that.kemuList[0].enrollTypeList;
					}
				})
			},
			kemu(item) {
				this.kemu_id = item.id;
				this.pici_id = 0;
				this.kemu_click_name = item.category;
				this.pici_click_name = item.enrollTypeList[0].enrollType;
				let that = this;
				this.kemuList.forEach((item, index) => {
					if (item.id == that.kemu_id) {
						that.piciList = item.enrollTypeList;
					}
				})
			},
			pici(item) {
				this.pici_click_name = item.enrollType;
				this.pici_id = item.id
			},
			//获取学校基本信息
			getUniversityDetail() {
				uni.request({
					url: `http://localhost:3000/api/getUniversityDetail`,
					data: {
						name: this.university_name
					},
					success: (res) => {
						// console.log( res.data.result.data)
						this.university_detail = res.data.result.data[0]
						console.log(this.university_detail)
					}
				});
			},
			//获取院系信息
			getyuanxi() {
				uni.request({
					url: `http://47.107.23.59/gaokao-2.0/university/department`,
					data: {
						universityName: this.university_name,
						size: 5
					},
					success: (res) => {
						// console.log( res.data.data)
						this.university_yuanxi_list = res.data.data
						// console.log(this.university_detail)
					}
				});
			},
			//请求条件筛选的信息
			gettiaojian() {
				uni.request({
					url: `http://47.107.23.59/gaokao-2.0/university/planConfig?students=%E5%B9%BF%E4%B8%9C`,
					data: {
						universityName: this.university_name
					},
					success: (res) => {
						this.zhaoshen_xuanze = res.data.data;
						this.nianfen_name = this.zhaoshen_xuanze.select.year - 0;
						this.kemu_name = this.zhaoshen_xuanze.select.category;
						this.pici_name = this.zhaoshen_xuanze.select.enrollType;
						this.xuanze_arr = res.data.data.data;


						this.xuanze_arr.forEach((item, index) => {
							item.id = index
							item.categoryList.forEach((item1, index1) => {
								item1.id = index1
								item1.enrollTypeList.forEach((item2, index2) => {
									item2.id = index2
								})
							})
						})
						this.kemuList = this.xuanze_arr[0].categoryList;
						this.piciList = this.kemuList[0].enrollTypeList;
						// console.log(this.xuanze_arr);
						this.getoneyear_xx(this.university_name, this.nianfen_name, this.kemu_name, this
							.pici_name);
						// this.geteveryyear_xx(this.university_name, this.nianfen_name, this.kemu_name, this
						// 	.pici_name);
						// let cateArr = []
						this.xuanze_arr.forEach((item,index)=>{
							item.categoryList.forEach((item1,index1)=>{
								item1.enrollTypeList.forEach((item2, index2) => {
									item2.year=item.year;
									item2.cate =item1.category;
									this.cateArr.push(item2)
								})
							})
						})
						// console.log(this.cateArr);
						
						for(let i = 0 ; i <this.cateArr.length ; i++) {
							// console.log(i)
							this.geteveryyear_xx(this.university_name,this.cateArr[i].year,this.cateArr[i].cate,this.cateArr[i].enrollType)
						}
						this.fenshu_xx_arr =this.wenke_dataArr;
					}
				});
			},
			//获取院校某一年的录取分数
			getoneyear_xx(name, year, cate, enro) {
				uni.request({
					url: `http://47.107.23.59/gaokao-2.0/university/enrollHistoryTitle/v2`,
					data: {
						students: '广东',
						universityName: name,
						year: year,
						category: cate,
						enrollType: enro
					},
					success: (res) => {
						// console.log( res.data.data)
						this.luqu_xx_arr = res.data.data
						// console.log(this.luqu_xx_arr)
					}
				});
			},
			//获取全部年份的分数线信息
			geteveryyear_xx(name, year, cate, enro) {
				// console.log(name, year, cate, enro);
				
				if((cate=='历史'||cate=='文科')&&enro=='本科'){
					uni.request({
						url: `http://47.107.23.59/gaokao-2.0/university/enrollHistoryTitle/v2`,
						data: {
							students: '广东',
							universityName: name,
							year: year,
							category: cate,
							enrollType: enro
						},
						success: (res) => {
							
							if(year>2016){
								res.data.data[0].year = year
								this.wenke_dataArr.push(res.data.data[0]);
							}
						}
					})
				}
				else if((cate=='物理'||cate=='理科')&&enro=='本科'){
					uni.request({
						url: `http://47.107.23.59/gaokao-2.0/university/enrollHistoryTitle/v2`,
						data: {
							students: '广东',
							universityName: name,
							year: year,
							category: cate,
							enrollType: enro
						},
						success: (res) => {
							if(year>2016) {
								res.data.data[0].year = year
								this.like_dataArr.push(res.data.data[0]);
							}
						}
					})
				}
				else if(cate=='美术'){
					uni.request({
						url: `http://47.107.23.59/gaokao-2.0/university/enrollHistoryTitle/v2`,
						data: {
							students: '广东',
							universityName: name,
							year: year,
							category: cate,
							enrollType: enro
						},
						success: (res) => {
							if(year>2016) {
								res.data.data[0].year = year
								this.qita_dataArr.push(res.data.data[0]);
							}
						}
					})
				}
			},
			//收藏院校---请求
			add_like_university(userName,university_name){
				// console.log(userName,university_name);
				uni.request({
					url:`http://localhost:3000/api/addUniversityLike`,
					data:{
						userName,
						university_name
					},
					success: (res) => {
						// console.log(res)
						this.is_like=true
						uni.showToast({
							title: '添加成功',
							duration: 1000,
						});
					}
				})
			},
			//取消收藏院校---请求
			delete_like_university(userName,university_name){
				uni.request({
					url:`http://localhost:3000/api/delectUniversityLike`,
					data:{
						userName,
						university_name
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
			//是否查询收藏了
			search_like_university(userName,university_name){
				uni.request({
					url:`http://localhost:3000/api/searchUniversityLike`,
					data:{
						userName,
						university_name
					},
					success: (res) => {
						console.log(res.data)
						if(res.data.result.is_like==true) {
							this.is_like = true
						}
					}
				})
			},
			//点击收藏按钮
			university_like_click() {
				
				uni.getStorage({
					key:'token',
					success: (res) => {
						// console.log(res.data)
						if(this.is_like==false){
							this.add_like_university(this.userName,this.university_name);
						}else {
							this.delete_like_university(this.userName,this.university_name);
						}
						
					},
					fail: (res) => {
						// console.log('err')
						uni.navigateTo({
							url:'../login/login'
						})
					}
				})
				
			}
		},
		onShow() {
			
		},
		onLoad(option) {
			uni.getStorage({
				key:'userName',
				success: (res) => {
					this.userName = res.data
				}
			})
			this.university_name = option.name;
			this.getUniversityDetail();
			uni.setNavigationBarTitle({
				title: this.university_name
			})
			this.getyuanxi();
			this.gettiaojian();
			this.search_like_university(this.userName,this.university_name);
			// console.log(option.name)
		}
	}
</script>

<style lang="scss">
	.content {
		width: 100%;
		height: 100%;
		background-color: #f0f0f0;

		.university_jbxx {
			width: 100%;
			position: relative;
			background-color: white;
			.university_sc {
				position: absolute;
				right: 50rpx;
				top: 50rpx;
				z-index: 99;
				color: white;
				font-size: 20rpx;
				height: 45rpx;
				width: 170rpx;
				text-align: center;
				line-height: 45rpx;
				border: 2px solid white;
				border-radius: 30rpx;
				// box-sizing: border-box;
			}
			.university_zp {
				width: 100%;
				height: 310rpx;
			}

			.university_xx {
				position: absolute;
				top: 250rpx;
				width: 100%;
				// background-color: white;
				padding-bottom: 10rpx;

				.xx_box {
					display: flex;
					height: 120rpx;
					width: 100%;
					padding: 0 20rpx;

					.box_left {
						margin-right: 30rpx;

						.badge {
							border-radius: 50%;
							border: 8rpx solid white;
							height: 100rpx;
							width: 100rpx;
							// box-sizing: border-box;
						}
					}

					.box_right {

						// background-color: red;
						.box_right_top {
							margin-top: 10rpx;
							color: white;
							font-size: 26rpx;
						}

						.box_right_buttom {
							font-size: 26rpx;
							margin-top: 20rpx;
							display: flex;
							color: #888888;

							.code {
								margin-right: 50rpx;
							}

							.address {
								margin-right: 50rpx;
							}
						}
					}
				}

				.arrangement {
					padding: 0 20rpx;
					margin-top: 10rpx;
					display: flex;
					align-items: center;

					.arrangement_title {
						color: #888888;
						font-size: 26rpx;

					}

					.arrangement_box {
						display: flex;
						align-items: center;
						margin-left: 30rpx;

						.arrangement_list {
							font-size: 20rpx;
							background-color: #ffd7e0;
							color: #ff3815;
							margin-right: 13rpx;
							border-radius: 8rpx;
							padding: 2rpx 10rpx;
						}
					}
				}
			}
		}

		.university_introduce {
			// margin-top: 100rpx;
			padding-top: 130rpx;
			background-color: white;
			// background-color: rgb(248, 248, 248);

			.introduce_tab {
				height: 70rpx;
				// background-color: rgb(248, 248, 248);
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

		.university_detail {
			width: 100%;

			margin-top: 10rpx;

			.university_gaikuang {
				padding-bottom: 40rpx;
				background-color: white;

				.university_gaikuang_top {
					display: flex;
					justify-content: space-between;
					padding: 20rpx 30rpx;
					box-sizing: border-box;
					align-items: center;

					.university_gaikuang_title {
						font-weight: bold;
					}

					.university_gaikuang_more {
						font-size: 26rpx;
						color: #7f7f7f;
						display: flex;
						align-items: center;

						.university_gaikuang_more_icon {
							margin-left: 5rpx;
							height: 26rpx;
							width: 26rpx;
						}
					}
				}

				.university_gaikuang_content {
					padding: 0rpx 30rpx;
					box-sizing: border-box;
					color: #7f7f7f;
					width: 100%;
					word-break: break-all;
					text-overflow: ellipsis;
					overflow: hidden;
					display: -webkit-box;
					-webkit-line-clamp: 3;
					-webkit-box-orient: vertical;
					font-size: 28rpx;

					p {
						text-indent: 24px;
					}
				}
			}

			.university_guimo {
				margin-top: 10rpx;
				background-color: white;

				.major_survey {
					width: 100%;
					background-color: white;
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
					background-color: white;

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

			}

			.university_yuanxi {
				padding-bottom: 10rpx;
				background-color: white;
				margin-top: 10rpx;

				.university_yuanxi_top {
					display: flex;
					justify-content: space-between;
					padding: 20rpx 30rpx;
					box-sizing: border-box;
					align-items: center;

					.university_yuanxi_title {
						font-weight: bold;
					}

					.university_yuanxi_more {
						font-size: 26rpx;
						color: #7f7f7f;
						display: flex;
						align-items: center;

						.university_yuanxi_more_icon {
							margin-left: 5rpx;
							height: 26rpx;
							width: 26rpx;
						}
					}
				}

				.university_yuanxi_content {
					padding: 20rpx 30rpx;
					box-sizing: border-box;

					.yuanxi_list {
						font-size: 28rpx;
						color: #7f7f7f;
						margin-bottom: 20rpx;
					}
				}
			}

			.university_zhibiao {
				margin-top: 10rpx;
				background-color: white;

				.major_survey {
					width: 100%;
					background-color: white;
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
					background-color: white;

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

			}

		}

		.university_zhaoshen {
			width: 100%;
			background-color: white;
			margin-top: 10rpx;
			padding: 0 20rpx;
			box-sizing: border-box;

			.select {
				width: 100%;
				height: 100rpx;
				margin-top: 2rpx;
				background-color: white;
				display: flex;
				justify-content: center;
			}

			.select .nature,
			.select .type,
			.select .province {
				display: flex;
				align-items: center;
				justify-content: center;
				color: black;
				font-size: 30rpx;
				/* background-color: #007AFF; */
			}

			.select .type {
				margin: 0 50rpx;
			}

			.select .nature image,
			.select .type image,
			.select .province image {
				width: 20rpx;
				height: 20rpx;
				margin-left: 10rpx;
			}

			.fenshuxian {
				width: 100%;
				// background-color: red;
				overflow: hidden;
				border-radius: 16rpx;
				margin-bottom: 30rpx;

				.fenshuxian_xiangtong {
					&:nth-child(even) {
						background-color: #f0f0f0;
					}

					&:nth-child(odd) {
						background-color: #ffd8db;
					}
				}

				.fenshuxian_xx {
					padding: 15rpx;
					display: flex;
					justify-content: space-around;
					font-size: 26rpx;

					.xiangtong {
						width: 120rpx;
						text-align: center;
					}
				}

				.fenshuxian_fs {

					padding: 15rpx;
					display: flex;
					justify-content: space-around;
					font-size: 26rpx;

					.xiangtong {
						width: 120rpx;
						text-align: center;
					}
				}
			}

			.luquxian {
				width: 100%;
				// background-color: red;
				overflow: hidden;
				border-radius: 16rpx;
				// margin-bottom: 30rpx;
				// padding: 0 0 80rpx;
				.luquxian_title {
					padding: 20rpx 0;
				}

				.luquxian_xiangtong {
					&:nth-child(odd) {
						background-color: #f0f0f0;
					}

					&:nth-child(even) {
						background-color: #ffd8db;
					}
				}

				.luquxian_xx {
					border-top-left-radius: 16rpx;
					border-top-right-radius: 16rpx;
				}

				.luquxian_xiangtong {

					// padding: 40rpx 0;
					display: flex;
					justify-content: space-around;
					font-size: 26rpx;

					.xiangtong {
						width: 25%;
						height: 90rpx;
						line-height: 90rpx;
						text-align: center;
						border-right: 1rpx solid white;
					}
				}
			}
		}
	}

	.content .mask {
		width: 100%;
		// height: 1245rpx;
		height: 100%;
		position: absolute;
		top: 0;
		left: 0;
		z-index: 99;
		background-color: rgba(0, 0, 0, .4);

		.mask_body {
			width: 70%;
			height: 1150rpx;
			background-color: #f7f7f7;
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
						// width: 85rpx;
						padding: 0 26rpx;
						margin: 5rpx 15rpx 15rpx 0;
						font-size: 20rpx;
						border-radius: 10rpx;
						text-align: center;
						line-height: 45rpx;
						background-color: #ebebeb;
						box-sizing: border-box;
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
