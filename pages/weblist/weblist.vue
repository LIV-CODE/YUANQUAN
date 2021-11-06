<template>
	<view>
		<web-view :src="weatl.aurl"></web-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				weatl:[]
			}
		},
		onLoad(e) {
			this.weatl = e
			console.log(e.aurl)
			console.log(this.weatl)
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
