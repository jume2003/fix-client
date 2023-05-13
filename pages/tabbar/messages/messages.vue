<template>
	<view>
		<OrderUnity :showall=true :isedit=true>

		</OrderUnity>
	</view>
</template>

<script>
	import OrderUnity from '../../../components/orderunity.vue'
	export default {
		components: {
			OrderUnity,
		},
		data() {
			return {
				title: 'Hello'
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style>
	.content {
		text-align: center;
		height: 400upx;
		margin-top: 200upx;
	}
</style>
