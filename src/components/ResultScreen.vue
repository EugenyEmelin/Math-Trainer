<template>
	<div class="alert alert-primary">
		<h3>Правильные ответы: {{ stats.success }}</h3>
		<h3 style="color:#a12123">Ошибки: {{ stats.error }}</h3>
		<hr>
		<button class="btn btn-primary" @click="$emit('repeat')">Повтор</button>
		<button class="btn btn-primary" @click="$emit('nextLevel')">Следующий уровень</button>
	</div>
</template>
<script>
	export default {
		props: ['stats']
	}
</script>
<style>
	
</style>