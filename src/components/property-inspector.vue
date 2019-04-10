<template>
	<div class="q-title text-center" :class="title ? '': 'text-light'">
		{{title || 'No title'}}
		<q-popup-edit title="Edit name chart" buttons v-show="hasEdit">
			<q-input type="text" :value="title" @change="changeTitle($event)" />
		</q-popup-edit>
	</div>
</template>

<script>
export default {
	props: ['title', 'index', 'hasEdit'],
	methods: {
		changeTitle(e) {
			this.$root.$emit('changeTitle', e, this.index)
		}
	}
}
</script>

<style>
</style>