<template>
	<div>
		<el-form :disabled="compisedit" ref="form" label-position="left" :model="form" label-width="80px"
			style="padding: 20px;">
			<el-form-item label="用户单位">
				<el-select v-model="form.user_company" placeholder="请选择">
					<el-option v-for="item in userCompanyList" :key="item" :label="item" :value="item">
					</el-option>
			 </el-select>
			</el-form-item>
			<el-form-item label="电话">
				<el-input v-model="form.phone"></el-input>
			</el-form-item>
			<el-form-item label="地址">
				<el-input type="textarea" v-model="form.address"></el-input>
			</el-form-item>

			<el-form label-width="80px" label-position="left" :inline="true" :model="form" class="demo-form-inline">

				<el-form-item label="仪器型号">
					<el-input maxlength="10" v-model="form.instrument_model"></el-input>
				</el-form-item>
				<el-form-item label="序列号">
					<el-input maxlength="10" v-model="form.serial_number"></el-input>
				</el-form-item>
			</el-form>

			<el-form-item label="维护">
				<el-input type="textarea" :autosize="{ minRows: 2, maxRows: 20}" placeholder="请输入内容"
					v-model="form.maintenance">
				</el-input>
			</el-form-item>
			<el-form-item label="维修">
				<el-input type="textarea" :autosize="{ minRows: 2, maxRows: 20}" placeholder="请输入内容"
					v-model="form.repair">
				</el-input>
			</el-form-item>
			<el-form-item label="其他">
				<el-input type="textarea" :autosize="{ minRows: 2, maxRows: 20}" placeholder="请输入内容"
					v-model="form.other">
				</el-input>
			</el-form-item>

			<el-form-item label="问题点">
				<el-checkbox-group v-model="form.is_daily_equipment">
					<el-checkbox label="设备日常维护" name="type"></el-checkbox>
				</el-checkbox-group>
			</el-form-item>
			<el-input type="textarea" style="margin-bottom: 10pt;" :autosize="{ minRows: 4, maxRows: 20}"
				placeholder="请输入内容" v-model="form.problem">
			</el-input>

			<el-upload class="upload-demo" action="https://jsonplaceholder.typicode.com/posts/"
				:on-preview="handlePreview" :on-remove="handleRemove" :file-list="problemFileList" list-type="picture">
				<el-button size="small" type="primary">点击上传</el-button>
				<div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
			</el-upload>

			<div v-if="showall">
				<el-form-item label="处理结果">
					<el-radio-group v-model="form.solution" style="padding: 10pt;">
						<el-radio label="校对加样器加样位置"></el-radio>
						<el-radio label="校对机械手抓卡位置"></el-radio>
						<el-radio label="检查离心机卡套顺畅度，内部有无杂物"></el-radio>
					</el-radio-group>
				</el-form-item>
				<el-input style="margin-bottom: 10pt;" type="textarea" :autosize="{ minRows: 4, maxRows: 20}"
					placeholder="请输入内容" v-model="form.solution">
				</el-input>

				<el-upload class="upload-demo" action="https://jsonplaceholder.typicode.com/posts/"
					:on-preview="handlePreview" :on-remove="handleRemove" :file-list="completeFileList"
					list-type="picture">
					<el-button size="small" type="primary">点击上传</el-button>
					<div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
				</el-upload>

				<el-form-item label="物料表" />
				<el-table :data="form.material_list" border show-summary style="width: 100%">

					<el-table-column prop="name" label="物料名称" width="auto">
						<template slot-scope="scope">
							<el-select v-model="scope.row.name" placeholder="名称"
								@change="materialSelectCompany(scope.$index, scope.row)">
								<el-option v-for="item in materialNameList" :key="item.name" :label="item.name"
									:value="item.name">
								</el-option>
							</el-select>
						</template>
					</el-table-column>

					<el-table-column prop="count" label="使用数量" width="80pt">
						<template slot-scope="scope">
							<el-input v-model="scope.row.count" placeholder="数量">
							</el-input>
						</template>
					</el-table-column>

					<el-table-column prop="amount1" label="单价" width="80pt">
					</el-table-column>

					<el-table-column label="操作" width="80pt">
						<template slot-scope="scope">
							<el-button size="mini" type="danger" @click="handleMaterialDelete(scope.$index, scope.row)">
								删除</el-button>
						</template>
					</el-table-column>

				</el-table>
				<el-button size="mini" type="danger" @click="handleMaterialAdd()">添加</el-button>

				<el-form-item label="差旅费" />
				<el-table :data="form.travel_list" border show-summary style="width: 100%">

					<el-table-column prop="name" label="项目" width="auto">
						<template slot-scope="scope">
							<el-input v-model="scope.row.name" placeholder="项目">
							</el-input>
						</template>
					</el-table-column>

					<el-table-column prop="amount1" label="价格" width="80pt">
						<template slot-scope="scope">
							<el-input v-model="scope.row.amount1" placeholder="价格">
							</el-input>
						</template>
					</el-table-column>

					<el-table-column label="操作" width="80pt">
						<template slot-scope="scope">
							<el-button size="mini" type="danger" @click="handleTravelDelete(scope.$index, scope.row)">删除
							</el-button>
						</template>
					</el-table-column>

				</el-table>
				<el-button size="mini" type="danger" @click="handleTravelAdd()">添加</el-button>



				<el-form-item label="用量表" />
				<el-table :data="form.usage_list" border show-summary style="width: 100%">

					<el-table-column prop="name" label="Gle卡名称" width="auto">
						<template slot-scope="scope">
							<el-select v-model="scope.row.name" placeholder="名称">
								<el-option v-for="item in gleNameList" :key="item.name" :label="item.name"
									:value="item.name">
								</el-option>
							</el-select>
						</template>
					</el-table-column>

					<el-table-column prop="count" label="使用数量" width="80pt">
						<template slot-scope="scope">
							<el-input v-model="scope.row.count" placeholder="数量">
							</el-input>
						</template>
					</el-table-column>

					<el-table-column prop="time" label="时间" width="80pt">
					</el-table-column>

					<el-table-column label="操作" width="80pt">
						<template slot-scope="scope">
							<el-button size="mini" type="danger" @click="handleUsageDelete(scope.$index, scope.row)">删除
							</el-button>
						</template>
					</el-table-column>

				</el-table>
				<el-button size="mini" type="danger" @click="handleUsagelAdd()">添加</el-button>

				<el-form-item label="用户意见" style="margin-top: 10pt;">
					<el-input type="textarea" :autosize="{ minRows: 2, maxRows: 20}" placeholder="请输入内容"
						v-model="form.user_feedback">
					</el-input>
				</el-form-item>


				<el-row :gutter="24" style="margin-top: 10pt; color: #b3b3b3;">
					<el-col :span="16">
						<h5>工程师签名:</h5>
					</el-col>
					<el-col :span="4" style="text-align: right;">
					</el-col>
					<el-col :span="4" style="text-align: right;">
						<h5>日期:</h5>
					</el-col>
				</el-row>

				<el-row :gutter="24" style="margin-top: 10pt; color: #b3b3b3;">
					<el-col :span="16">
						<h5>客户签名:</h5>
					</el-col>
					<el-col :span="4" style="text-align: right;">
					</el-col>
					<el-col :span="4" style="text-align: right;">
						<h5>日期:</h5>
					</el-col>
				</el-row>
			</div>



		</el-form>
		<el-button size="mini" type="danger" @click="exportWord()">导出word</el-button>
	</div>
