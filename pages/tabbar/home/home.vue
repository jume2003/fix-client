<template>
	<el-container class="bg-page">
		<el-header style="height: 150px;">
			欢迎回来
			<el-carousel height="150px">
				<el-carousel-item v-for="item in img_list" :key="item">
					<img :src="item">
				</el-carousel-item>
			</el-carousel>
		</el-header>
		<el-main>

			<el-row :gutter="24" class="card_block">
				<el-col :span="8" style="padding: 20pt;" v-for="item in home_dates">
					<div>
						<el-row>
							<h2>
								{{item.value}}
							</h2>
						</el-row>
						<el-row>
							{{item.title}}
						</el-row>
					</div>
				</el-col>
			</el-row>

			<el-row :gutter="24" class="card_block">
				<el-row :gutter="24" style="text-align: left;padding: 5pt;">
					<el-col :span="24">
						<h3>主要功能</h3>
					</el-col>
				</el-row>

				<el-row :gutter="24">
					<el-col :span="8" style="padding: 20pt;" v-for="item in tool_list">
						<div class="icon-button" @click="onClickTools(item.url,item.type)">
							<el-row>
								<i :class="item.icon"></i>
							</el-row>
							<el-row>
								{{item.title}}
							</el-row>
						</div>
					</el-col>
				</el-row>
			</el-row>


			<el-row :gutter="24" class="card_block">
				<el-row :gutter="24" style="text-align: left;padding: 5pt;">
					<el-col :span="24">
						<div>
							<h3>数据统计</h3>
						</div>
					</el-col>
				</el-row>

				<el-row :gutter="24">
					<el-col :span="8" style="padding: 20pt;" v-for="item in date_list">
						<div class="icon-button">
							<el-row>
								<i :class="item.icon"></i>
							</el-row>
							<el-row>
								{{item.title}}
							</el-row>
						</div>
					</el-col>
				</el-row>
			</el-row>

		</el-main>

	</el-container>
</template>

<script>
	export default {
		data() {
			return {
				img_list: [
					"https://www.investopedia.com/thmb/4mQYo6Nbw4WfmN3CN1Q99-QEhd8=/1500x0/filters:no_upscale():max_bytes(150000):strip_icc()/shutterstock_227705986_google_android_robot-5bfc3afbc9e77c00587b1dcf.jpg",
					"https://i.ytimg.com/vi/2eXoM6YUPLk/maxresdefault.jpg",
					"https://img0.baidu.com/it/u=741268616,1401664941&fm=253&app=138&size=w931&n=0&f=JPEG&fmt=auto?sec=1683392400&t=26f77d7bbdb9a46da328db5080797b74"
				],
				home_dates: [{
						title: '总设备数',
						value: 36
					},
					{
						title: '实时故障',
						value: 10
					},
					{
						title: '维修中',
						value: 5
					}
				],
				tool_list: [
					//enum(//0待指派 1待接单 2待完成 3已完成 4未完成)	工单状态 
					{
						title: '报修',
						icon: 'el-icon-edit',
						url: '/pages/postorder/postorder',
						type: 1
					},
					{
						title: '待指派',
						icon: 'el-icon-s-promotion',
						url: '/pages/tabbar/orders/orders?filter=0',
						type: 0
					},
					{
						title: '待接单',
						icon: 'el-icon-s-order',
						url: '/pages/tabbar/orders/orders?filter=1',
						type: 0
					},
					{
						title: '维修中',
						icon: 'el-icon-s-cooperation',
						url: '/pages/tabbar/orders/orders?filter=2',
						type: 0
					},
					{
						title: '人员管理',
						icon: 'el-icon-s-custom',
						url: '/pages/personmanager/personmanager',
						type: 1
					},
					{
						title: '物料管理',
						icon: 'el-icon-s-shop',
						url: '/pages/materialmanager/materialmanager',
						type: 1
					}
				],
				date_list: [{
						title: '维修看板',
						icon: 'el-icon-edit'
					},
					{
						title: '知识库',
						icon: 'el-icon-s-promotion'
					}
				],
				title: 'Hello'
			};
		},
		onLoad() {},
		methods: {
			onClickTools(url, type) {
				console.log(url, type)
				if (type == 0) {
					uni.reLaunch({
						url: url
					})
				} else {
					uni.navigateTo({
						url: url
					})
				}
			}
		}
	};
</script>

<style>
	.content {
		text-align: center;
		height: 100%;
	}

	.card_block {
		text-align: center;
		background-color: white;
		margin-top: 10pt;
		border-radius: 10px;
	}

	.el-col {
		border-radius: 4px;
	}

	.bg-purple-dark {
		background: #99a9bf;
	}

	.bg-purple {
		background: #d3dce6;
	}

	.bg-purple-light {
		background: #e5e9f2;
	}

	.grid-content {
		border-radius: 4px;
		min-height: 36px;
	}

	.icon-button {
		background-color: #f5f5f5;
		border-radius: 4px;
		padding: 5pt;
	}

	.bg-page {
		text-align: center;
		background: #f5f5f5;
		padding: 0px;
		margin: 0px;
		height: 90vh;
	}
</style>
