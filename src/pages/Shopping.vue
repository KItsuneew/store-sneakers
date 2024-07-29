<script setup>
import axios from 'axios';

import { onMounted, ref } from 'vue';

import CardList from '../components/CardList.vue';

const shopping = ref([]);

onMounted(async () => {
	try {
		const { data } = await axios.get(
			'https://1902939eeee538b4.mokky.dev/orders'
		);

		shopping.value = data.flatMap((order) => order.items);
	} catch (err) {
		console.log(err);
	}
});
</script>

<template>
	<h2 class="font-bold text-3xl pb-10">Мои покупки</h2>
	<CardList :items="shopping" is-favorites />
	<div
		v-if="!shopping.length"
		class="flex flex-col items-center gap-4"
		v-auto-animate
	>
		<img class="w-20 h-20" src="/image10.png" alt="smile" />
		<h2 class="font-bold text-3xl">У вас нет заказов</h2>
		<p class="text-slate-400">Вы нищеброд? Оформите хотя бы один заказ.</p>
	</div>
</template>
