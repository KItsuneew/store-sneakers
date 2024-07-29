<script setup>
defineProps({
	title: String,
	imageUrl: String,
	description: String,
});
</script>

<template lang="">
	<div class="flex flex-col items-center text-center gap-4 m-72 mx-auto">
		<img class="w-20 h-20" :src="imageUrl" alt="inf" />
		<h2 class="text-2xl font-bold">{{ title }}</h2>
		<p class="text-slate-400">{{ description }}</p>
	</div>
</template>
