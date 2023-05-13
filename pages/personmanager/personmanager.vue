<template>
	<div class="app-container">
		<el-table :data="tableData" style="width: 100%">
			<el-table-column prop="name" label="人员">
			</el-table-column>
			<el-table-column prop="status" label="状态">
			</el-table-column>
			<el-table-column prop="to_be_repaired" label="待维修">
			</el-table-column>
			<el-table-column prop="being_repaired" label="维修中">
			</el-table-column>
			<el-table-column label="操作">
				<template slot-scope="scope">
					<el-row>
						<el-button type="primary" size="small" @click="handleEdit(scope.$index, scope.row)">编辑
						</el-button>
					</el-row>
					<el-row>
						<el-button type="danger" size="small" @click="handleDelete(scope.$index)">删除</el-button>
					</el-row>
				</template>
			</el-table-column>
		</el-table>

		<el-dialog :visible.sync="dialogVisible" title="编辑" :close-on-click-modal="false">
			<el-form :model="editForm" :rules="editFormRules" ref="editForm" label-width="80px">
				<el-form-item label="人员" prop="name">
					<el-input v-model="editForm.name"></el-input>
				</el-form-item>
				<el-form-item label="状态" prop="status">
					<el-input v-model="editForm.status"></el-input>
				</el-form-item>
				<el-form-item label="待维修" prop="to_be_repaired">
					<el-input v-model="editForm.to_be_repaired"></el-input>
				</el-form-item>
				<el-form-item label="维修中" prop="being_repaired">
					<el-input v-model="editForm.being_repaired"></el-input>
				</el-form-item>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="dialogVisible = false">取消</el-button>
				<el-button type="primary" @click="saveEdit">保存</el-button>
			</div>
		</el-dialog>

		<el-dialog :visible.sync="addDialogVisible" title="添加新人员" :close-on-click-modal="false">
			<el-form :model="addForm" :rules="addFormRules" ref="addForm" label-width="80px">
				<el-form-item label="人员" prop="name">
					<el-input v-model="addForm.name"></el-input>
				</el-form-item>
				<el-form-item label="状态" prop="status">
					<el-input v-model="addForm.status"></el-input>
				</el-form-item>
				<el-form-item label="待维修" prop="to_be_repaired">
					<el-input v-model="addForm.to_be_repaired"></el-input>
				</el-form-item>
				<el-form-item label="维修中" prop="being_repaired">
					<el-input v-model="addForm.being_repaired"></el-input>
				</el-form-item>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="addDialogVisible = false">取消</el-button>
				<el-button type="primary" @click="saveAdd">添加</el-button>
			</div>
		</el-dialog>
		<el-button style="margin-top: 20px;" type="primary" @click="addDialogVisible = true">添加新人员</el-button>

	</div>
</template>

<script>
	export default {
		data() {
			return {
				tableData: [{
						name: '张三',
						status: '空闲',
						to_be_repaired: 1,
						being_repaired: 0
					},
					{
						name: '李四',
						status: '维修中',
						to_be_repaired: 1,
						being_repaired: 0
					}
				],
				dialogVisible: false,
				addDialogVisible: false,
				editFormIndex: 0,
				editForm: {
					name: '',
					status: '空闲',
					to_be_repaired: 0,
					being_repaired: 0,
				},
				addForm: {
					name: '老六',
					status: '空闲',
					to_be_repaired: 0,
					being_repaired: 0,
				},
				editFormRules: {
					name: [{
						required: true,
						message: '请输入人员名称',
						trigger: 'blur'
					}]
				},
				addFormRules: {
					name: [{
						required: true,
						message: '请输入人员名称',
						trigger: 'blur'
					}]
				}
			};
		},
		methods: {
			handleEdit(index, row) {
				this.editForm = Object.assign({}, row);
				this.editFormIndex = index;
				this.dialogVisible = true;
			},
			saveEdit() {
				this.$refs.editForm.validate(valid => {
					if (valid) {
						console.log(this.editForm)
						console.log(this.editFormIndex)
						console.log(this.tableData[this.editFormIndex])
						Object.assign(this.tableData[this.editFormIndex], this.editForm);
						this.dialogVisible = false;
						this.$message.success('保存成功');
					}
				});
			},
			handleDelete(index) {
				this.$confirm('确认删除此人员？')
					.then(_ => {
						this.tableData.splice(index, 1);
						this.$message.success('删除成功');
					})
					.catch(_ => {});
			},
			saveAdd() {
				this.$refs.addForm.validate(valid => {
					if (valid) {
						this.tableData.push(this.addForm);
						this.addForm = {
							name: '老六',
							status: '空闲',
							to_be_repaired: 0,
							being_repaired: 0,
						}
						this.addDialogVisible = false;
						this.$message.success('添加成功');
					}
				});
			}
		}
	};
</script>
<style>
	.app-container {
		display: flex;
		flex-direction: column;
		height: 100%;
	}
</style>
