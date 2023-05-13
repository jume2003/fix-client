<template>
	<div class="app-container">
		<el-table :data="tableData" style="width: 100%">
			<el-table-column prop="name" label="名称">
			</el-table-column>
			<el-table-column prop="stock" label="库存">
			</el-table-column>
			<el-table-column prop="number" label="编号">
			</el-table-column>
			<el-table-column prop="price" label="价格">
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
				<el-form-item label="名称" prop="name">
					<el-input v-model="editForm.name"></el-input>
				</el-form-item>
				<el-form-item label="库存" prop="stock">
					<el-input v-model="editForm.stock"></el-input>
				</el-form-item>
				<el-form-item label="编号" prop="number">
					<el-input v-model="editForm.number"></el-input>
				</el-form-item>
				<el-form-item label="价格" prop="price">
					<el-input v-model="editForm.price"></el-input>
				</el-form-item>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="dialogVisible = false">取消</el-button>
				<el-button type="primary" @click="saveEdit">保存</el-button>
			</div>
		</el-dialog>

		<el-dialog :visible.sync="addDialogVisible" title="添加新物料" :close-on-click-modal="false">
			<el-form :model="addForm" :rules="addFormRules" ref="addForm" label-width="80px">
				<el-form-item label="名称" prop="name">
					<el-input v-model="addForm.name"></el-input>
				</el-form-item>
				<el-form-item label="库存" prop="stock">
					<el-input v-model="addForm.stock"></el-input>
				</el-form-item>
				<el-form-item label="编号" prop="number">
					<el-input v-model="addForm.number"></el-input>
				</el-form-item>
				<el-form-item label="价格" prop="price">
					<el-input v-model="addForm.price"></el-input>
				</el-form-item>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="addDialogVisible = false">取消</el-button>
				<el-button type="primary" @click="saveAdd">添加</el-button>
			</div>
		</el-dialog>
		<el-button style="margin-top: 20px;" type="primary" @click="addDialogVisible = true">添加新物料</el-button>

	</div>
</template>

<script>
	export default {
		data() {
			return {
				tableData: [{
					name: '机械手',
					stock: 100,
					number: 1001,
					price: 0
				}],
				dialogVisible: false,
				addDialogVisible: false,
				editFormIndex: 0,
				editForm: {
					name: '',
					stock: 0,
					number: 0,
					price: 0,
				},
				addForm: {
					name: '名称',
					stock: 0,
					number: 0,
					price: 0,
				},
				editFormRules: {
					name: [{
						required: true,
						message: '请输入物料名称',
						trigger: 'blur'
					}],
					stock: [{
						required: true,
						message: '请输入物料库存',
						trigger: 'blur'
					}],
					number: [{
						required: true,
						message: '请输入物料编号',
						trigger: 'blur'
					}],
					price: [{
						required: true,
						message: '请输入物料价格',
						trigger: 'blur'
					}]
				},
				addFormRules: {
					name: [{
						required: true,
						message: '请输入物料名称',
						trigger: 'blur'
					}],
					stock: [{
						required: true,
						message: '请输入物料库存',
						trigger: 'blur'
					}],
					number: [{
						required: true,
						message: '请输入物料编号',
						trigger: 'blur'
					}],
					price: [{
						required: true,
						message: '请输入物料价格',
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
				this.$confirm('确认删除此物料？')
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
							name: '物料',
							stock: 0,
							number: 0,
							price: 0,
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
