<script setup>
const props = defineProps({
	id: Number,
	title: String,
	imageUrl: String,
	price: Number,
	isFavorite: Boolean,
	isAdd: Boolean,
	onClickFavorite: Function,
	onClickAdd: Function,
});
</script>

<template>
	<div
		class="relative bg-white border border-slate-400 rounded-3xl p-4 cursor-pointer transition hover:-translate-y-2 hover: shadow-xl flex flex-col gap-2 w-72"
	>
		<img
			v-if="onClickFavorite"
			class="absolute"
			@click="onClickFavorite"
			:src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
			alt="like"
		/>

		<img class="w-64 h-56" :src="imageUrl" alt="sneaker" />
		<p class="text-sm font-medium">{{ title }}</p>

		<div class="flex justify-between mb-auto">
			<div class="flex flex-col">
				<p class="text-slate-400 uppercase">Цена:</p>
				<p class="text-black font-bold">{{ price }} руб.</p>
			</div>
			<img
				v-if="onClickFavorite"
				@click="onClickAdd"
				:src="!isAdd ? '/plus.svg' : '/checked.svg'"
				alt="check"
			/>
		</div>
	</div>
</template>
