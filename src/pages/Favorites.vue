<script setup>
import axios from 'axios';

import { onMounted, ref } from 'vue';

import CardList from '../components/CardList.vue';

const favorites = ref([]);

onMounted(async () => {
	try {
		const { data } = await axios.get(
			'https://1902939eeee538b4.mokky.dev/favorites?_relations=items'
		);

		favorites.value = data.map((obj) => obj.item);
	} catch (err) {
		console.log(err);
	}
});
</script>

<template>
	<h2 class="font-bold text-3xl pb-10">Мои закладки</h2>
	<CardList :items="favorites" is-favorites />
	<div
		v-if="!favorites.length"
		class="flex flex-col items-center gap-4"
		v-auto-animate
	>
		<img class="w-20 h-20" src="/sadsmile.png" alt="sad" />
		<h2 class="font-bold text-3xl">Закладок нет :(</h2>
		<p class="text-slate-400">Вы ничего не добавляли в закладки</p>
	</div>
</template>
