<script setup>
defineProps({
	totalPrice: Number,
});

const emit = defineEmits(['openDrawer']);
</script>

<template>
	<header class="flex justify-between items-center">
		<router-link to="/">
			<div class="flex items-center gap-4">
				<img class="w-12 h-12" src="/logo.png" alt="Logo" />
				<div class="">
					<h2 class="uppercase font-bold text-xl">Store Sneakers</h2>
					<p class="font-normal text-sm text-gray-300">
						Магазин лучших кроссовок
					</p>
				</div>
			</div>
		</router-link>

		<nav>
			<ul class="flex gap-10">
				<li
					@click="() => emit('openDrawer')"
					class="flex items-center gap-2 cursor-pointer text-gray-500 hover:text-black"
				>
					<img src="/cart.svg" alt="Cart" />
					<p class="font-semibold text-sm">{{ totalPrice }} руб</p>
				</li>
				<router-link to="/favorites">
					<li
						class="flex items-center gap-2 cursor-pointer text-gray-500 hover:text-black"
					>
						<img src="/heart.svg" alt="Cart" />
						<p class="text-sm font-semibold">Избранное</p>
					</li>
				</router-link>

				<router-link to="/shopping">
					<li
						class="flex items-center gap-2 cursor-pointer text-gray-500 hover:text-black"
					>
						<img src="/profile.svg" alt="Cart" />
						<p class="text-sm font-semibold">Профиль</p>
					</li>
				</router-link>
			</ul>
		</nav>
	</header>
</template>
