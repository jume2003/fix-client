<template>
	<div class="app-container">
		<el-row :style="{ flexGrow: 1, overflowY: 'auto' }">
			<!-- 中间可滑动的区域 -->
			<OrderUnity :showall=false :isedit=true>
			</OrderUnity>
		</el-row>
		<el-row>
			<el-col :span="24" style="position: fixed; bottom: 0; width: 100%; z-index:100;">
				<!-- 底部固定的区域 -->
				<el-form style="background-color: white;">
					<el-button type="danger" @click="handlePost()" style="text-align: center;">发布工单</el-button>
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

			}
		},
		methods: {
			handlePost() {
				uni.reLaunch({
					url: '/pages/tabbar/orders/orders?filter=0'
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

	.el-form {
		display: flex;
		justify-content: center;
	}

	.el-button {
		margin: auto;
	}
</style>
