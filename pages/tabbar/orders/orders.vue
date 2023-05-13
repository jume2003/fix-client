<template>
	<div>
		<OrderList :filter="filter"> </OrderList>
	</div>
</template>

<script>
	import OrderList from '../../../components/orderlist.vue'
	export default {
		components: {
			OrderList,
		},
		data() {
			return {
				filter: 0,
			}

		},
		computed: {

		},
		onLoad(e) {
			this.filter = parseInt(e.filter)
		},
		methods: {

		}
	}
</script>

<style>

</style>