</template>

<script>
	export default {
		name: "orderunity",
		data() {
			return {
				problemFileList: [],
				completeFileList: [],
				userCompanyList: ['1', '2'],
				materialNameList: [{
						name: '离心机',
						price: 12.5
					},
					{
						name: '机械手电机',
						price: 19.5
					},
					{
						name: '电机',
						price: 18.5
					}
				],
				gleNameList: [{
						name: '抗人球蛋白检测卡'
					},
					{
						name: '交叉配血卡'
					},
					{
						name: 'abo卡'
					}
				],
				form: {
					id: '', //	int	工单编号
					user_company: '', //	varchar(255)	用户单位
					address: '', //	varchar(255)	地址
					phone: '', //	varchar(20)	电话
					instrument_model: '', //	varchar(50)	仪器型号
					serial_number: '', //	varchar(50)	序列号
					maintenance: '', //	text	维护内容
					repair: '', //	text	维修内容
					other: '', //	text	其他备注
					problem: '', //	text	问题点
					solution: '', //	text	处理结果
					//	text	使用物料清单
					material_list: [],
					travel_list: [], //	decimal(10,2)	差旅费用
					usage_list: [], //	text	使用量记录
					user_feedback: '', //	text	用户意见反馈
					priority: '', //	enum('紧急','高','中','低')	处理等级
					status: '', //	enum(//0待指派 1待接单 2待完成 3已完成 4未完成)	工单状态 
					assignee: '', //	varchar(50)	指派人
					repairman: '', //	varchar(50)	维修人
					created_time: '', //	datetime	创建时间
					is_daily_equipment: false, //设备日常维护
				}
			}
		},
		props: {
			showall: {
				type: Boolean,
				default: false
			},
			isedit: {
				type: Boolean,
				default: true
			},
		},
		computed: {
			compisedit() {
				return !this.isedit;
			}
		},
		methods: {
			exportWord() {

			},
			materialSelectCompany(index, data) {
				console.log(index, data);
				for (let i = 0; i < this.materialNameList.length; i++) {
					if (this.materialNameList[i].name == this.form.material_list[index].name) {
						this.form.material_list[index].amount1 = this.materialNameList[i].price;
						break;
					}
				}
			},
			handleMaterialAdd() {
				this.form.material_list.push({
					id: '0',
					name: '',
					count: 1,
					amount1: 0,
				})
			},
			handleMaterialDelete(index, data) {
				console.log(index, data);
				this.form.material_list.splice(index, 1)
			},
			handleTravelAdd() {
				this.form.travel_list.push({
					id: '0',
					name: '',
					count: 1,
					amount1: 0,
				})
			},
			handleTravelDelete(index, data) {
				console.log(index, data);
				this.form.travel_list.splice(index, 1)
			},
			handleUsagelAdd() {
				var d = new Date(); //获取系统当前时间
				this.form.usage_list.push({
					id: '0',
					name: '',
					count: 1,
					time: d.toLocaleString(),
				})
			},
			handleUsageDelete(index, data) {
				console.log(index, data);
				this.form.usage_list.splice(index, 1)
			},
			handleRemove(file, fileList) {
				console.log(file, fileList);
			},
			handlePreview(file) {
				console.log(file);
			},
			onSubmit() {
				console.log('submit!');
			}
		}
	}
</script>

<style>

</style>
