<template>
	<div class="app-container">
		<el-row :style="{ flexGrow: 1, overflowY: 'auto' }">
			<!-- 中间可滑动的区域 -->
			<OrderUnity :showall=true :isedit=true>
			</OrderUnity>
		</el-row>
		<el-row>
			<el-col :span="24" style="position: fixed; bottom: 0; width: 100%; z-index:100;">
				<!-- 底部固定的区域 -->
				<el-form ref="form" label-position="top" :model="form" label-width="80px"
					style="padding: 20px;background-color: white;">
					<el-form-item label="团队成员" style="text-align: center;">
						<el-table :data="form.member_list" :border="true" show-summary style="width: 100%">

							<el-table-column prop="name" label="名字" width="auto">
								<template slot-scope="scope">
									<el-select v-model="scope.row.name" placeholder="名字">
										<el-option v-for="item in memberop_list" :key="item.name" :label="item.name"
											:value="item.name">
										</el-option>
									</el-select>
								</template>
							</el-table-column>

							<el-table-column label="操作" width="80pt">
								<template slot-scope="scope">
									<el-button size="mini" type="danger" @click="handleDelete(scope.$index, scope.row)">
										删除</el-button>
								</template>
							</el-table-column>

						</el-table>
						<el-button size="mini" type="danger" @click="handleAdd()">添加成员</el-button>
						<el-button size="mini" type="danger" @click="handleFinish()">完成</el-button>
					</el-form-item>
				</el-form>



			</el-col>
		</el-row>
	</div>
</template>

<script>
	import OrderUnity from '../../components/orderunity.vue'
	export default {
		components: {
			OrderUnity,
		},
		data() {
			return {
				memberop_list: [{
					name: '老李'
				}, {
					name: '老许'
				}, {
					name: '老王'
				}],
				form: {
					member_list: [],
				}
			}
		},
		methods: {
			handleAdd() {
				var d = new Date(); //获取系统当前时间
				this.form.member_list.push({
					id: '0',
					name: '',
				})
			},
			handleDelete(index, data) {
				console.log(index, data);
				this.form.member_list.splice(index, 1)
			},
			handleFinish() {
				uni.reLaunch({
					url: '/pages/tabbar/orders/orders?filter=1'
				})
			}
		}
	}
</script>

<style>
	.app-container {
		display: flex;
		flex-direction: column;
		height: 100%;
	}
</style>
