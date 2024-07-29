<script setup>
const emit = defineEmits(['onClickRemove']);

const props = defineProps({
	id: Number,
	title: String,
	imageUrl: String,
	price: Number,
});
</script>

<template>
	<div class="border border-slate-100 rounded-3xl p-4 flex gap-4 items-center">
		<img class="w-16 h-16" :src="imageUrl" alt="" />

		<div class="flex justify-between flex-col flex-1">
			<p class="text-sm">{{ props.title }}</p>
			<div class="flex justify-between">
				<p class="font-bold flex-1">{{ props.price }} руб.</p>
				<img
					@click="emit('onClickRemove')"
					class="cursor-pointer opacity-50 hover:opacity-100 transition"
					src="/close.svg"
					alt="closed"
				/>
			</div>
		</div>
	</div>
</template>
