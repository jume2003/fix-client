<template>
	<div class="infinite-list-wrapper" style="overflow:auto;text-align: center;">
		<ul class="list" v-infinite-scroll="load" infinite-scroll-disabled="disabled">
			<li v-for="item in orderList" class="list-item">
				<el-card class="el-card">
					<el-row :gutter="24">
						<el-col :span="12">
							<h4>{{item.title}}</h4>
						</el-col>
						<el-col :span="4">
							<h5>一级</h5>
						</el-col>
						<el-col :span="8" style="text-align: right;">
							<h5>状态:未指派</h5>
						</el-col>
					</el-row>

					<el-row :gutter="24" style="margin-top: 10pt; color: #b3b3b3;">
						<el-col :span="16">
							<h5>地址：惠农区康乐路1号</h5>
							<h5>项目名称：机械手从卡舱...</h5>
							<h5>创建时间：2020-12-12 12:00</h5>
						</el-col>

						<el-col :span="8" style="text-align: right;">
							<h5>指派人：老李</h5>
							<h5>维修人：老李</h5>
						</el-col>
					</el-row>
					<el-row :gutter="24" style="margin-top: 10pt; color: #b3b3b3;">
						<el-button @click="onOrderDetailed(item)">查看</el-button>
						<el-button @click="assign" v-if="item.state==0">指派</el-button>
						<el-button @click="receivingOrder" v-if="item.state!=0">接单</el-button>
					</el-row>
				</el-card>
			</li>
		</ul>
		<p v-if="loading">加载中...</p>
		<p v-if="noMore">没有更多了</p>
	</div>
</template>

<script>
	export default {
		name: "orderlist",
		props: {
			filter: {
				type: Number,
				default: 0
			},
		},
		data() {
			return {
				orderList: [],
				loading: false,
			}

		},
		computed: {
			noMore() {
				return this.orderList.length >= 10
			},
			disabled() {
				return this.loading || this.noMore
			}
		},
		onLoad() {

		},
		methods: {
			onOrderDetailed(date) {
				console.log(date)
				uni.navigateTo({
					url: '/pages/orderdetailed/orderdetailed'
				})
			},
			assign() {
				uni.navigateTo({
					url: '/pages/assign/assign'
				})
			},
			receivingOrder() {
				this.$message.success("接单成功")
				uni.reLaunch({
					url: '/pages/mine/mine'
				})
			},
			load() {
				this.loading = true
				for (let i = 0; i < 5; i++) {
					this.orderList.push({
						title: '1',
						state: this.filter
					})
				}
				setTimeout(() => {
					this.orderList.push({
						title: this.orderList.length + 1,
						state: 0
					})
					this.loading = false
				}, 2000)
			}
		}
	}
</script>

<style>
	.content {
		text-align: center;
		margin-top: 200upx;
	}

	.time {
		font-size: 13px;
		color: #999;
	}

	.bottom {
		margin-top: 13px;
		line-height: 12px;
	}

	.button {
		padding: 0;
		float: right;
	}

	.image {
		width: 100%;
		display: block;
	}

	.clearfix:before,
	.clearfix:after {
		display: table;
		content: "";
	}

	.clearfix:after {
		clear: both
	}

	.bg-page {
		text-align: center;
		padding: 0px;
		margin: 0px;
	}

	.el-card {
		border-radius: 4px;
		border: 1px solid #e6ebf5;
		background-color: #fff;
		overflow: hidden;
		color: #303133;
		webkit-transition: .3s;
		transition: .3s;
		margin: 5px;
		margin-right: 20px;
	}
</style>
